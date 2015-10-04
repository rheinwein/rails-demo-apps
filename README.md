# Getting started with Docker and Docker Compose

## Installing Docker
Hop over to [Docker Toolbox](https://www.docker.com/toolbox) to get started whether you're on UNIX, Linux, or Windows. 

## app-with-sqlite
This Rails application runs in one container. Using the Dockerfile and/or docker-compose.yml, you can start this application in a container. First, `docker-compose build` to make sure your image builds correctly. Then, you can run `docker-compose up` to start your application in a container. User `docker-compose ps` to see running processes.

## app-with-postgres
A two-container Rails application that runs a Postgres database in a separate container. Using Docker's `link` instruction, we can declare a dependency and the access automatically generated environment variables in our config file in order to connect to the database. Note that this application includes a port binding rule to 3333 on the Docker host -- this is to avoid a conflict if 3000 is already bound to the previous example. 

## Orchestration
[CoreOS and Fleet](https://coreos.com/using-coreos/clustering/)
[Kubernetes](http://kubernetes.io/)
[Marathon and Mesos](https://mesosphere.github.io/marathon/)
