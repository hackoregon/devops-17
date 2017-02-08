How to use Docker on OS X (MacOS) with the Docker Toolkit

TL;DR enable a Bridged Network adapter and always access the dockerized apps via http://[docker_machine_IP]:[app_port] rather than the usual http://localhost:[app_port]

Using native Docker on modern MacBooks (most every MacBook purchased in the last few years) is relatively straightforward and operates very easily.

However, there are some Macbooks (like mine) that are too old to run the native Docker engine in the OS - any MacBook pre-2010 doesn't support the "MMU virtualization" (that wasn't introduced until the Intel CPUs that were first incorporated in 2010-generation Macbooks).

For such lowlifes like me, Docker has released the Docker Toolbox - which is basically the Docker command-line tools + a Virtualbox machine that hosts the Docker engine inside the VM.  It's a little bit of a kludge, but commands like docker-compose work like a charm, and in practice most of the operations work the same as native Docker.

What *doesn't* work like a charm is the networking.  Of course - Docker is actually a US Marines bootcamp in networking, disguised as a lightweight app isolation engine.



NAT

Add a Bridged Adapter
- run the Virtualbox app
- right-click on the machine labelled "default" and choose "Close" and "ACPI Shutdown"
- click the Settings button and choose the Networking tab
- 




http://stackoverflow.com/questions/33021581/how-to-bind-the-vm-docker-machine-creates-to-osx-ip-address#33576212
