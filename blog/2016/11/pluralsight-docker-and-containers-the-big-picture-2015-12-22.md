Review of Pluralsight course: Docker and Containers: The Big Picture (2015/12/22)
=================================================================================

**Author:** Alan Mills  
**Date:** [15 July 2016 00:29](/blog/history/2016-07.md)  
**Tags:** [Pluralsight](/blog/categories/pluralsight.md), [Docker](blog/categories/docker.md)
**Status**: Draft

The Pluralsight course [Docker and Containers: The Big Picture](https://app.pluralsight.com/library/courses/docker-containers-big-picture/table-of-contents), published on 22 December 2015 is given by [Nigel Poulton](http://blog.nigelpoulton.com).


**Gives an overview of Docker echosystem but does not have any hands on exercieses**  For me the course was a bit long for what it covered however, as Nigel is trying to give an overview for a wide range of skills and experience he does a good job.


Overview
--------
Nigel presents an enthusiastic overview of Docker through the chapters of the course:
* Course Overview
* Course Introduction
* What Are Containers?
* What Is Docker?
* Preparing to Thrive in a Container World
* What Kind of Work Will Containers Do?
* Docker Hub and Other Container Registries
* Are Docker and Containers Ready for Production and the Enterprise?
* What Is Container Orchestration All About?
* What Next?


Docker is already being used by developers, therefore if you an operations person then you need to make sure that you have the

What Kind of Work Will Containers Do?
-------------------------------------
Containers unlike Virtual Machines, it's no so easy to lift and shift from a physical environment to a virtual one as Containers are not a virtual representation of what came before.

When moving to Containers, to take advantage of the architecture paradigm we need to redevelop our applications.  The advantage for doing so is allows us to develop apps that are:
* Modern microservices style apps
* Scalable
* Self-healing
* Portable


However it takes time, effort and money to do so.

https://labs.ctl.io/docker-hub-top-10/

Docker containers are persistent and the back-end is pluggable:
* Stopping a container does not wipe its Data
* Restarting a container brings back data

Docker Hub and Other Container Registries
-----------------------------------------
There are other container registers, the offical register is [Docker hub](https://hub.docker.com), you have to sign-up for a Docker account before.

You can **pull** containers from the registry.  If you create your own container you can **push** this up and mark the registry as **public** or **private**.

As well as cloud registries, you can also have an on-premise solution, a couple of examples are:
* Docker Trusted Registry (DTR)
* Quay Enterprise


Docker Content Trust allows you to validate the content of the container and the author through the **Docker Content Trust**.


Contain registries are central to the automated Workflow:
Write app code -> push to VC -> run CI build and test process -> push to container registry -> deploy from container registry (to dev, QA, prod, etc) -> start using the deployed app.  This is covered in the Pluralsight course [Integrating Docker with DevOps Automated Workflows](https://app.pluralsight.com/library/courses/integrating-docker-with-devops-automated-workflows/table-of-contents) which is part of the advanced track of the [Container Management using Docker](https://app.pluralsight.com/paths/skills/docker) learning path.


Are Docker and Containers Ready for Production and the Enterprise?
------------------------------------------------------------------
This chapter is about helping you make a decision if Docker is ready to support your environment.

Docker stated that that the Docker Engine was production ready at 1.0 (released 2014/07/009)
* Experimental - daily releases with features that can come/go and or be unstable.
* Stable - Released every two months
* Commercially Supported - every six months, this version can be bought with a support contract.

Docker Swarm again was marked as production ready at 1.0 (released )
Docker Content Trust - this really help with increasing the security of your deployments as you're using containers that are from trusted sources.

Cloud solution is strong and the on-premise solution is growing quickly with a particular focus on the needs of the enterprise.

The wider ecosystem is growing very quickly with startups to existing enterprise providers and therefore, there is likely to be services in the ecosystem that support your needs.

What Is Container Orchestration All About?
------------------------------------------
Container orchestration is a way to setup a number of containers each performing a role for our overall solution architecture.  Docker provides a number of tools to support this:
* Docker Machine - provisions docker hosts/engines
* Docker Compose - compose multi-container apps
* Docker Swarm - schedule containers over multiple docker engines
* Docker Totum - A UI over the other Docker services.

Some echo system contain orchestrators:
* Kubernetes - came out of Goolge, based on Borg the internal Google container management services
* Mesosphere DCOS - Commercial solution and there is a relationship with Azure
* CoreOS fleet, etcd...
* OpenStack Magnum
