#Getting Started With MongoDB As A Docker Container Deployment.md

Downloading the Latest MongoDB Docker Image

Assuming that you have Docker installed on your computer or server, we can obtain the MongoDB image from the Docker Hub container registry. There are numerous images available, each representing different versions of MongoDB or the underlying operating system that it is installed on.

The simplest solution to download our image is to execute the following:

docker pull mongo

The above command will get the latest tag which could be anything. This is great if you want to be bleeding edge, but when you’re doing realistic deployments you probably want to be in control of the versioning information. Instead, you might want to specify the tag like this:

docker pull mongo:4.0.4

At the time of writing this, MongoDB 4.0.4 is the latest version and it is what the latest tag represents. This time we’re actually specifying the 4.0.4 tag because that is what we want.

It may take a moment, but when it is done, we have an image that is ready for deployment.
Deploying an Instance of MongoDB as a Container

With the image available to us, we need to deploy it. In its simplest form, and what is outlined in the Docker Hub documentation, we can execute the following command:

docker run --name mongodb mongo:4.0.4

This command works, but there are potentially a few problems. What we’re doing is we are running a container and naming it mongodb. We aren’t running it in the background and if we wanted to connect to it, we’d have to do it from another container instance. In other words, we wouldn’t be able to connect to it from our host computer or server.

Instead, what we could do is the following:

docker run -d -p 27017-27019:27017-27019 --name mongodb mongo:4.0.4

The above command will run the container in detached mode, or in the background for us. We are also mapping the container ports with host ports so that way we can access the database from a host level application if we wanted to. The ports used were taken from the MongoDB documentation. You don’t need to map the ports in order to use MongoDB. Like I said, the port mapping is only necessary if you wish to use it from your host Mac, Windows, or Linux computer. If you plan to deploy all your applications as micro-services with Docker, then you’d be fine as long as your containers can communicate with each other.
Interacting with the MongoDB Docker Container with Basic Shell Operations

At this point in time we have a functional MongoDB deployment. Rather than creating an application to make use of it, we’re going to interact with the database through the shell client.

If you’ve been following along so far, your container is currently running in detached mode. This means that we need to connect to it using the interactive terminal:

docker exec -it mongodb bash

The above command will connect to our deployment named mongodb using the interactive terminal and start the bash shell. More details on connecting to detached Docker containers can be found in my previous tutorial titled, Connecting to a Detached Docker Container for Terminal Interaction.

You’ll notice that you are now using your Terminal as if you were inside your container. This is where we can start using MongoDB.

To launch the MongoDB shell client, execute the following:

mongo

When inside the MongoDB shell client, you can access all the functionality that is outlined in the MongoDB documentation. For example, we can see what databases exist in our instance with the following:

show dbs

To create a new database, we can use a multi-step process, the first being to define the database we wish to use:

use thepolyglotdeveloper

We’re using the database thepolyglotdeveloper, but it doesn’t exist until we start creating collections and data. To create a collection with data, we can do something like this:

db.people.save({ firstname: "Nic", lastname: "Raboy" })
db.people.save({ firstname: "Maria", lastname: "Raboy" })

With two documents created in a new people collection in our thepolyglotdeveloper database, we can query for data using something like the following:

db.people.find({ firstname: "Nic" })

There is a lot that you can accomplish with the shell client, but you can get the general idea. What we did prove is that we were able to interact with the container instance.