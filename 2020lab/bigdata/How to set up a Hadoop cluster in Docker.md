#How to set up a Hadoop cluster in Docker.md

Apache Hadoop is a popular big data framework that is being used a lot in the software industry. As a distributed system, Hadoop runs on clusters ranging from one single node to thousands of nodes.

If you want to test out Hadoop, or don’t currently have access to a big Hadoop cluster network, you can set up a Hadoop cluster on your own computer, using Docker. Docker is a popular independent software container platform that allows you to build and ship your applications, along with all its environments, libraries and dependencies in containers. The containers are portable, so you can set up the exact same system on another machine by running some simple Docker commands. Thanks to Docker, it’s easy to build, share and run your application anywhere, without having to depend on the current operating system configuration.

For example, if you have a laptop that is running Windows but need to set up an application that only runs on Linux, thanks to Docker, you don’t need to install a new OS or set up a virtual machine. You can set up a Docker container containing all the libraries you need and delete it the moment you are done with your work.

In this tutorial, we will set up a 3-node Hadoop cluster using Docker and run the classic Hadoop Word Count program to test the system.
1. Setting up Docker

If you don’t already have Docker installed, you could install it easily following the instructions on the official Docker homepage.

To check the version of your Docker Engine, Machine and Compose, use the following commands:

$ docker --version
$ docker-compose --version
$ docker-machine –version

If it’s your first time running Docker, test to make sure things are working properly by launching your first Dockerized web server:

docker run -d -p 80:80 --name myserver nginx

Since this is the first time you will run this command, and the image is not yet available offline, Docker will pull it from the Docker Hub library. After everything is finished, visit http://localhost to view the homepage of your new server.
2. Setting up a Hadoop Cluster Using Docker

To install Hadoop in a Docker container, we need a Hadoop Docker image. To generate the image, we will use the Big Data Europe repository. If Git is installed in your system, run the following command, if not, simply download the compressed zip file to your computer:

$ git clone git@github.com:big-data-europe/docker-hadoop.git

Once we have the docker-hadoop folder on your local machine, we will need to edit the docker-compose.yml file to enable some listening ports and change where Docker-compose pulls the images from in case we have the images locally already (Docker will attempt to download files and build the images the first time we run, but on subsequent times, we would love to use the already existing images on disk instead of rebuilding everything from scratch again). Open the docker-compose.yml file and replace the content with the following (You can also download or copy and paste from this Github Gist):

To deploy a Hadoop cluster, use this command:

$ docker-compose up -d

Docker-Compose is a powerful tool used for setting up multiple containers at the same time. The -d parameter is used to tell Docker-compose to run the command in the background and give you back your command prompt so you can do other things. With just a single command above, you are setting up a Hadoop cluster with 3 slaves (datanodes), one HDFS namenode (or the master node to manage the datanodes), one YARN resourcemanager, one historyserver and one nodemanager.

A Typical Hadoop HDFS Architecture

Docker-Compose will try to pull the images from the Docker-Hub library if the images are not available locally, build the images and start the containers. After it finishes, you can use this command to check for currently running containers:

$ docker ps

Go to http://localhost:9870 to view the current status of the system from the namenode:
3. Testing your Hadoop cluster

Now we can test the Hadoop cluster by running the classic WordCount program.

Enter into the running namenode container by executing this command:

$ docker exec -it namenode bash

First, we will create some simple input text files to feed that into the WordCount program:

$ mkdir input
$ echo "Hello World" >input/f1.txt
$ echo "Hello Docker" >input/f2.txt

Now create the input directory on HDFS

$ hadoop fs -mkdir -p input

To put the input files to all the datanodes on HDFS, use this command:

$ hdfs dfs -put ./input/* input

Download the example Word Count program from this link (Here I’m downloading it to my Documents folder, which is the parent directory of my docker-hadoop folder.

Now we need to copy the WordCount program from our local machine to our Docker namenode container.

Use this command to find out the ID of your namenode container:

$ docker container ls

Copy the container ID of your namenode in the first column and use it in the following command to start copying the jar file to your Docker Hadoop cluster:

$ docker cp ../hadoop-mapreduce-examples-2.7.1-sources.jar cb0c13085cd3:hadoop-mapreduce-examples-2.7.1-sources.jar

Now you are ready to run the WordCount program from inside namenode:

root@namenode:/# hadoop jar hadoop-mapreduce-examples-2.7.1-sources.jar org.apache.hadoop.examples.WordCount input output

To print out the WordCount program result:

root@namenode:/# hdfs dfs -cat output/part-r-00000

World 1

Docker 1

Hello 2

Congratulations, you just successfully set up a Hadoop cluster using Docker!

To safely shut down the cluster and remove containers, use this command:

$ docker-compose down