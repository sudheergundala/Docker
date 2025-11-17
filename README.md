### Docker and Docker Compose
---------

## Docker

---------

It is a platform for Building, Running and Managing Containers.

 ## Container:-
It is a lightweight, portable, isolated and self sufficient software package that includes everything an application needs to run(its code, dependencies,system libraries,runtime and settings). 

## Virtual Machine(VM) vs Container

![VirtualMachine vs Containers](VMvsContainer.png)

## Docker Architecture

![Docker Architecture](image.png)


Docker CLI ---> DockerD ----> ContainerD ---> Containerd-shim ----> runc

Docker CLI:-

docker CLI communicates with Docker daemon via unix socket(/var/run/docker.sock) using the docker REST APIs if both the docker cli and docker daemon are on the same host.

docker cli communicates with docker daemon via TCP://0.0.0.0:2376 using REST APIs if both are on remote setup. But this very rare now as the orchestrators came to picture they are using the run time to decide how and where to deploy the containers. i.e locally they are building the images and pushing into repo and the runtimes are pulling the images and runnig as containers.

Docker Daemon (dockerd):-

dockerd analyze the Dockerfile or image, setting up the mounts, network config, volumes etc and creats the container's OCI bundle(config.json + rootfs). Then talks to Containerd runtime via gRPC to create and start the container. Dockerd applies the docker specific logic.

Containerd :-

The Containerd manages the containers as "Tasks". Once it receives the call from Dockerd it initiates the lightweight , dedicated process to manage and interact with that specific container. the process is called continerd-shim

Containerd-shim:-

The Shim then setup the STDIO(STDIN,STDOUT,STDERR) to the appropriate streams to collect the output from containers to containerd.
Also setup the runtime environment and prepares the filesystem.ie.mounts the container root filesystem, setup the namespaces(PID,mount,network etc), configure cgroups(CPU,memory etc).
Now calls the RUNC by executing the command runc run <container-id>

Containerd-shim will be there till the container exists as it the one which will monitor and allow the STDIO operations. it will give the status of the container to containerd. so if the containerd crashes also it will not effect the running container. once the containerd is up and back again it will get the container information about the running containers from containerd-shim and update accordingly.

if the containerd-shim crashes, then the running container will become orphan process. So the contianerd wont be able to find them out.

runc:- 
 runc then reads the OCI spec created by dockerd and execute the syscall clone() to create 
   1. namespaces
   2. cgroups
   3. apply rootfilesystem.
   4. Network

   once the process is created then the runc exits and the shim will takecare of the container.

## namespaces:-

## Cgroups:-
   



  



