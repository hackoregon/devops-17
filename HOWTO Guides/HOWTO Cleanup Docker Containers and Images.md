# Summary

Docker makes it super-simple to import new images and create new containers from an existing image.

Docker is less great about cleaning up all the containers and images it leaves behind after you're done with them.

# Removing currently-used Docker Containers
Find all currently-running containers:

```
docker ps
```

Take note of the container ID for the containers that are running e.g. e7759a471783

Then run docker rm [CONTAINER ID] e.g.:

```
docker rm e7759a471783
```

# Cleaning up all unused Docker Containers
```
docker rm $(docker ps -a -q)
```

# Cleaning up all unused Docker Images
```
docker rmi $(docker images -q)
```
