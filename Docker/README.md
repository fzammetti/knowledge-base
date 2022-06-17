# Docker
----------

## Install Docker on CentOS 7

    curl -fsSL https://get.docker.com/ | sh
    systemctl enable docker

## Create a new container

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

## Start a previously created container

    docker start <container_name>

## Stop a running container

    docker stop <container_name>

## List running containers

    docker ps

## List ALL containers

    docker ps -a

## To view logs of a container

    docker logs <container_name>

Note that you can do **docker logs -f \<container_name>** to follow the logs.

## If Docker daemon is running but docker commands can't connect to it

* If the docker group doesn't already exist, add it: **sudo groupadd docker**
* Add the user that wants to run docker commands to it: **sudo usermod -aG docker \<username>**
* Restart the Docker daemon: **service docker restart**
* Either do a **newgrp docker** or log out and back in to activate the changes to groups.

## To start Docker daemon

    sudo service docker start

## To build an image

**docker login** (only needed if you're going to push to Docker Hub)
In directory with dockerfile:

    docker build -t <tag> .

## To stop ALL running containers and then delete ALL containers

    docker stop $(docker ps -a -q)
    docker rm $(docker ps -a -q)

## To have one container talk to another

For example, Confluence talking to PostgreSQL, you may need to configure the containers to use a custom user-defined network.  To set that network up, do:

    docker network create -d bridge <net_name>

You only need to do this once!  Then, when creating a container, specify:

    --network=<net_name>

For example:

    docker network create -d bridge docker-net
    docker run -d -u 0 -p 5432:5432 -v /data/postgres:/var/lib/postgresql/data --privileged --name postgres --network=docker-net -e POSTGRES_PASSWORD=my_password postgres

Note that in a container, to reference these, you use the container name as the hostname.  For example, when setting up Confluence to use a Postgres container, the hostname you specify in the Confluence setup wizard is **postgres** (assuming that’s name as in the above example).
