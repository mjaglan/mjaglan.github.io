# Why containerize apps?

# Problem Statement
Running two or more Microservices in isolation on same or different hosts in a cost effective way.

# Possible Solutions

There are two approaches which can handle this problem -
-	Run each Microservices inside a ```virtual machine```. Example: running Ubuntu VM in VirtualBox.
-	Run each Microservices inside an ```application container```. Example: running an app in Docker container.

# Virtual Machine vs Application Container

For comparison of these two approaches, I considered VirtualBox VMM and Docker container. VirtualBox was first released in 2007. Docker was first released in 2013.

![Docker containers vs. virtual machines](../images/docker-virtualbox/docker-vs-vm.png)

_Source Image: [Docker containers vs. virtual machines](https://newsroom.netapp.com/blogs/containers-vs-vms/)_


From above picture we see a typical VMM provides containment for Operating Systems and Docker provides containment for Applications. Based on this following are the Technology Differences between an Application Container like Docker and a Virtual Machine Manager like VirtualBox -


-	```Hypervisor```: The hypervisor requirements for VMs are large and are inevitably tied to where the hypervisor is installed and its version. VM containers are also confined to a specific hypervisor type. There are plenty of conversion tools, but the conversion process has to happen before you can move from one hypervisor to another. Meanwhile, containers are hypervisor independent. Containers are sandboxed so that they can't interact with each other in order to preserve the integrity of data and keep them secure.

-	```Size```: Application deployments on a Docker container will be much smaller than its equivalent VM.  This means that provisioning, which requires the copy of the physical image file, is much faster with containers.

-	```Speed```: The difference in speed degradation from the host OS to the container is an important consideration.  There is a loss with VMs, while containers arguably have none -- excluding the small overhead for the Docker service. This means you will get a greater performance benefit with containers versus VMs. However, currently most Docker instances run on VMs. There may be no loss, but they inherit the degradation of the VM from its bare metal host.
 
-	```Host OS```: A VM is fully isolated from the host OS, where Docker has isolated memory and disk space, but it is leveraging the same Host OS kernel. Additionally, Docker does not support the breadth of host or container OS that a VM does. VM allows you to choose your own, but Docker is limited to both Linux host and container.

-	```Networking```: VMs have a broader set of networking capabilities including support for virtual networks, which can be snapshot along with a collection of networked VMs. But these are more relevant for desktop environments and less for application development. Application containers can also be networked, but the configuration is relatively more complex, and negates the portability aspects of an application.

-	```Images```: VM snapshots and container images are both ways to maintain a library of instances to be provisioned in the future. One feature of VMs that is not possible with Docker is the ability to suspend and maintain memory state. This actually can fast track full-stack deployments for enterprises and offers a unique ability to address application bugs. Docker supports PAUSE and UNPAUSE, but it is still only local to the active instance.

-	```Management```: Docker containers are treated like application components and versioned and stored in a repository. Docker containers are built of layers and have a lot of moving parts for which all changes need to be tracked. VMs change less frequently, but still need to be managed. It is likely that a typical setup of Docker will have more containers than the number of VMs in a typical setup of a hypervisor. For business users the big advantage of using containers is that they offer a consistent environment all the way from development to production. There's no risk of introducing errors when software is moved to a different machine as the same container is used by developers, testers and in production.

-	```Portability```: Because of size, hypervisor dependency and networking, VMs are not nearly as portable as Docker containers.  It also means that you are locked into a specific vendor's hypervisor. You can move, but it is not as seamless as provisioning a new instance of a Docker container anywhere you want without any overhead. Moving to new hardware or to a different cloud platform is also easier, since if the software is in a container it should run in exactly the same way wherever it is. 


###### While researching for technology comparison, I also learned the key motivations for inventing application containers versus VM boxes -

-	```Culture```: Docker tool is built by developers for developers. It was something that developers understood in their typical development flow and could access much easier. On the other hand, VMs were introduced at a time when IT had full control of all infrastructure, including development, and it has since been considered IT's domain. So in a world of DevOps/ IAC, application containers like Docker fits better.

-	```Process```: Docker is considered a development tool, not an enterprise IT tool, and it has been built with the development process in mind. It better supports release automation and full-stack deployments. Docker has better developer documentation, includes more integrations and a smoother flow. While managing VMs and disk images are a part of IT's domain in a traditional enterprise.

-	```Procurement Overhead```: Because VMs are tied to a hypervisor and are bigger, there is still typically a process to procure them. Because there is still a process to procure them, it does not support the modern DevOps delivery chain and keeps them in the IT domain. Docker, application containerization tool for developers, is designed to be a part of modern DevOps delivery chain.

-	```Price```: Docker is free and its enterprise tools are cheaper. The resulting costs is still less than the two largest providers of hypervisor technology. For this reason, and if the budget falls under development, Docker is a very attractive solution. The key to the popularity of containerization is that it offers the resilience and isolation of a virtual machine but a lighter footprint and lower licensing and maintenance costs. 


Docker's container software nearly always runs on a VM. But again, the VM is a set and forgets its infrastructure, so having Docker run on them builds more flexibility. Additionally, if you choose to run containers on bare metal, it may not be a common implementation, but is a huge benefit to performance.

###### So next question is when to use application containerization?

In an ideal deployment container-based applications are delivered as a series of stateless microservices vs. the more traditional monolithic model found with virtual machines. With that context here are three scenarios to consider when deciding where to deploy your application -

-	_If ```you're starting from scratch on a new application (or rewriting an existing app from the ground up)```, and you're committed to writing it around a microservices-based architecture then containers are a no brainer. Companies will leave their existing monolithic apps in place, while they develop the next version using Docker containers and microservices. By leveraging Docker, companies can accelerate application development and delivery efforts, while creating code that can be run across almost any infrastructure without modification._

-	_You are committed to developing software based on microservices, but rather than wait until an app is completely rewritten, ```you want to begin gaining benefits of Docker immediately```. In this scenario, customers will "lift and shift" an existing application from a VM into a Docker container. With the monolithic application running in a container, the development teams can start breaking it down piece by piece. They can move some functions out of the monolith, and begin deploying them as loosely coupled services in Docker containers. The new containers can interact with older, legacy applications as necessary, and over time the entire application is deconstructed, and deployed as a series of portable and scalable services inside Docker containers._

-	_There are cases much like the second case, where companies want some benefits that Docker offers, and they ```move monolithic apps from VMs to containers with no intention of ever rewriting them```. Typically these customers are interested in the ```portability aspect that Docker containers offer``` out of the box. Imagine if your CIO came to you and said "Those 1,000 VMs we got running in the data center, I want those workloads up in the cloud by the end of next week." That's a daunting task even for the most hardcore VM ninja. There just isn't good portability from the data center to the cloud, especially if you want to change vendors. Imagine you have vSphere in the datacenter and the cloud is Azure - VM converters be what they may. However, with Docker containers, this becomes a pretty pedestrian effort. Docker containers are inherently portable and can run in a VM or in the cloud unmodified, the containers are portable from VM to VM to bare metal without a lot of heavy lifting to facilitate the transition._


# Conclusion

By comparing the container stack with its predecessors, we learned that containerization is the fastest, most resource-effective, and most secure setup we know to date. Containers are isolated instances that run your applications. These lightweight instances can be replaced, rebuilt, and moved around easily. This allows us to mirror the production and development environment and is a tremendous help in the continuous integration and delivery process. The advantages containers can provide are so compelling that they're definitely here to stay.

# Link to code repository

-	[Docker version of running cowsay game inside ubuntu:14.04](https://hub.docker.com/r/mjaglan/image-0/)

# References
[Technical Breakdown: Application Containers vs Virtual Machines]
-	https://betanews.com/2016/11/07/containerization-breakdown/

-	http://searchcloudapplications.techtarget.com/tip/Why-use-Dockers-container-software-when-VMs-do-the-job

-	http://www.informationweek.com/strategic-cio/it-strategy/containers-explained-9-essentials-you-need-to-know/a/d-id/1318961

-	https://resources.codeship.com/hubfs/resources_PDFs/Codeship_Why_Containers_and_Docker_are_the_Future.pdf?t=1452849717341

-	https://xebialabs.com/the-ultimate-devops-tool-chest/containerization/

-	http://searchcloudapplications.techtarget.com/tip/Five-development-containers-to-consider-that-arent-Docker

-	https://blog.risingstack.com/operating-system-containers-vs-application-containers/

[Performance: Docker Tool vs Virtual Box VMM]
-	https://www.slideshare.net/Flux7Labs/performance-of-docker-vs-vms

-	https://caleblloyd.com/hardware/docker-performance-bare-metal-vs-virtual/

-	http://mspmentor.net/technologies/docker-vs-virtual-machines-understanding-performance-differences

-	https://www.htpcbeginner.com/what-is-docker-docker-vs-virtualbox/



