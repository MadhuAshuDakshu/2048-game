# 2048-game
Docker project

What is Docker ?
Docker is a containerization platform that provides easy way to containerize your applications, which means, using Docker you can build container images, run the images to create containers and also push these containers to container regestries such as DockerHub, Quay.io and so on.

Docker LifeCycle
We can use the above Image as reference to understand the lifecycle of Docker.

There are three important things,

docker build -> builds docker images from Dockerfile
docker run -> runs container from docker images
docker push -> push the container image to public/private regestries to share the docker images.

Understanding the terminology (Inspired from Docker Docs)
Docker daemon
The Docker daemon (dockerd) listens for Docker API requests and manages Docker objects such as images, containers, networks, and volumes. A daemon can also communicate with other daemons to manage Docker services.

Docker client
The Docker client (docker) is the primary way that many Docker users interact with Docker. When you use commands such as docker run, the client sends these commands to dockerd, which carries them out. The docker command uses the Docker API. The Docker client can communicate with more than one daemon.

Docker Desktop
Docker Desktop is an easy-to-install application for your Mac, Windows or Linux environment that enables you to build and share containerized applications and microservices. Docker Desktop includes the Docker daemon (dockerd), the Docker client (docker), Docker Compose, Docker Content Trust, Kubernetes, and Credential Helper. For more information, see Docker Desktop.

Docker registries
A Docker registry stores Docker images. Docker Hub is a public registry that anyone can use, and Docker is configured to look for images on Docker Hub by default. You can even run your own private registry.

When you use the docker pull or docker run commands, the required images are pulled from your configured registry. When you use the docker push command, your image is pushed to your configured registry. Docker objects

When you use Docker, you are creating and using images, containers, networks, volumes, plugins, and other objects. This section is a brief overview of some of those objects.

Dockerfile
Dockerfile is a file where you provide the steps to build your Docker Image.

Images
An image is a read-only template with instructions for creating a Docker container. Often, an image is based on another image, with some additional customization. For example, you may build an image which is based on the ubuntu image, but installs the Apache web server and your application, as well as the configuration details needed to make your application run.

You might create your own images or you might only use those created by others and published in a registry. To build your own image, you create a Dockerfile with a simple syntax for defining the steps needed to create the image and run it. Each instruction in a Dockerfile creates a layer in the image. When you change the Dockerfile and rebuild the image, only those layers which have changed are rebuilt. This is part of what makes images so lightweight, small, and fast, when compared to other virtualization technologies.


In simple words, you can understand as containerization is a concept or technology and Docker Implements Containerization.
# 2048 Game

A simple and fun implementation of the classic 2048 game, packaged using Docker for easy deployment and portability.

## Table of Contents

-Technologies Used Docker, AWS EBS, Github
-Installation - Docker desktop,Visual studio, AWS account


## Features

- Classic 2048 gameplay mechanics
- Responsive design for easy play on various devices
- Scoring system to track your best scores
- Dockerized for easy setup and deployment

## Technologies Used

- HTML, CSS, and JavaScript for the front end
- Docker for containerization
- AWS EBS for deployment

## Installation

STEP1:  ** Create a Docker file**

## Import the base image UBUNTU
FROM ubuntu:24.04

## update the UBUNTU using the RUN command
RUN apt-get update

## install curl,zip and nginx server using RUN command
RUN apt-get install -y nginx zip curl

## configure the nginx server
RUN echo "daemon off;" >>/etc/nginx/nginx.conf

## download the github repo into zip
RUN curl -o /var/www/html/master.zip -L https://codeload.github.com/gabrielecirulli/2048/zip/master

## unzip it and move to current location and remove the zip and unzip the file
RUN cd /var/www/html/ && unzip master.zip && mv 2048-master/* . && rm -rf 2048-master master.zip

## use EXPOSE to set port number 80 to listen the application
EXPOSE 80

## start the nginx server
CMD ["/usr/sbin/nginx", "-c", "/etc/nginx/nginx.conf"]

STEP2: Convert Docker file into Docker image 
## docker build command is used to build the docker image. Dot (.) represents the image from the current directory. If the image is from a different directory, then specify the path of the directory.
docker build -t <image name> .

STEP3: Check if docker image is build
## docker images command is used to check the docker image with its image id, created time and size.
docker images

STEP4: Create and run the container in the port number 80 using the created docker image.
## docker run command is used for this functionality
docker run -d -p 80:80 <image id>

STEP5: Once the image is ran to container, you can now access the application in the locahost using the port number 80 http://localhost:80

STEP6: Open AWS console and sign in with IAM user. Make sure that the IAM role has the EBS policy

## To create application in AWS Elastic Beanstalk (EBS)
1.Open the Elastic Beanstalk console.
2.Choose Create application.
3.For Application name enter getting-started-app.
4.Optionally add application tags.
5.For Platform, choose a platform.
6.Choose Next.
7.The Configure service access page displays.
8.Choose Use an existing service role for Service Role.
9.Next, we'll focus on the EC2 instance profile dropdown list. The values displayed in this dropdown list may vary, depending on whether you account has previously created a new environment.
10.Choose one of the following, based on the values displayed in your list.
If aws-elasticbeanstalk-ec2-role displays in the dropdown list, select it from the EC2 instance profile dropdown list.
If another value displays in the list, and it’s the default EC2 instance profile intended for your environments, select it from the EC2 instance profile dropdown list.
If the EC2 instance profile dropdown list doesn't list any values to choose from, expand the procedure that follows, Create IAM Role for EC2 instance profile.
11.Complete the steps in Create IAM Role for EC2 instance profile to create an IAM Role that you can subsequently select for the EC2 instance profile. Then return back to this step.
12.Now that you've created an IAM Role, and refreshed the list, it displays as a choice in the dropdown list. Select the IAM Role you just created from the EC2 instance profile dropdown list.
13.Choose Skip to Review on the Configure service access page. This skips the optional steps.
14.The Review page displays a summary of all your choices. Choose Submit at the bottom of the page.

## Elastic Beanstalk workflow
To deploy and run the example application on AWS resources, Elastic Beanstalk takes the following actions. They take about five minutes to complete.
1.Creates an Elastic Beanstalk application named getting-started-app.
2.Launches an environment named GettingStartedApp-env with these AWS resources:
3.An Amazon Elastic Compute Cloud (Amazon EC2) instance (virtual machine)
4.An Amazon EC2 security group
5.An Amazon Simple Storage Service (Amazon S3) bucket
6.Amazon CloudWatch alarms
7.An AWS CloudFormation stack
8.A domain name
9.For details about these AWS resources, see AWS resources created for the example application.
10.Creates a new application version named Sample Application. This is the default Elastic Beanstalk example application file.
11.Deploys the code for the example application to the GettingStartedApp-env environment.
12.During the environment creation process, the console tracks progress and displays event status in the Events tab. When all of the resources are launched and the EC2 instances running the application pass health checks, the environment's health changes to Ok. You can now use your web application's website.
