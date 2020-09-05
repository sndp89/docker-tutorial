# Docker 

Welcome to Docker basics. Understanding and using docker is essential before you start to use our GPU Sandbox environment, where you can run deep learning algorithms/models on the NVIDIA GPU's! 

So, it is worth spending some time learning docker, because config changes or installations to the GPU box is prohibited! That includes python packages! Lynda.com has a lot of good introductions, and so does youtube! https://www.lynda.com/search?q=Docker

Quick funny video of Hitler using Docker : https://www.youtube.com/watch?v=PivpCKEiQOQ



 ## What is Docker ? 

Docker is an open platform for developing, shipping and running applications and enables you to package and run an application/model in an isolated environment called a container. The NVIDIA DGX-1™ , NVIDIA DGX Station™ , and NVIDIA® GPU Cloud™ (NGC) family system is are designed to run containers. Containers hold the application as well as any libraries or code that are needed to run the application. Containers are portable within an operating system family. For example, you can create a container using Red Hat Enterprise Linux and run it on an Ubuntu system, or vice versa. The only common thread between the two operating systems is that they each need to have the container software so they can run containers.

## What are Docker Images and Containers ?

An image is an executable package that includes everything needed to run an application- code, model, a runtime, libraries, python packages, spark libs, environment variables and config files.

A container is launched by running an image. A container is a runtime instance of an image basically what the image becomes in memory when executed. if you are familiar with object oriented programming, think of Image as a class and container as an object where object is an instance of a class. Likewise, container is an instance of an image.

