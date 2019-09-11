# Docker
Docker Installation

<h2>Introduction</h2>
<p>Docker is an application that makes it simple and easy to run application processes in a container, which are like virtual machines, only more portable, more resource-friendly, and more dependent on the host operating system.
Docker is a containerization technology that allows you to quickly build, test and deploy applications as portable, self-sufficient containers that can virtually run everywhere.
Docker vastly changed many software development and operations paradigms all at once. The ways we architect, develop, ship, and run software before and after Docker are vastly different. Although Docker does not prescribe a certain recipe, it forces people to think in terms of microservices and immutable infrastructure.

<h2>Install Docker on CentOS</h2>
Although the Docker package is available in the official CentOS 7 repository, it may not always be the latest version. The recommended approach is to install Docker from the Docker’s repositories.
To install Docker on your CentOS 7 server follow the steps below:</p>
<strong>Update system packages and install the required dependencies:</strong>
<li>$ sudo yum check-update</li>
<li>$ sudo yum update</li>
<li>$ sudo yum install -y yum-utils device-mapper-persistent-data lvm2</li>

<strong>add the Docker stable repository:</strong>
<li>$ sudo yum-config-manager --add-repo  https://download.docker.com/linux/centos/docker-ce.repo</li>

<strong>Enable Docker repository:</strong>
<li>$ sudo yum-config-manager --enable docker-ce-edge</li>

<strong>install the latest version of Docker CE (Community Edition) using yum by typing:</strong>
<li>$ sudo yum install docker-ce</li>

$ start the Docker daemon and enable it to automatically start at boot time:
<li>$ sudo systemctl start docker</li>
<li>$ sudo systemctl enable docker</li>

<strong>verify that the Docker service is running type:</strong>
$ sudo systemctl status docker

<strong>Print the Docker version type:</strong>
$ docker -v
$ docker version

<strong>view system-wide information:</strong>
$ docker info

<strong>Executing the Docker Command Without Sudo</strong>
By default managing, Docker requires administrator privileges. If you want to run Docker commands as a non-root user without prepending sudo you need to add your user to the docker group which is created during the installation of the Docker CE package. You can do that by typing:
$ sudo usermod -aG docker $(whoami)
$ sudo usermod -aG docker $USER

<strong>Log out and log back in so that the group membership is refreshed.</strong>
To verify Docker is installed successfully and that you can run docker commands without sudo, issue the following command which will download a test image, run it in a container, print a “Hello from Docker” message and exit:
$ docker container run hello-world

<strong>docker CLI:</strong>
list all available commands by typing
$ docker

<strong>Docker images:</strong>
a Docker image is essentially a snapshot of a Docker container.
To search the Docker Hub repository for an image just use the search subcommand. For example, to search for the CentOS image, run:
$ docker search centos

If we want to download the official build of CentOS 7, we can do that by using the image pull subcommand:
$ docker image pull centos

<strong>Check List of Images:</strong>
$ docker image
$ docker image ls
$ docker images ls

<strong>delete an image:</strong>
$ docker image rm centos

<strong>Docker container:</strong>
An instance of an image is called a container. A container represents a runtime for a single application, process, or service.

It may not be the most appropriate comparison but if you are a programmer you can think of a Docker image as class and Docker container as an instance of a class.

We can start, stop, remove and manage a container with the docker container subcommand.

The following command will start a Docker container based on the CentoOS image. If you don’t have the image locally, it will download it first:

The switch -it allows us to interact with the container via the command line. To start an interactive container type:

$ docker container run -it centos /bin/bash

As you can see from the output once the container is started the command prompt is changed which means that you’re now working from inside the container:

<strong>list active containers:</strong>
$ docker container ls

<strong>To view both active and inactive containers, pass it the -a switch:</strong>
$ docker container ls -a


<strong>delete one or more containers:</strong>
$ docker container rm c55680af670c

<strong>Committing Changes in a Container to a Docker Image:</strong>
When you start up a Docker image, you can create, modify, and delete files just like you can with a virtual machine. The changes that you make will only apply to that container. You can start and stop it, but once you destroy it with the docker rm command, the changes will be lost for good.

