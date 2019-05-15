# Docker Tutorial

After the installation we want to create a simple docker container which has running an apache webserver.

## Step 1: Install Docker

Instructions: https://docs.docker.com/install/
or install Docker Toolbox if your system does not meet the requirements: https://docs.docker.com/toolbox/toolbox_install_windows/

## Step 2: Verify installation

Windows/Mac: Open Docker Desktop/Docker Toolbox
Linux: Open terminal
Run

    $ docker run hello-world
    
to make sure that docker works successfully.

## Step 3: Download Sources

Create a new directory where you run your container (i.e. docker-tutorial).

Download a apache sample file to that directory.
Source: https://tomcat.apache.org/tomcat-7.0-doc/appdev/sample/sample.war or the file from my repository at https://github.com/blapplications/introduction-to-docker/tutorial/
## Step 4: Create dockerfile

Create a new file named _Dockerfile_.

Put following text in your dockerfile:

    FROM tomcat:latest
    ADD sample.war /usr/local/tomcat/webapps/sample.war
    
The _FROM_ defines the base image, so our docker image is based on the latest version of the apache webserver. With the _ADD_ we copy an external file to a specified path in the container. 

## Step 5: Build the dockerfile

Go to your terminal and run 

    $ docker build -t dockertest .
    
You should not miss the dot.

## Step 6: Get Host IP

You need this ip later to access the web server.

    $ docker-machine ip
    
## Step 7: Run Docker Image

To run your image you have to run the following command:

    $ docker run dockertest
    
Your container is running now, but you cannot connect to the webserver now. This is because no ports are mapped out of the container.
If you like to run multiple containers/images there could be conflicts with the ports that the container/image is using. To set the port mapping you have to add

    -p 8888:8080
    
to your command. 8080 is the port inside the container and 8888 is the port you connect from outside.
Now you are able to access the apache webserver at http://{docker-machine-ip}:8888/sample/.

So in our example the complete command is

    $ docker run -p 8888:8080 dockertest
    
If the container still exists and you want to remove it and create a new one you have to add

    --rm
    
So the full command could be

    $ docker run --rm -p 8888:8080 dockertest
    
These are the basics of build and run a docker container.
If there are not just one or two ports that have to be mapped out it would be annoying to type the command every time you start the container. Also in the most time you use docker you need more than one container to run your application, for example a website. You need a Database, your webserver itself, and maybe a database.
In this case you need docker-compose.

## Step 8: docker-compose

We will start with the example aboth. So what we have to do is to specify the ports inside the config-file.

Create a new file called docker-compose.yml.

Set the version of docker-compose.

    version: "2"

If possible you should choose Version 3 oder higher. On my system it is just working with version 2 because I use the Docker Toolbox.

The next line to add is the services key, followed by the service name key, in this example it is called test.

    services:
      test:

At least set the name of your image and define the ports.

    image: dockertest
    ports:
      - "8888:8080"
     
Your complete docker-compose.yml is

    version: "2"
    services:
      test:
        image: dockertest
        ports:
          - "8888:8080"


To run the docker-compose.yml use the following command. The -d parameter is to run it in the background.

    $ docker-compose up -d 

