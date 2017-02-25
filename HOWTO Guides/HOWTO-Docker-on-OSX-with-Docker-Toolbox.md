How to use Docker on OS X (MacOS) with the Docker Toolbox

TL;DR enable a Bridged Network adapter and always access the dockerized apps via http://[docker_machine_IP]:[app_port] rather than the usual http://localhost:[app_port]

Using native Docker on modern MacBooks (most every MacBook purchased in the last few years) is relatively straightforward and operates very easily.

However, there are some Macbooks (like mine) that are too old to run the native Docker engine in the OS - any MacBook pre-2010 doesn't support the "MMU virtualization" (that wasn't introduced until the Intel CPUs that were first incorporated in 2010-generation Macbooks).

For such lowlifes like me, Docker has released the Docker Toolbox - which is basically the Docker command-line tools + a Virtualbox machine that hosts the Docker engine inside the VM.  It's a little bit of a kludge, but commands like docker-compose work like a charm, and in practice most of the operations work the same as native Docker.

# One Small Problem: Networking
What *doesn't* work like a charm is the networking.  Of course - Docker is actually a US Marines bootcamp in networking, disguised as a lightweight app isolation engine.

When you run the Docker Toolbox that configures NAT on the 1st Network Adapter, you may experience errors like these:

```
$ docker-machine env
Error checking TLS connection: Error checking and/or regenerating the certs: There was an error validating certificates for host "192.168.99.100:2376": dial tcp 192.168.99.100:2376: getsockopt: connection refused
```

```
$ docker ps
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
```

And when you start the `docker Quickstart Terminal` you'll notice the **refused** message amongst the output:

```
Starting "default"...
(default) Check network to re-create if needed...
(default) Waiting for an IP...
Machine "default" was started.
Waiting for SSH to be available...
Detecting the provisioner...
Started machines may have new IP addresses. You may need to re-run the `docker-machine env` command.
Regenerate TLS machine certs?  Warning: this is irreversible. (y/n): Regenerating TLS certificates
Waiting for SSH to be available...
Detecting the provisioner...
Copying certs to the local machine directory...
Copying certs to the remote machine...
Setting Docker configuration on the remote daemon...
Unable to verify the Docker daemon is listening: Maximum number of retries (10) exceeded
Error checking TLS connection: Error checking and/or regenerating the certs: There was an error validating certificates for host "192.168.99.100:2376": dial tcp 192.168.99.100:2376: getsockopt: connection refused
You can attempt to regenerate them using 'docker-machine regenerate-certs [name]'.
Be advised that this will trigger a Docker daemon restart which might stop running containers.
```

# One small solution: reconfigure networking in Virtualbox
If you're experiencing these kinds of issues, reconfigure the virtual machine to use a Bridged Adapter:
- launch the **Virtualbox** app
- right-click on the machine labelled "default" and choose **Close** and **ACPI Shutdown** (or from a **Terminal** window run `docker-machine stop default`)
- click the **Settings** button and choose the **Network** tab
- Configured Adapter 3 as a **Bridged Adapter** (this article)[http://stackoverflow.com/questions/33021581/how-to-bind-the-vm-docker-machine-creates-to-osx-ip-address#33576212] explains why you can't reconfigure Adapter 1 from NAT to Bridged
- from a Terminal window, run `docker-machine start default`