![Docker Image & container relationship](https://i.stack.imgur.com/vGuay.png)

## Why do we need docker for GPU ?

We need docker because GPU configurations are very tricky to get right. The required system configuration and installations from project to project can vary greatly, and Docker allows us to all play nicely together. When done correctly, there are NO INSTALLATIONS NEEDED on the GPU box when using docker.

Using docker containers allows you to create the software/build model/algorithm on whatever OS you are confortable with and then run the application in an isolated environment. It also allows you to share the application/model with other users without having to rebuld the application/model on the OS they are using.

Containers are not a VM(virtual machine). A VM has a complete operating system and possibly applications and data files. Containers do not contain a complete operating system. They only contain the software needed to run the application/model. The container relies on the host OS for things such as file system services, networking, and an OS kernel. The application/model in the container will always run the same anywhere, regardless of the OS/compute environment.

nvidia-docker is an open-source project that provides a command line tool to mount the user-mode components of the NVIDIA driver and the GPUs into the Docker container at launch.

## How do I build a docker image ?

 Docker builds images automatically by reading the instructions from a Dockerfile -- a text file that contains all commands, in specific order, needed to build a given image.
 
 A Dockerfile is a simple text file that contains all the commands a user could call on the command line to assemble an image.
 
 A Dockerfile is a text file with instructions written in a format understood by the Docker daemon (Docker daemon is one of the Docker components, that does the heavy-duty tasks of building, running, and distributing the Docker images, after getting commands from the Docker client). To create an image, you need to write your own Dockerfile. But don’t worry, there are only a handful of Dockerfile configuration options (called instructions).
 
 Dockerfile Instructions:
 
         FROM   This instruction is used to set the base image for subsequent instructions
         MAINTAINER   This is a non-executable instruction used to indicate the author of the Dockerfile.MAINTAINER <name>.
         RUN   This instruction lets you execute a command on top of an existing layer and create a new layer with the results of command execution
         CMD   The major difference between CMD and RUN is that CMD doesn’t execute anything during the build time. It just specifies the intended command for the image. Whereas RUN actually executes the command during build time.There can be only one CMD instruction in a Dockerfile, if you add more, only the last one takes effect
         LABEL    You can assign metadata in the form of key-value pairs to the image using this instruction. e.g. LABEL version="1.0" description="This is a sample desc"
         EXPOSE    While running your service in the container you may want your container to listen on specified ports
         ENV    This instruction can be used to set the environment variables in the container
         COPY    This instruction is used to copy files and directories from a specified source to a destination (in the file system of the container)
         ADD    This instruction is similar to the COPY instruction with few added features like remote URL support in the source field and local-only tar extraction
         ENTRYPOINT    You can use this instruction to set the primary command for the image
         VOLUME    You can use the VOLUME instruction to enable access to a location on the host system from a container.Just pass the path of the location to be accessed
         USER    This is used to set the UID (or username) to use when running the image
         WORKDIR    This is used to set the currently active directory for other instructions such as RUN, CMD, ENTRYPOINT, COPY and ADD
         ONBUILD    This instruction adds a trigger instruction to be executed when the image is used as the base for some other image. It behaves as if a RUN instruction is inserted immediately after the FROM instruction of the downstream Dockerfile. This is typically helpful in cases where you need a static base image with a dynamic config value that changes whenever a new image has to be built (on top of the base image).e.g.ONBUILD RUN rm -rf /usr/temp
     
 For example, lets say we want to create a docker image that has python, tensorflow, sklearn and enable the ability to pip install. We first create the following Dockerfile:
        
    datascience@ebidgx1:~/projects/demo/trial_Dockerfilecreation$ vi Dockerfile 
    
    Once the Dockerfile is created , insert all your instructions you want as below:
    
        FROM ubuntu
        RUN apt-get update
        RUN apt-get install -y python
        RUN apt-get install -y python-pip
        RUN pip install --upgrade pip
        RUN pip install tensorflow
        RUN pip install sklearn
        CMD ["echo","Image created successfully"]
        
 Then an image can be built using the syntax:
 
        docker build  -t ImageName:TagName dir
        
   where:
   
        -t is to mention a tag to the image
        ImageName − This is the name you want to give to your image.
        TagName − This is the tag you want to give to your image.
        Dir − The directory where the Docker File is present.
        
so lets do:

        docker build -t demo_image:0.1 /home/datascience/projects/demo/trial_Dockerfilecreation/

 Once the above command is run, we can see the image created when you do : 
 
        docker image ls
                
 Which list all the images that exists on our computer/GPU.
 
 Following are the list of commonly used docker commands which can be like a cheat sheet(these commands can be obtained when you just type "docker" on the command prompt/shell of the computer/GPU:
 
 
## Popular commands:
    
    docker images    lists all images
    docker ps -a     lists all containers that are open
    docker rm        manually remove a container using its ID
    docker rmi       manually remove an image from the local machine (this just saves space)
    docker run       runs a container from an image
    docker commmit   turns a container into a new image
    docker tag       tags the image with something like a version, latest, etc
    docker push      pushes an image to repository - we should only push to hub.comcast.net/ebi_ds/

## All basic Commands:

    attach      Attach local standard input, output, and error streams to a running container
    build       Build an image from a Dockerfile
    commit      Create a new image from a container's changes
    cp          Copy files/folders between a container and the local filesystem
    create      Create a new container
    deploy      Deploy a new stack or update an existing stack
    diff        Inspect changes to files or directories on a container's filesystem
    events      Get real time events from the server
    exec        Run a command in a running container
    export      Export a container's filesystem as a tar archive
    history     Show the history of an image
    images      List images
    import      Import the contents from a tarball to create a filesystem image
    info        Display system-wide information
    inspect     Return low-level information on Docker objects
    kill        Kill one or more running containers
    load        Load an image from a tar archive or STDIN
    login       Log in to a Docker registry
    logout      Log out from a Docker registry
    logs        Fetch the logs of a container
    pause       Pause all processes within one or more containers
    port        List port mappings or a specific mapping for the container
    ps          List containers
    pull        Pull an image or a repository from a registry
    push        Push an image or a repository to a registry
    rename      Rename a container
    restart     Restart one or more containers
    rm          Remove one or more containers
    rmi         Remove one or more images
    run         Run a command in a new container
    save        Save one or more images to a tar archive (streamed to STDOUT by default)
    search      Search the Docker Hub for images
    start       Start one or more stopped containers
    stats       Display a live stream of container(s) resource usage statistics
    stop        Stop one or more running containers
    tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
    top         Display the running processes of a container
    unpause     Unpause all processes within one or more containers
    update      Update configuration of one or more containers
    version     Show the Docker version information
    wait        Block until one or more containers stop, then print their exit codes
 
 
## Can i just pull and use images that are already built rather than building a new image ?

  Yes, Ofcourse. That is what I would normally prefer doing as there are more than 500K public images already built which can be pulled onto our computer/GPU that includes images with most deep learning frameworks and softwares already installed.

If we need to install more unpopular packages/libraries, we can pull existing public image to spin up a container and continue to install more extra libraries we want from inside the container. We can search for public images by:

                docker search [keyword]
            
            like e.g.
            
                docker search keras
                
      shows all images that have keras and I usually pull the one that shows the maxiumum number of stars.
      
   If you want to pull and use a specific image from the hub , just do 
   
                docker pull [REPO NAME]
                docker pull tensorflow/tensorflow
                
   If you pulled something by mistake and want to delete it, for example:
   
                docker rmi tensorflow/tensorflow
             
## How Do I launch a container using an image ?

  A container is launched by running an image. For example, We can do so by the command:
  
    nvidia-docker run -ti --rm --shm-size=8g --ulimit memlock=-1 -p 8888:8888 -v /home/datascience/projects/demo/:/demo demo_image:0.1 bash
    
  where we use:
 
       nvidia as prefix for docker as we are running on nvidia GPU
       --rm helps to automatially stop/kill container once the process is done and exited
       --shm-size is shared memory size in container
       -p is the port. We can map a port of the container, to a port on the host machine. We map them together with the colon (:) 
       memlock is used to set the max amount of locked-in-memoey address space
       ulimit provides control over resources available to each user via a shell
       -v is the mounted volumes. Here, we can map a volume on the host directory to a mount inside the container. In this case we use /home/datascience/projects/demo/ so that we can reference all these files from within the container
       /demo this dir is the path inside the container where we want to push our files/data
       demo_image is the image repository that corresponds to an image ID. A repo can have multiple image versions(tagged as image ID's).
       0.1 is the tag for the image. we can even use versioning names here like 1.0,1.1, etc..
       -ti means interactive
       bash means that we want to run bash immediately after starting the container. Some containers have an automatic execution script that runs, and bash gets us around that, allowing us to explore the container prior to running anything.
  
  
Once you are inside the container you will see something like :

       root@1a24da677b12:/#
       
Here, for exampe,  you can "cd /demo" to see the files/data you referenced to this container. Here you can run your code/model/algorithm whatever you want. It is generally a good idea to run your code/command in the format:

          nohup [command] &
          
          for example,  nohup python run.py &
 
 This helps to save all your output in nohup.out file inside your path even if your terminal is killed. One more thing to remember is whatever files you add to this path inside your container will sync up with the path on GPU/computer from where you referenced these files. In my example, So all changes made to /demo will sync up with /home/datascience/projects/sandeep/
 
 Once all your process is done , you can exit the container and it will automatially kill since you included --rm while launching it.
 
          root@1a24da677b12:/demo# exit
 
 ## EBI docker registry
 
  We have a private EBI registry to which we can commit our images if we want to. We have to login using our NT ID from our GPU to be able to commit the images we create. This is completely optional to commit and push your images to this repo.
  
          docker login hub.comcast.net
          Username: 
          Password:
         
  ## How do I get inside an already running container ?
  
  If you created a container and your terminal got killed (eventhough your process may still be running). You can still log into the GPU and get inside your container:
  
          docker exec -it [CONTAINER ID] /bin/bash
          
  You can obtain your container ID by the command:
  
          docker ps -a
          
  This will list all running and inactive containers that are not deleted.
  If you still see a running container that needs to be killed, you can:
 
          docker stop [CONTAINER ID]
  
  ## Run a jupyter instance from inside the container
  
  You need to use an image that has jupyter installed. May be you could use the image: jupyter/tensorflow-notebook
  Then once inside the container, run the following:
  
          jupyter notebook --no-browser --ip=`http://ebidgx1.cable.comcast.com` --port=8888 --allow-root
          
          OR
          
          jupyter notebook --ip 0.0.0.0 --no-browser --allow-root
          
 
## How do I commit the changes to the container as a new image ?

If you want to add more new software/libraries/packages after launching the container, you can still install stuff inside your container. Once you have installed new packages or libraries inside your container , lets say using a shell script or may be just a pip install, do the following:

      1) Exit the container using "exit" command
      2) docker commit [container ID] [repository:tag]
         e.g. docker commit 84a27cbe7cf4 demo_image:0.1
      3) confirm if the changes are made by "docker image ls"

