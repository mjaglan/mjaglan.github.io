# Setting up an automated local development environment using Docker and Vagrant

# Problem Statement: 
We need a way to build isolated and repeatable production like development environments on local machine.


# Possible Solutions:
Classic VMs ensure isolated and repeatable environments. But these are resource consuming. Developers need to code/build/test every few minutes and won't accept the virtualization overhead. So instead of make OS level containers (i.e., VMs), using application level containers is extremely fast and easier to maintainer. For this we use docker application containers and vagrant automation to provide the desired OS for docker container to run in isolation - 

Feature | Options
------- | -------
Application Container | [Docker](https://www.docker.com/)
Base OS Provider | [Vagrant](https://www.vagrantup.com/)


# Solution Evaluations:  

![docker-dev-in-box-environment](images/dev-environment/docker-vagrant-host.png) 

Image Source: [docker-dev-in-box-environment](http://itsmyviewofthings.blogspot.com/2014/06/docker-dev-in-box-environment-setup.html)

In this experiment, we write code for setting up the base infrastructure on local developer machine, a.k.a., Infrastructure as code (IaC). The project on which this experiment is done - [github.com/airavata-courses/spring17-API-Server](https://github.com/airavata-courses/spring17-API-Server) - a microservice. For building Infrastructure as code the additional files created are  "Dockerfile" for each microservice, and one "Vagrantfile" as provider automation script. For any project this is a one time effort. After that building production like environment on local machine is fast and easy. So it can save a lot time and effort compared to doing the same activity manually.

## Docker
Dockerfiles are really straightforward and there is an excellent [online reference manual](https://docs.docker.com/engine/reference/builder/). You can directly run a docker container from the command like this - 
```docker build -t=myDockerImageName .```

And you can run a docker-image as container like this -
```docker run -a stdin -a stdout -i -t myDockerImageName /bin/bash```


![running-docker-directly](images/dev-environment/no-vagrant.png) 

By default, on Windows or Mac machine, these containers run on top of [boot2docker](https://github.com/boot2docker/boot2docker) docker-machine by docker installation. On linux, docker directly leverages the virtualization from the host machine kernel. The docker container (i.e., running images) consumes few MBs of memories as compared to running a VM which consumes memory in GBs. On Windows or Mac machine, in order to run docker-containers, on custom linux flavor, Vagrant VM automation tool is useful which is explained below.  

## Vagrant
![running-vagrant-with-docker-provider](images/dev-environment/yes-vagrant.png) 


# Conclusion
Writing Dockerfile and Vagrantfile requires initial effort. But the over all gains are considerable for long term. Infrastructure as Code provides the fastest and most resource-effective way of building isolated and repeatable environments. Since containers are isolated instances that run your applications. These lightweight instances can be replaced, rebuilt, and moved around easily. This allows us to mirror the production and development environment and is a tremendous help in the continuous integration and delivery process.

  
# Associated Issue(s) on GitHub
- [IAC: [POC] Setting up a development environment](https://github.com/airavata-courses/spring17-devops/issues/6)


# Associated Code(s) on GitHub
[github.com/airavata-courses/spring17-devops/tree/feature-iac-dev-env/infrastructure/api-server/terraform/dev-env](https://github.com/airavata-courses/spring17-devops/tree/feature-iac-dev-env/infrastructure/api-server/terraform/dev-env)


# References Used for Experiment(s)
- [blog.zenika.com/../development-environment-using-docker-and-vagrant](http://blog.zenika.com/2014/10/07/setting-up-a-development-environment-using-docker-and-vagrant/)

