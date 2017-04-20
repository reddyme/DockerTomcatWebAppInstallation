# Tomcat Webapp Installation on Docker

**This is a guide to install Tomcat Webapp (.war) on Docker**
**Steps followed to create this project:**

1. Install Docker on Amazon EC2 instance.
2. Create a docker image for war file.
3. Build the docker file using the image (it will create image)
4. Run the image file on docker (it will create the container). On this container we will have all war file installed.

#Instructions in detail:
1. Install Docker on Amazon EC2 instance.
   Login to Amazon EC2 instance and execute below command:
   a. apt-get install docker.io or yum install docker-io
   b. Install (as root): yum install docker-io
   c. Start the docker service (as root): service docker start
   d. Let regular users access docker (as root):
      usermod -a -G docker <username>
2. Create a docker image for war file.
   vi Dockerfile
   # Pull base image
    From tomcat:8-jre8
   # Copy to images tomcat path
    COPY RestWebService.war /usr/local/tomcat/webapps/
3. Build the docker file using the image (it will create image)
    docker build -t restfulwebservice .
4. Run the image file on docker (it will create the container). On this container we will have all war file installed.
    docker run -it --rm -p 8080:8080 restfulwebservice

#How to change the port number 80 instead of 8080:
1. Once we run the docker, it creates a container for us. We can login to that container and change the server file in config.
2. After the docker run, open another terminal and run docker ps -a (this will display the container ID)
3. Login to that container by docker exec -it containerid bash
4. Once we logged in, do apt-get update and apt-get install vim to install vim editor. This is required to change server file in config folder.
5. After port is changed we need to commit the changes using docker commit image-id newImageName

#How to create tar file of the image:
1. docker images (it will display all the images with id's)
2. docker save <image id> | gzip > out.tar.gz

#How to Test:
1. docker load -i restwebserviceimage.tgz
2. docker run -d -p 8081:80 "DockerImageId"
   Note: We can get the DockerImageId by executing "docker images" command
3. http://HostIP:8081/RestWebService/

