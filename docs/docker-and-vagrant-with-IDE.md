# Integration of Docker and Vagrant tools with popular IDEs 

# Problem Statement
In continuation with [previous work](./dev-environment.md), explore capabilities of Docker and Vagrant in that how well it integrates with an IDE. It should allow developer to edit code, compile, deploy locally, test, and debug the system.

# Possible Solutions
For this experiment, popular IDEs like Eclipse and IntelliJ Idea are considered. For Eclipse IDE there are following options - 

Feature | Options
------- | -------
Docker | [Doclipser](https://marketplace.eclipse.org/content/doclipser), [Eclipse Docker Tooling](https://marketplace.eclipse.org/content/eclipse-docker-tooling)
Vagrant | [Vagrant-Plugin](https://marketplace.eclipse.org/content/vagrant), [Eclipse Vagrant Tooling](https://marketplace.eclipse.org/content/eclipse-vagrant-tooling)


For IntelliJ IDEA Ultimate Edition IDE there are following options -

Feature | Options
------- | -------
Docker | [Docker-Integration](https://plugins.jetbrains.com/plugin/7724-docker-integration)
Vagrant | [Vagrant-Plugin](https://plugins.jetbrains.com/plugin/7379-vagrant)

# Solution Evaluations

### Docker plugins for Eclipse and IntelliJ Idea
Doclipser and "Eclipse Docker Tooling (EDT)" were compared for Eclipse IDE. EDT is very stable, more rich in features, easier to use. When running dockerfile from IDE, project runs as Docker-project (instead of gradle project or maven project). EDT tool is very well supported. Similarly for IntelliJ Idea IDE, there is [Docker-Integration](https://plugins.jetbrains.com/plugin/7724-docker-integration) plugin which is the only choice available for that IDE and is also feature rich.

### Vagrant plugins for Eclipse and IntelliJ Idea
I also compared Vagrant Plugin and Eclipse Vagrant Tooling (EVT). None of them supports intellisense, code highlighting, and has [very simple features](https://developers.redhat.com/blog/2015/12/22/using-vagrant-tooling-in-eclipse/) to start, stop, suspend and run the VM which can be done from external CLI tool also. Running docker container via vagrant is more effortful than simple running a dockerfile from eclipse IDE. The general Vagrant Plugin for IntelliJ Idea Ultimate Edition (and other JetBrains tools WebStorm, AppCode and Gogland) has also [very simple features](https://www.jetbrains.com/help/idea/2017.1/vagrant.html) which can be done from external CLI tool also.

### Exploring Vagrant-Scripting-Engine Capabilities
I also tried the standalone capabilities of Vagrant tool for cases where non-docker applications can be run inside custom virtual-machine. Vagrant is a higher-level wrapper that allows one to manage (i.e., create, configure and use) reproducible virtual environments in a uniform manner regardless of the hypervisor (eg. VirtualBox, VMware, Hyper-V, KVM). The use of vagrant is good when exact production-OS & installed application is required on local development machine. And if you are not using dockerized applications, using vagrantfiles can help save time when you have to reproduce a production like VM which has already running desired applications in it. Since Docker-machine provides a base boot2docker OS when running docker-containers on non-linux kernel based OS. If the production servers are linux serves, simply using docker tooling (without vagrant provided custom linux host) on local development machine will not make much difference with containerized applications. The example template for running a VM in headless mode via vagrant script is [here](https://github.com/mjaglan/vagrant-vbox-script-sample-1).

# Conclusion
Vagrant can bring its own custom VM, on top of which it runs dockerized application (i.e., docker container). If custom VM is not specified inside vagrantfile, vagrant scripting engine fall-backs to boot2docker VM (a minimal linux OS provided by docker installation), on top of which it runs dockerized application. Using docker standalone on Windows/ MacOS requires installation of Docker-Machine which has boot2docker VM running, on top of which it runs dockerized application. So for dockerized applications, only advantage of using vagrant on local dev machine is to specify custom OS variant (windows, or linux versions) which matches the exact production server (not just the OS kernel). I think if there are linux servers installed in production, then it is optional to use vagrant tooling (which can run dockerized application on custom OS). Assuming production servers are linux OS, we prefer to eliminate vagrant tool. And suggest using docker with an IDE (example IntelliJ Idea Ultimate Edition or Eclipse).

# Associated Issue(s) on GitHub
- [IAC: Docker and Vagrant integration with IDE](https://github.com/airavata-courses/spring17-devops/issues/8)


# Associated Code(s) on GitHub
- [github.com/mjaglan/vagrant-vbox-script-sample-1](https://github.com/mjaglan/vagrant-vbox-script-sample-1)

- [github.com/airavata-courses/spring17-devops/tree/feature-iac-dev-env/infrastructure/api-server/terraform/dev-env](https://github.com/airavata-courses/spring17-devops/tree/feature-iac-dev-env/infrastructure/api-server/terraform/dev-env)

 
# References Used for Experiment(s)
- [IntelliJ IDEA: Enabling Docker Support](https://www.youtube.com/watch?v=V_x7c8jjXJc)
- [Doclipser: How I've put Docker in your favorite IDE](https://youtu.be/8tjpFzTVDpY)
- [Getting started with Eclipse-Docker-Tooling](https://youtu.be/_Xms1mK5xxE)
- [The State of Docker and Vagrant Tooling in Eclipse](https://youtu.be/eK11zKJ21F0)
- [RedHat: using-vagrant-tooling-in-eclipse](https://developers.redhat.com/blog/2015/12/22/using-vagrant-tooling-in-eclipse/)
- [JetBrainsTV: Vagrant Support](https://www.youtube.com/watch?v=f7Kss62DHhw)
- [IntelliJ IDEA: Vagrant Support](https://www.jetbrains.com/help/idea/2017.1/vagrant.html)

