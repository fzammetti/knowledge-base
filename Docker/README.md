# Docker

---

* [Install Docker on CentOS 7](#32acacd9-f1b8-40a9-b0da-a47cd14d3bd2)
* [Create a new container](#8dcccda3-75f3-4e88-ad18-3adac5a08697)
* [Start a previously created container](#1c03d2b9-8ead-4adb-a500-db70e9b8e632)
* [Stop a running container](#01e188aa-7e0a-42bb-be47-1ad7890159fd)
* [List running containers](#d400746c-f833-4888-8abe-5df1140273be)
* [List ALL containers](#9513d5c4-152d-4ae2-bfc4-4ac6b7dc4cf7)
* [To view logs of a container](#f68382a0-9d61-4f70-9a4e-05734d1ffece)
* [If Docker daemon is running but docker commands can't connect to it](#3973941d-cc05-4638-8e5a-8749ddf5f11b)
* [To start Docker daemon](#6bb91d21-ef86-4c71-afd9-08fa4c040ba6)
* [To build an image](#aec26c39-b079-419f-b442-a3d2a0519f62)
* [To stop ALL running containers and then delete ALL containers](#9fbf7076-ff29-4956-a976-606134f3529f)
* [To have one container talk to another](#c28351f7-e132-4820-8371-687a7fa47270)
* [Move Docker directory (CentOS, maybe others)](#b0231ccb-865b-47b4-bef2-9326a9942863)
* [Remove all unused images](#03d9296f-22c0-442b-9f22-95a1b5470620)
* [Destructively clean up Docker](#bfa4a295-1d0e-401f-be5f-9a21ddd504b1)

---




<div id="32acacd9-f1b8-40a9-b0da-a47cd14d3bd2">

## Install Docker on CentOS 7

</div>

    curl -fsSL https://get.docker.com/ | sh
    systemctl enable docker




<div id="8dcccda3-75f3-4e88-ad18-3adac5a08697">

## Create a new container

</div>

Using Jenkins as an example, and assuming you’ve already did docker pull \<image_name>:

    docker run -d -u 0 -p 8080:8080 -p 50000:50000 -v /data/jenkins:/var/jenkins_home --privileged --name jenkins jenkins/jenkins:lts

* **-d** means "detached"
* **-u** 0 means run as root]
* **-p** are port mappings with 8080 as the Jenkins port and 50000 as the slave agent port
* **-v** is volume mapping to write data to /data/jenkins on the host
* **--privileged** gives extra rights to the container
* **--name** is the name to give the container

RUN THIS ONLY ONCE!  You DO NOT need to specify all the switches after the container is created, you can just **docker start \<container_name>** after that to start it again.

Note that for port and volume mappings, the format is always **\<host>:\<container>**.




<div id="1c03d2b9-8ead-4adb-a500-db70e9b8e632">

## Start a previously created container

</div>

    docker start <container_name>




<div id="01e188aa-7e0a-42bb-be47-1ad7890159fd">

## Stop a running container

</div>

    docker stop <container_name>




<div id="d400746c-f833-4888-8abe-5df1140273be">

## List running containers

</div>

    docker ps




<div id="9513d5c4-152d-4ae2-bfc4-4ac6b7dc4cf7">

## List ALL containers

</div>

    docker ps -a




<div id="f68382a0-9d61-4f70-9a4e-05734d1ffece">

## To view logs of a container

</div>

    docker logs <container_name>

Note that you can do **docker logs -f \<container_name>** to follow the logs.




<div id="3973941d-cc05-4638-8e5a-8749ddf5f11b">

## If Docker daemon is running but docker commands can't connect to it

</div>

* If the docker group doesn't already exist, add it: **sudo groupadd docker**
* Add the user that wants to run docker commands to it: **sudo usermod -aG docker \<username>**
* Restart the Docker daemon: **service docker restart**
* Either do a **newgrp docker** or log out and back in to activate the changes to groups.




<div id="6bb91d21-ef86-4c71-afd9-08fa4c040ba6">

## To start Docker daemon

</div>

    sudo service docker start




<div id="aec26c39-b079-419f-b442-a3d2a0519f62">

## To build an image

</div>

**docker login** (only needed if you're going to push to Docker Hub)
In directory with dockerfile:

    docker build -t <tag> .




<div id="9fbf7076-ff29-4956-a976-606134f3529f">

## To stop ALL running containers and then delete ALL containers

</div>

    docker stop $(docker ps -a -q)
    docker rm $(docker ps -a -q)




<div id="c28351f7-e132-4820-8371-687a7fa47270">

## To have one container talk to another

</div>

For example, Confluence talking to PostgreSQL, you may need to configure the containers to use a custom user-defined network.  To set that network up, do:

    docker network create -d bridge <net_name>

You only need to do this once!  Then, when creating a container, specify:

    --network=<net_name>

For example:

    docker network create -d bridge docker-net
    docker run -d -u 0 -p 5432:5432 -v /data/postgres:/var/lib/postgresql/data --privileged --name postgres --network=docker-net -e POSTGRES_PASSWORD=my_password postgres

Note that in a container, to reference these, you use the container name as the hostname.  For example, when setting up Confluence to use a Postgres container, the hostname you specify in the Confluence setup wizard is **postgres** (assuming that’s name as in the above example).




<div id="b0231ccb-865b-47b4-bef2-9326a9942863">

## Move Docker directory (CentOS, maybe others)

</div>

* Stop docker: service docker stop
* Verify no docker process running: ps faux
* Copy /var/lib/docker directory to your new partition: cp /var/lib/docker /home/docker
* Rename original directory: mv docker docker-old
* Make a symlink: ln -s /home/docker /var/lib/docker
* Confirm directory contents: ls /home/docker
* Restart docker: service docker start
* Restart containers as necessary
* Confirm containers are running properly, then delete old directory: rm -rf /var/lib/docker-old




<div id="03d9296f-22c0-442b-9f22-95a1b5470620">

## Remove all unused images

</div>

WARNING! This will remove all images without at least one container associated to them.

    docker image prune -a




<div id="bfa4a295-1d0e-401f-be5f-9a21ddd504b1">

## Destructively clean up Docker

</div>

WARNING! This will delete ALL containers, images and networks! It is destructive and irreversible!

    docker system prune -a -f
    docker builder prune
    find /var/lib/docker/containers/ -type f -name "*.log" -delete
