HOWTO Troubleshoot backend container health in CloudFormation EC2 Container Service

# Problem: New task definition not running within 90 seconds

The `ecs-deploy.sh` script can often result in a fine-looking deploy (image is available at expected location, task definition version tag gets incremented) but that the containerized app doesn't spin up in a reasonable amount of time. e.g.

```
Running ecs-deploy.sh script...
Using image name: 845828040396.dkr.ecr.us-west-2.amazonaws.com/integration/budget-service:latest
Current task definition: arn:aws:ecs:us-west-2:845828040396:task-definition/budget-service:36
New task definition: arn:aws:ecs:us-west-2:845828040396:task-definition/budget-service:37
ERROR: New task definition not running within 90 seconds
```

# Possible causes

## Container takes longer to start than CloudFormation Health check retries
We had everything we could think of fixed up for the Emergency Response backend deploy, and it still wasn't starting correctly (no response to web clients or CloudFormation Health check).  Finally Dan looked and said, "I increased the number of tries from 2 to 5 on the health check.  Health check was marking service unhealthy since the service take more than 40 seconds to start up."  The fix was in [this commit](https://github.com/hackoregon/hackoregon-aws-infrastructure/commit/5018b636084ead67e663579d56de8a0de7ead2e4).

## PROJ_SETTINGS_DIR - TaskDefinition vs. application configuration

e.g. Budget backend PROJ_SETTINGS_DIR = "budget_proj", but CF TaskDefinition (`master.yaml`) was "budgetAPI" - they need to match

## ALLOWED_HOSTS - needs to include specific address for CF Load Balancer

Typical configuration looks like `ALLOWED_HOSTS = ['127.0.0.1','localhost','hacko-integration-658279555.us-west-2.elb.amazonaws.com']`

## What route is the load balancer expecting?

In the service's `service.yaml`, under Parameters: Path, the Default path should match the default route configured in `urls.py` for the overall application.

e.g. when `service.yaml` configures `Parameters: Path: Default: emergency`, then the Django app's `urls.py` should include urlpatterns configuration like the following (note the `url(r'^emergency/'` string and not just `url(r'/'`):

```
urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^emergency/', include('data.urls', namespace='emergency'))
```

## Does the container return 200 HTTP response to the default route?

Try requesting an HTTP response from your default route: `curl -v localhost:8000/emergency/`

Does it return a response such as the following:

```
*   Trying localhost...
* Connected to localhost (127.0.0.1) port 8000 (#0)
> GET /emergency/ HTTP/1.1
> Host: 127.0.0.1:8000
> User-Agent: curl/7.43.0
> Accept: */*
>
< HTTP/1.1 200 OK
...
```

## Does your Config bucket have the correct project_config.py?

Download the `project_config.py` from your project's Config bucket in S3 (from the correct folder e.g. "integration"), and verify it contains the correct values for the env vars included.

## Does your Dockerfile include an ENTRYPOINT?

The Dockerfile used to build the deployed container image must contain an ENTRYPOINT command, and that must be a command that results in a running application.  Without this, the container will start, but no application will be running in the background to receive incoming requests.  (i.e. the container is not running via `docker-compose` when the container runs in ECS)



# Other deployment-related issues

## 403 Forbidden error when running docker login command

Issue: when using `docker login ...`, passing in issued credentials, those login tokens will expire after 12 hours.

Solution: instead of this command:
` docker login -u "$DOCKER_USERNAME" -p "$DOCKER_PASSWORD" "$DOCKER_REPO"`

Try this command:
`eval $(aws ecr get-login --region $AWS_DEFAULT_REGION)`
