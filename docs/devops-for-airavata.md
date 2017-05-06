# DevOps for Apache Airavata

# Problem Statement
In [previous work](./docker-and-vagrant-with-IDE.md), IDE Integration of Docker and Vagrant tools were explored. In the final report, there are three ways of Local Provisioning for development & Testing suggested -
-	Run Ansible Playbooks on top of Vagrant Box
-	Run Ansible Playbooks on top of Docker Container
-	Dockerizing Airavata Microservices

For these three approaches, new tools like Vagrant and Docker are heavily used. Explore and suggest best practices of Vagrant and Docker Development in IDE. And explore and suggest best practices of Debugging deployed code base in Vagrant box and Docker container.

# Possible Solutions
For Vagrant development in IDE following tool is considered -

Feature | Options
------- | -------
Eclipse IDE | [Eclipse Vagrant Tooling](https://marketplace.eclipse.org/content/eclipse-vagrant-tooling)
IntelliJ Idea IDE | [Jetbrains Vagrant Plugin](https://plugins.jetbrains.com/plugin/7379-vagrant)

For Docker Development in IDE following tool is considered -

Feature | Options
------- | -------
Eclipse IDE | [Eclipse Docker Tooling](https://marketplace.eclipse.org/content/eclipse-docker-tooling)
IntelliJ Idea IDE | [Jetbrains Docker Integration](https://plugins.jetbrains.com/plugin/7724-docker-integration)

For debugging deployed code base in Vagrant box following approaches are considered - 
- [Vagrant SSH](https://www.vagrantup.com/intro/getting-started/up.html)
- [Vagrant Synced folders](https://www.vagrantup.com/docs/synced-folders/)
- Remote Debugging
- Debugging output from Ansible and Vagrant (for Ansible Provisioning in Vagrant)

For debugging deployed code base in Docker Container following approaches are considered -
- Logging via the Application
- Logging via Data Volumes 
- Logging via the Docker Logging Driver 
- Logging via a Dedicated Logging Container
- Sidecar Approach



# Solution Evaluations

### For Vagrant development in IDE

##### Eclipse Plugin - Eclipse Vagrant Tooling
![Eclipse Vagrant Tooling](./../images/airavata-devops/EVT.png)

_Source Image: [marketplace.eclipse.org/.../eclipse-vagrant-tooling](https://marketplace.eclipse.org/content/eclipse-vagrant-tooling#group-screenshots)_


##### IntelliJ Idea Plugin - Jetbrains Vagrant Plugin

![Eclipse Vagrant Tooling](./../images/airavata-devops/JVP.png)

_Source Image: [plugins.jetbrains.com/plugin/7379-vagrant](https://plugins.jetbrains.com/plugin/7379-vagrant)_


### For Docker Development in IDE

##### Eclipse Plugin - Eclipse Docker Tooling
![Eclipse Vagrant Tooling](./../images/airavata-devops/EDT.png)

_Source Image: [marketplace.eclipse.org/.../eclipse-docker-tooling](https://marketplace.eclipse.org/content/eclipse-docker-tooling#group-screenshots)_


##### IntelliJ Idea Plugin - Jetbrains Docker Integration

![Eclipse Vagrant Tooling](./../images/airavata-devops/JDIP.png)

_Source Image: [plugins.jetbrains.com/plugin/7724-docker-integration](https://plugins.jetbrains.com/plugin/7724-docker-integration)_

### For debugging deployed code base in Vagrant box

- ```Vagrant SSH```: Login to vagrant box in order to inspect processes manually.

- ```Vagrant Synced folders```: Set up shared folders in vagrantfile like this 

	```
	config.vm.synced_folder "/home/dev-user/my-box/application-1/logs", "/var/log/application-1/", disabled: false, create: true
	config.vm.synced_folder "/home/dev-user/my-box/application-2/logs", "/var/log/application-2/", disabled: false, create: true
	config.vm.synced_folder "/home/dev-user/my-box/application-3/logs", "/var/log/application-3/", disabled: false, create: true
	```

	This shared folder is a centralized persistent space on disk available for read from the host machine.
	
- ```Remote Debugging```: Attach your IDE's debugger to a running process which is inside the vagrant box. For this port-forwarding configuration should be enabled. A typical configuration would look like this -

![Remote debug configuration in an IDE](./../images/airavata-devops/remote-debug.png)

_Image Source: [confluence.jetbrains.com/.../Configuring+a+Vagrant+VM+for+Debugging](https://confluence.jetbrains.com/display/PhpStorm/Configuring+a+Vagrant+VM+for+Debugging)_



- ```Debugging output from Ansible and Vagrant```: Get additional debug information about locally running Ansible playbooks when provisioned via Vagrant like [this](https://serverfault.com/a/611070) - 

	```
	config.vm.provision "ansible" do |ansible|
    	ansible.verbose = "vvv"
	end
	```
	
	This sets the verbose option of ansible:

	```
	-v, --verbose         verbose mode (-vvv for more, -vvvv to enable
                        connection debugging)
	```


### For debugging deployed code base in Docker Container
- ```Logging via the Application```: This method requires each container to have its own internal methods for logging. However, since the logging framework is limited to the container itself, any logs stored in the container's filesystem will be lost if the container shuts down. This is because a container's underlying file system only persists for the life of the container. To prevent data loss, you will have to either configure a persistent data volume or forward logs to a remote destination. Application logging also becomes difficult when deploying multiple identical containers, since you would need a way of uniquely identifying each container.

- ```Logging via Data Volumes```: Each running microservice in a container will have its own internal methods for logging (as discussed in above point).  And then create shared-directory between host machine and running container.  The application in the container will dump log files to this data volume. With a data volume, you can store long-term data in your containers by mapping a directory in the container to a directory on the host machine. You can also share a single volume across multiple containers to centralize logging across multiple services. However, data volumes make it difficult to move these containers to different hosts without potentially losing log data.

- ```Logging via the Docker Logging Driver (Recommended)```: This method uses the Docker service to read stdout and stderr output generated by containers. The default configuration writes logs to a file on the host machine, but changing the logging driver lets you forward messages to syslog, journald, gelf, or other endpoints.

- ```Logging via a Dedicated Logging Container```: A dedicated logging container serves the sole purpose of consuming log events. Running it as a container lets you make your logging solution a part of your architecture.


- ```Sidecar Approach```: Like the dedicated logging approach, the sidecar approach uses logging containers. The difference is that it pairs each application container with its own dedicated logging container. It can be implemented using mounted volumes to share logs between containers, as discussed in [this](https://www.loggly.com/blog/how-to-implement-logging-in-docker-with-a-sidecar-approach/) blog by loggly.com.



# Conclusion

In short term, we recommend ```Ansible Playbooks on top of Vagrant Box```, and for this the Vagrant IDE plugin and shared directory based debugging practices suits best. This is a minimal effort solution, fits well in the current development life cycle of Airavata and easier to set up.

In long term, if Airavata is to be built ground up using Docker containers, we recommend ```Dockerizing Airavata Microservices```, and for this Docker IDE plugin and one of five different ways of debugging options are available depending on how production architecture of logging mechanism is setup. This is a good solution, with larger long term benefits in terms of saving resource consumption and time, but this will need some initial time investment in order to dockerize/ containerize all Airavata microservices.

# Associated Discussion(s)
- [[Debugging applications with Docker Containers] Setting up an automated production like local development environment (part-4)](https://lists.apache.org/)

- [[Debugging applications with Vagrant Boxes] Setting up an automated production like local development environment (part-3)](https://lists.apache.org/)

- [[IDE Integration] Setting up an automated production like local development environment (part-2)](https://lists.apache.org/thread.html/52c10855b6b11d53fa2bca9003c39779d4522d4f7386954179ac41b4@%3Cdev.airavata.apache.org%3E)


# Associated Issue(s) on GitHub
- [IAC: devops for airavata environment](https://github.com/airavata-courses/spring17-devops/issues/7)


# Associated Code(s) on GitHub
- [github.com/.../Vagrantfile]() by [Anuj](https://github.com/anujbhan)

- [github.com/.../airavata-ansible/.../local_airavata_deploy.yml]() by [Anuj](https://github.com/anujbhan)

- [github.com/.../airavata-docker/.../Dockerfiles](https://github.com/ajinkya-dhamnaskar/airavata/tree/airavata-docker/dev-tools/docker) by [Ajinkya](https://github.com/ajinkya-dhamnaskar)


# References used for study
[For Vagrant development in IDE]
- [Eclipse Vagrant Tooling](https://marketplace.eclipse.org/content/eclipse-vagrant-tooling) for Eclipse IDE
- [Jetbrains Vagrant Plugin](https://plugins.jetbrains.com/plugin/7379-vagrant) for IntelliJ Idea IDE

[For Docker Development in IDE]
- [Eclipse Docker Tooling](https://marketplace.eclipse.org/content/eclipse-docker-tooling) for Eclipse IDE
- [Jetbrains Docker Integration](https://plugins.jetbrains.com/plugin/7724-docker-integration) for IntelliJ Idea IDE


[For debugging deployed code base in Vagrant box]
- [Enable additional debugging output from Ansible and Vagrant?](https://serverfault.com/a/611070)

- [debug code on a remote server (or in vagrant box)](https://www.dev-metal.com/debug-code-remote-server-vagrant-box-phpstorm/)

- [Debugging a Web-app in a Vagrant-provisioned VirtualBox VM](http://www.papayasoft.com/2013/02/25/debugging-vagrant-virtualbox-vm-netbeans/)

- [intellij-support.jetbrains: Debugging on Vagrant](https://intellij-support.jetbrains.com/hc/en-us/community/posts/206595125-Debugging-on-Vagrant)

- [Getting the remote debugger to work with a java process running in a vagrant vm](https://gist.github.com/happysundar/8704161)

- [confluence.jetbrains: Configuring a Vagrant VM for Debugging](https://confluence.jetbrains.com/display/PhpStorm/Configuring+a+Vagrant+VM+for+Debugging)

[For debugging deployed code base in Docker Container]
- [Docker Logs - Docker Logging drivers](https://docs.docker.com/engine/reference/commandline/logs/)

- [5 Methods of Logging in Docker](https://www.loggly.com/blog/top-5-docker-logging-methods-to-fit-your-container-deployment-strategy/)

- [Docker Logging Strategies](https://www.loggly.com/docs/strategies-for-docker-logging/)
