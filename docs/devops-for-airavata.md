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
IntelliJ Idea IDE | [Vagrant-Plugin](https://plugins.jetbrains.com/plugin/7379-vagrant)

For Docker Development in IDE following tool is considered -

Feature | Options
------- | -------
Eclipse IDE | [Eclipse Docker Tooling](https://marketplace.eclipse.org/content/eclipse-docker-tooling)
IntelliJ Idea IDE | [Docker-Integration](https://plugins.jetbrains.com/plugin/7724-docker-integration)

For debugging deployed code base in Vagrant box following approach is considered - 

- [Vagrant Synced folders](https://www.vagrantup.com/docs/synced-folders/) for centralized application logs on Host Machine
- [Vagrant SSH](https://www.vagrantup.com/intro/getting-started/up.html) to inspect application process in Guest Machine

For debugging deployed code base in Docker Container following approach is considered -
- Logging via the Application
- Logging via Data Volumes 
- Logging via the Docker Logging Driver 
- Logging via a Dedicated Logging Container
- Sidecar Approach



# Solution Evaluations

### For Vagrant development in IDE


### For Docker Development in IDE

### For debugging deployed code base in Vagrant box

### For debugging deployed code base in Docker Container

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


# References used for study
[For Vagrant development in IDE]
-	abc

[For Docker Development in IDE]
-	 abc

[For debugging deployed code base in Vagrant box]
-	Abc

[For debugging deployed code base in Docker Container]
-	Abc