$ docker container run -it centos /bin/bash

<strong>Important: Note the container id in the command prompt.</strong>

Now you may run any command inside the container. For example, let's install MariaDB server in the running container. No need to prefix any command with sudo, because you're operating inside the container with root privileges:

$ yum install mariadb-server

This section shows you how to save the state of a container as a new Docker image.

After installing MariaDB server inside the CentOS container, you now have a container running off an image, but the container is different from the image you used to create it.

To save the state of the container as a new image, first exit from it:

$ exit

Then commit the changes to a new Docker image instance using the following command. The -m switch is for the commit message that helps you and others know what changes you made, while -a is used to specify the author. The container ID is the one you noted earlier in the tutorial when you started the interactive docker session. Unless you created additional repositories on Docker Hub, the repository is usually your Docker Hub username:

docker commit -m "What did you do to the image" -a "Author Name" container-id repository/new_image_name

<strong>For example:</strong>
$ docker commit -m "added mariadb-server in centos" -a "Ubaid" 59839a1b7de2 finid/centos-mariadb

After that operation has completed, listing the Docker images now on your computer should show the new image, as well as the old one that it was derived from:
$ docker images

<strong>Listing Docker Containers:</strong>
After using Docker for a while, you'll have many active (running) and inactive containers on your computer. To view the active ones, use:
$ docker ps

<strong>To view all containers — active and inactive, pass it the -a switch:</strong>
docker ps -a

<strong>To view the latest container you created, pass it the -l switch:</strong>
$ docker ps -l

<strong>Stopping a running or active container is as simple as typing:</strong>
$ docker stop container-id

<strong>Pushing Docker Images to a Docker Repository:</strong>
The next logical step after creating a new image from an existing image is to share it with a select few of your friends, the whole world on Docker Hub, or other Docker registry that you have access to. To push an image to Docker Hub or any other Docker registry, you must have an account there.

<strong>To create an account on Docker Hub, register at Docker Hub (https://hub.docker.com/). Afterwards, to push your image, first log into Docker Hub. You'll be prompted to authenticate:</strong>
$ docker login -u docker-registry-username

<strong>If you specified the correct password, authentication should succeed. Then you may push your own image using:</strong>
$ docker push docker-registry-username/docker-image-name

<h2>Create Docker Image and Run Java Application</h2>

<strong>First create a Dockerfile inside your java project</strong>

// Download Java image from docker repository:

<strong>FROM openjdk:8</strong>

// Add JAR file from target to docker conatiner on root directory"

<strong>ADD target/docker-spring-boot.jar docker-spring-boot.jar</strong>

// Port number which application listen:
EXPOSE 8085

// Tell the command to run application
ENTRYPOINT ["java", "-jar", "docker-spring-boot.jar"]

<strong>Build docker image using Dockerfile</strong>

$ docker build -f Dockerfile -t docker-spring-boot .
$ docker build -f Dockerfile -t ubaidimage .

<strong>Run docker container</strong>
$ docker run -p 8085:8085 docker-spring-boot
$ docker run -p 9090:9090 ubaidimage --name ubaidcontainer

<strong>Check if image is created:</strong>

$ docker images

<strong>Run this image:</strong>
// -p is to push the application
$ docker run -p 8085:8085 docker-spring-boot

<strong>ANOTHER DOCKERFILE:</strong>
Dockerfile
FROM openjdk:8
MAINTAINER Acme Inc.
RUN mkdir -p /opt
ADD fat.jar /opt/fat.jar
EXPOSE 8080
ENV MYSQL_PASSWORD=secret
USER nobody
WORKDIR /opt
CMD ["/usr/opt/java", "-jar", "fat.jar"]


$ docker machine env // to check docker IP

Github.com/spotify - Docker maven plugin

docker start (image ID)
docker run -d (image ID)

docker pull registry
docker images
docker run -d -p 5000:5000 registry

docker pull mysql

enter in to the container
docker exec -it (image ID) bash

docker logs -f (image ID)
