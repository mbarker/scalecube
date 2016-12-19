# ScaleCube

[![Build Status](https://travis-ci.org/scalecube/scalecube.svg?branch=master)](https://travis-ci.org/scalecube/scalecube)
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/io.scalecube/scalecube-cluster/badge.svg)](https://maven-badges.herokuapp.com/maven-central/io.scalecube/scalecube-cluster)
[![Join the chat at https://gitter.im/scalecube/Lobby](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/scalecube/Lobby?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

ScaleCube, the art of scaling, in microservice architecture scalecube is a strategy in which components can scale on X, Y, Z axis. 
ScaleCube project provides the tools to develop, test and scale microservice components in distributed manner with ease.

The project focuses on ensuring that your application realises the full potential of the [Reactive Manifesto](http://www.reactivemanifesto.org/), 
while delivering a high productivity development environment, and seamless production deployment experience.

## Features

ScaleCube is designed as an embeddable library for the Java VM. It is built in a modular way where each module independently 
implements a useful set of functionality and provides foundation for higher level features by abstracting away lower level concerns.

Next is described modules in the top to bottom order from the higher level features to the low level components.

### MICROSERVICES

ScaleCube Services provides a low latency Reactive Microservices library for peer-to-peer service registry and discovery 
based on gossip protocol ad without single point-of-failure or bottlenecks.

User Guide:

* [Services Overview] (http://scalecube.io/services.html)
* [Defining Services] (http://scalecube.io/user-reference/services/DefineService.html)
* [Implementing services] (http://scalecube.io/user-reference/services/ServiceImplementation.html)
* [Provisioning Clustered Services] (http://scalecube.io/user-reference/services/ProvisionClusterServices.html)
* [Consuming services] (http://scalecube.io/user-reference/services/ConsumingServices.html)

### CLUSTER

ScaleCube Cluster is a lightweight decentralized cluster membership, failure detection, and gossip protocol library. 
It provides an implementation of [SWIM](http://www.cs.cornell.edu/~asdas/research/dsn02-swim.pdf) cluster membership protocol for Java VM.
It is an efficient and scalable weakly-consistent distributed group membership protocol based on gossip-style communication between the 
nodes in the cluster. Read my [blog post](http://www.antonkharenko.com/2015/09/swim-distributed-group-membership.html) with distilled 
notes on the SWIM paper for more details.

It is using a [random-probing failure detection algorithm](http://www.antonkharenko.com/2015/08/scalable-and-efficient-distributed.html) which provides 
a uniform expected network load at all members. 
The worst-case network load is linear O(n) for overall network produced by running algorithm on all nodes and constant network 
load at one particular member independent from the size of the cluster.

ScaleCube Cluster implements all improvements described at original SWIM algorithm paper, such as gossip-style dissemination, suspicion mechanism 
and time-bounded strong completeness of failure detector algorithm. In addition to that we have introduced support of additional SYNC mechanism 
in order to improve recovery of the cluster from network partitioning events.
  
Using ScaleCube Cluster as simple as few lines of code:
 
``` java
// Start cluster node Alice as a seed node of the cluster, listen and print all incoming messages
ICluster alice = Cluster.joinAwait();
alice.listen().subscribe(msg -> System.out.println("Alice received: " + msg.data()));

// Join cluster node Bob to cluster with Alice, listen and print all incoming messages
ICluster bob = Cluster.joinAwait(alice.address());
bob.listen().subscribe(msg -> System.out.println("Bob received: " + msg.data()));

// Join cluster node Carol to cluster with Alice (and Bob)
ICluster carol = Cluster.joinAwait(alice.address());

// Send from Carol greeting message to all other cluster members (which is Alice and Bob)
carol.otherMembers().forEach(member -> carol.send(member, Message.fromData("Greetings from Carol")));
```

You are welcome to explore javadoc documentation on cluster API and examples module for more advanced use cases.

### TRANSPORT

The Transport module is communication layer of nodes and service. its main goal is to deal with managing message exchange

Web Site: [http://scalecube.io](http://scalecube.io/)

## Project Status

You are more then welcome to join us. Your [feedback](https://github.com/scalecube/scalecube/issues) is welcome.
or just show your support by granting us a small star :)

## Support

Chat with us or get support: https://gitter.im/scalecube/Lobby

## Maven

Binaries and dependency information for Maven can be found at 
[http://search.maven.org](http://search.maven.org/#search%7Cga%7C1%7Cio.scalecube.scalecube).

To add a dependency on ScaleCube Services using Maven, use the following:

``` xml
<dependency>
  <groupId>io.scalecube</groupId>
  <artifactId>scalecube-services</artifactId>
  <version>x.y.z</version> 
</dependency>
```

To add a dependency on ScaleCube Cluster using Maven, use the following:

``` xml
<dependency>
  <groupId>io.scalecube</groupId>
  <artifactId>scalecube-cluster</artifactId>
  <version>x.y.z</version>
</dependency>
```

To add a dependency on ScaleCube Transport using Maven, use the following:

``` xml
<dependency>
  <groupId>io.scalecube</groupId>
  <artifactId>scalecube-transport</artifactId>
  <version>x.y.z</version>
</dependency>
```

## Bugs and Feedback

For bugs, questions and discussions please use the [GitHub Issues](https://github.com/scalecube/scalecube/issues).

## License

[Apache License, Version 2.0](https://github.com/scalecube/scalecube/blob/master/LICENSE.txt)
