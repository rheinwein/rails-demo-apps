# Getting started with Docker and Docker Compose

## Installing Docker
Hop over to the Docker installation docs to get started whether you're on [OS X](http://docs.docker.com/mac/started/), [Linux](http://docs.docker.com/linux/started/), or [Windows](http://docs.docker.com/windows/started/). 

On a Linux machine, you can use the official packages because Docker runs natively on Linux. If you're on Windows or Mac, it's best to start by using [Docker Toolbox](https://www.docker.com/toolbox).

## Using Docker Toolbox
If you're on a Mac or Windows and have installed Docker Toolbox, you'll probably have arrived at a terminal window. Run `docker-machine ls` to get a list of the Docker VMs running on your system. Before we can go further, you need to `eval $(docker-machine env MACHINE_NAME)` in order to be able to run Docker commands from your host system's terminal.

If you want to see your applications running in a browser on your host system, you'll need to know the IP address of your Docker machine. Run `docker-machine ip MACHINE_NAME` to find it, and keep it handy, because you'll want it later.

## Running the dummy applications
Each of the applications comes with a `Dockerfile` (to build the Docker image) and a `docker-compose.yml` file, which will run with Docker Compose. The Rails applications have been freshly created with a simple `rails new`.

### app-with-sqlite
This Rails application runs in one container. Using the Dockerfile and/or docker-compose.yml, you can start this application in a container. First, `docker-compose build` to make sure your image builds correctly. Then, you can run `docker-compose up` to start your application in a container. Use `docker-compose ps` to see running processes.

This application runs on port 3000, and you can see a port binding rule in the Docker Compose file of '3000:3000'. This means that you can access this application on port 3000 of the Docker Machine. In your browser, verify that the application is running by navigating to DOCKER_MACHINE_IP:3000. You can find the IP of your Docker machine by running `docker-machine ip MACHINE_NAME`.

### app-with-postgres
Taking it up a notch, the next example is a two-container Rails application that runs a Postgres database in a separate container. Using Docker's `link` instruction, we can declare a dependency and the access automatically generated environment variables in our config file in order to connect to the database. Note that this application includes a port binding rule to 3333 on the Docker host -- this is to avoid a conflict if 3000 is already bound to the previous example. You can verify that the application is running by navigating to DOCKER_MACHINE_IP:3333.


###Modifying code
For both of these examples, you can see a volume mount instruction in the Docker Compose file. A volume mount is one way to get code inside of a container so that you can still edit it on your local host and see the changes inside the container. Think of it as a synced folder. When your application is up and running, you can edit files on your local system and see the changes reflected in the container. This is ideal for development; note that for production, you'll want to statically copy the files into the image. We don't do this for development, because then we'd have to rebuild the image every time we edited a file, which is a real pain.

###Running a database migration with a containerized Rails service
In both examples, both the Rails service and databases are running inside a container. You can use Docker Compose to run commands within the containers. We can see this when running a task like a database migration. Locally, you can generate a scaffold for a new resource. Locally (remember that the code is mounted into the container), run `rails generate scaffold Dog name:string age:integer` or whatever resource you want to create. If you hit your app again in the browser, you'll get a migration error. Using Docker Compose, we can run the migration by saying `docker-compose run SERVICE_NAME rake db:migrate`. In the case of the example files, the service name is `web`. You'll see the migration run, and then you can hit DOCKER_MACHINE_IP:3000/dogs in your browser.

![](https://media.giphy.com/media/GAeKPO8201Vmg/giphy.gif)

Awesome!

## Orchestration
Once you've moved beyond setting up applications with Docker and are interested in some tools to manage Docker workloads in production, check out the following tools.

* [CoreOS and Fleet](https://coreos.com/using-coreos/clustering/)
* [Kubernetes](http://kubernetes.io/)
* [Marathon and Mesos](https://mesosphere.github.io/marathon/)
