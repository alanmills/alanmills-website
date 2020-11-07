# The Principles of Microservices 
**Author:** [Alan Mills]
**Date:** [08 August 2017 16:05]
**Tags:** [Microservices]
**Status**: Draft

O'Reilly Media video [The Principles of Microservies]() by [Sam Newman]() 

## Introduction

### Advantages of Microservices
* Organisational alignment
* Release Functionality Faster
* Independent Scaling
* Easier to focus on security concerns
* Adopt technology Faster
* Embrace Uncertainty in the digital age

Microservices follows the hexagonal architecture pattern which follows [Conway's Law]() that systems follow the archtiecture of the organisation.  By allowing each service to do one thing well and independent of the other services, you reduce the conflict within the organisation.

The Microservice provides a versioned public API that is used by consumers of the system, the internals are protected from the consumer and can be changed as the demands on the services change.

[The Art of Scalability: Scalable Web Architecture, Processes, and Organizations for the Modern Enterprise, Second Edition](https://www.safaribooksonline.com/library/view/the-art-of/9780134031408/) states a system can be scaled by:
* Horizontal Duplication
* Data Partitioning
* Functional Decomposition - **Microservices** is an example of Functional Decomposition

Enable Segregation Models - as we can treat each service independently, we can apply the relevant threat modeling and mitigations as appropriate to each service.  While in a traditional monolith archtiecture, we have to apply the same approach across the whole system, often a hardened outer layer, and more often than not a soft inner.

Each Microservice can use different technology which is most relevant to the service and team implementing the service.

### Disadvantages of Microservices
* Many, many options
* Takes time to get there
* Testing is more complex
* Monitoring is more complex
* Resiliency is not free
* Lots more infrastructure
* Distributed Systems are hard

To be able to work well with Microservices, there is a need for automated deployment, following the principles of DevOps.  If your organisation has not got this experience, there is a lot of up-front investment that can be difficult for an organisation to accept.

Monitoring the system is much more diffiuclt because instead of just monitoring the monolith, now you have to do this for each service and being able to monitor the system as a whole, a combination of services.

Micrservices is a distributed system and as such there is now lots of infrastructure to rely on and manage.  This can make the system more unreliable, especially as you develop your maturity with the architecture change.

## Principles of Microservices
These are the things if you have to do well to be successful with Microservices.
Ensure that these principles align with your organisation goals before using Microservices as unless these are aligned you will be unsuccesful.

### Principle 1 - Modelled Around Business Domain
We should model the system and development team around an area of the business.  This will allow a lay-person to be able to look at the structure of the system and understand what each area of the system does.

Key books for this are:
* [Domain-Driven Design: Tackling Complexity in the Heart of Software](https://www.safaribooksonline.com/library/view/domain-driven-design-tackling/0321125215/)
* [Implementing Domain-Driven Design](https://www.safaribooksonline.com/library/view/implementing-domain-driven-design/9780133039900/)

### Principle 2 - Culture of Automation
We need to build out a continuous delivery pipeline, where every commit is a potential release candidate.  Through automation the code goes through a rigorous deployment and test process and if successful the code is deployed to production.  This can be a problem for teams as they change their mindset to work in this way.

You require infrastructure as code services e.g.
* Infrastructure - AWS, Azure, VMware, 
* OS Configuraiton - Ansible, Chef, Puppet,
* Gold Images - Packer e.g. AWS AMI with Infrastucture and OS Configuraiton built
* Platforms - PaaS (heroku), CaaS (Docker, etc), IaaS (AWS)
* Declarative Environment Provisioning - provision the system in different environments, dev, test, prod, availability zones, etc.  Docker Compose can be usef for this.  Terraform is a more comprehensive tool for doing this (compared to docker-compose)
* Automated Testing - Follow the test pyramid and think about types of agile testing

### Principle 3 - Hide Implementation Details
This is tricky but essential to enable services to evolve and change independetly of each other.

As outlined in the article [Martin Fowler, BoundedContext](https://www.martinfowler.com/bliki/BoundedContext.html), when we provide an API for our service, we must only expose the details that are needed by a consumer.  So for instance in the Martin Fowler's article, The Support Context should not expose the Support Ticket as this is an internal artifact to the Support service.

We should never share DBs between services, you loose cohession and protection of internal details.

Use protocols that are flexible to change, XML and JSON are good for this.  And only bind to the parts of the interface that you need to consume, that way when the API changes outside of what you need, it will not break your service.

Be careful of client libraries, while it is useful to have a DRY library for patterns such as circuit breaker, these libraries can become god components that contain logic that should be in your service.  You know when this has happened when you have to publish a new version of your client library with each version of your service and if they don't use your new library, they cannot communicate with your service.

### Principle 4 - Decentralize All The Things
We want Microservices to give us autonomy, therefore enable teams to do what they need to do, avoid having to coordinate with lots of people, remove command-and-control structures.  Enable self-service and use the owner-operator model.  Use the internal open-source model where everyone can create a pull request with a change and submit back to the owning team.

This is not a free lunch, this is not easy but there are great benefits.

**Beware smart middleware**  the use of middleware, like RabbitMQ but do not have a team managing this, implementing knowledge of your system, using Enterprise Service Bus with services like conical datamodels.

#### Long-running processes
So what about long running-processes, e.g. the scenario when you register a new user and want to action a number of activities including sending emails, sending a welcome pack through the mail, etc.

You can use a couple of approaches:
* Orchestration
* Chorography

**Orchestration** this has the benefit of having the whole business process in once place, the negative is that it can become a God process, draining the logic from the other processes.

**Chorography** in this model, each step does not know anything about the other parts of the business processes.  This is normally implemented using events where other services are listening for the event.  In this case the **customer created** event that is generated by the **Customer Service**.  The architecture pattern is great for being able to add new services without having to update other parts of the system.

This architecture can have issues too, for instance a customer may get charged for a service that they are not getting provided and vice-versa when events have not been processed by all parts of the system.  This forces you to build reconciliation systems that work to the side of the event to make sure that the whole business process has been implemented.  This can be done by monitoring log files.

This event based system can become very difficult to observe and understand what is happening.

### Principle 5 - Deploy Independently  **This is the most important principle**
Have one service, per OS container as it reduces the side-effects that occure by deploying multiple services to the same host.  These side effects can be resource starvation, OS version, library versions, etc.  It can also be accessing internal details of other services.

Container services like Docker is a great way to support this and have efficient use of infrastructure.

How do I know that I have not broken another service when I reliease an update?  One solution is the use of **Consumer-Driven Contracts**, this is a high-trust model and therefore, only works within organisations.  Using *Consumer-Driven Contracts*, each consumer of your service develops a set of tests that run in your test cycle.  These tests explicitly test the expectations that your consumer has on your service e.g. if I do this, I expect this outcome.

Over time you will need to make **breaking changes**, there are two approaches to this:
* Co-Existing Service Versions - deploy different versions of your service to different locations
* Multiple Endpoints - have multiple end-points implemented in your service

Where you can, use **Multiple Endpoints** as it is much easier to manage the different endpoints within one code base than trying to main changes across multiple branches of your code base.  However, there are times where you will have to use *Co-Existing Service Versions* when working with 3rd-parties.

### Principle 6 - Consumer First
It is too easy to have a inside-out approach to service development.  Start with focusing on your consumer of your service, have a forum where your consumers can interact with you on what the service needs to do and have an ongoing conversation so that as you consinder how to evolve the service, the voice of your customer is part of the process.  Having **Consumer-Driven Contracts** is a useful tool for helping you understand what your constomers need of your service but it is only part of the *conversation*.

Think about **high-level standards for API design**, this ensure that across the organisation there is a consistency on how to use services.  An example of how do this has been published, [Paypal API Standards](https://github.com/paypal/api-standards/blob/master/api-style-guide.md).

It can be useful to document your interface, tools like [Swagger](https://swagger.io) can be extreemly useful for this.

**API Gateways** are very popular and one of the most useful things that API Gateways provides is API key management, these keys can be used to monitor who is using your service and how they are using your service e.g. one service may be making a large number of calls that would be better supported through a batch interface.

**Service discovery** DNS is often used for this however, DNS can be slow to propigate changes and has a simple query interface that can be limitting.  Organisation will often move to other services like:
* [Apache Zookeeper](https://zookeeper.apache.org).
* [CoreOS etch](https://coreos.com/etcd/)
* [HashiCorp Consul](https://www.consul.io)
* [Martin Fowler HumaneRegistry](https://martinfowler.com/bliki/HumaneRegistry.html) - use a Wiki

### Principle 7 - Isolate Failure
**Microservices are not reliable by default** as the failure service has expanded vs a traditional monolith.  If my service is made up of four services, if one of the services is down the whole system is down.  This failure may not be the individual nodes but the networking between the nodes.

You need to understand the implication of failure, for you and for your consumers.  The cascade of failure effect can be very painful.

One of the worst types of failure is **Failiing... slowly!** as this can consume resources across other parts of the system which can prevent those services from handling additional requests.  This can be addressed by taking mitigations like:
* Reducing timeouts - people do not wait long, so neither should your application; make this a few seconds
* Bulkheads - have a seperate threadpool per down-stream application, that way if one application fails it does not consume the threads for other (working) downstream applications
* Circuit Breakers - If a downstream service fails regular enough within a period of time, instead of failing slowly, fail fast.  With this part of the system being unavailable you can take action e.g. disabling a part of the UI to let users of the system know that that part of the system is unavailble.  Try and make these circuit breakers self-healing, where it tries to reconnect without flodding the system and bringing the service back on-line.

### Principle 8 - Highly Observable
Have monitoring standards:
**System**
* Log requests
* Response times
* Error codes
**Infrastructure**
* CPU
* Disk Space
* Memory
* IO

Some systems that you can use to monitor infrastucture include:
* Docker Stats
* collectd
* nsclient++

Have health-check pages for your services, make sure these are not exposed to the outside world, so it's a good idea to put this behind protected URLs, specific ports that can be controlled through network policies, etc.  These pages should have information like:
* Number of application errors
* Number of successful service requests (200)
* Number of not modified requests (304)
* Number of not found requets (404)
* Number of internal server error (500)
* Total number of requets

In a small environment, you can monitor each individual machine however, as the number of nodes growes this does not work and you need to **aggregate** the data.

## Logs
Take logs of the nodes using services like:
* fluentd
* logstash

The use **Elasticsearch** and **Kibana** to view the logs

## Stats
* [Graphite](https://graphiteapp.org) - can be good on-premise but can be difficult to manage
* [New Relic](https://graphiteapp.org) - A service for managing stats data

Make sure you have network monitoring too.

## Semantic Monitoring
As well as system monitoring that is checking the nodes are working but should you be waking your team at 02:00?  An approach to answer this is to use **semantic monitoring**.  Semantic monitoring is when you put a synthathised user event through your system e.g. purchase a book to check everything works.  Be careful with this as you could accidently cause the warehouse to send a lot of books to the development team!

## Correlation IDs
Multiple systems can be involved in making an action occurs, the initiating service creates an ID that is passed to the called systems, those systems then log that ID so that the action can be traced througout the system.  You can then use `grep` to view all log entries associated with that action.

## Reporting
Different stakeholders associated with your system will want to monitor different asspects of the system.  Therefore, you will want to use tools like [Dashing](http://dashing.io) or [DataDog](https://www.datadoghq.com).

# When Should You Use Microservices?
I think that you should use the principles of Microservices from the begining but start simple. Trying to build independent services from the outset can be extreemly difficult as you do not have a strong understand of the domain.  All of the poster children of Microservices have used the approach as a solution to an existing problem, spliting a well understood monolith appart to get the benefits of Microservices with a well understood problem.

In addition to the domain problem, Microservices is a distributed system pattern that requires distributed system maturity including automation, monitoring, isoltion, etc.  Without this, you can run into a lot of problems and should be taken step-by-step.

There are examples where even experienced teams have started building Microservices and found that the topoligy of the system becomes a cats-cradle where changes have to ripple across multple services.  The solution is often to pull the services together into single larger services until the real boundaries are understood. 
