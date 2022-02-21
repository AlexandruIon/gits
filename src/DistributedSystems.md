# Distributed Systems

# Table of contents
* [Introduction](#introduction)
  * [1.1 What is a distributed system?](#1.1-What-is-a-distributed-system?)
  * [1.2 Design goals](#1.2-Design-goals)
  * [1.3 Types of distributed systems](#1.3-Types-of-distributed-systems)
* [Architectures](#architectures)
  * [2.1 Architectural styles](#2.1-Architectural-styles)
  * [2.2 Middleware organization](#2.2-Middleware-organization)
  * [2.3 System architecture](#2.3-System-architecture)
* [Setup](#setup)

# Introduction

## 1.1 What is a distributed system?

**"A distributed system is a collection of autonomous computing elements
that appears to its users as a single coherent system."**

### Characteristic 1: Collection of autonomous computing elements

A fundamental principle is that nodes can act independently from each other.
(global clock/synchronization/coordination/group membership/open group/closed group)
overlay network (Structured overlay/Unstructured overlay)

### Characteristic 2: Single coherent system
distribution transparency

### Middleware and distributed systems
To assist the development of distributed applications, distributed systems are
often organized to have a separate layer of software that is logically placed on
top of the respective operating systems of the computers that are part of the
system.
![Middleware](../misc/distributed_systems/c1/fig1_1.png)
The distributed system provides the means for components of a single distributed application to communicate with each
other, but also to let different applications communicate. At the same time, it hides, as best and reasonably as possible, the differences in hardware and
operating systems from each application.</br>
In a sense, middleware is the same to a distributed system as what an operating system is to a computer: a manager of resources offering its applications
to efficiently share and deploy those resources across a network.
Next to resource management, it offers services that can also be found in most operating systems, including:
* Facilities for inter-application communication.
* Security services.
* Accounting services.
* Masking of and recovery from failures.</br>
Typical  middleware services:
* Communication - Remote Procedure Call (RPC)
* Transactions
* Service composition
* Reliability

## 1.2 Design goals
* make resources easily accessible
* hide the fact that resources are distributed across a network
* it should be open
* it should be scalable

### Supporting resource sharing
make it easy for users to access and share remote resources. (collaborate and exchange information)</br>
file-sharing peer-to-peer networks like BitTorrent.
### Making distribution transparent
hide the fact that its processes and resources are physically distributed across multiple computers possibly separated by large distances.</br>
![Transparency](../misc/distributed_systems/c1/fig1_2.png)

### Being open
An open distributed system is essentially a system that offers components that can easily be used by, or integrated into other systems.
#### Interoperability, composability, and extensibility
To be open means that components should adhere to standard rules that describe the **syntax** and **semantics** of what those components have to offer.
A general approach is to define services through **interfaces** using an **Interface Definition Language (IDL)**.
Interface definitions written in an IDL nearly always capture only the syntax of services(function names,type of parameters,return values).
The hard part is specifying precisely what those services do (specifications), that is, the semantics of interfaces.</br>

Proper specifications are **complete** and **neutral**.Complete means that everything that is necessary to make an implementation has indeed been specified.
Just as important is the fact that specifications do not prescribe what an implementation should look like; they should be neutral.</br>

**Completeness** and **neutrality** are important for **interoperability** and **portability**.
Interoperability characterizes the extent by which two implementations of systems or components from different manufacturers can co-exist and work together by merely relying
on each other’s services as specified by a common standard. Portability characterizes to what extent an application developed for a distributed system A can be executed, 
without modification, on a different distributed system B that implements the same interfaces as A. Also, it should be easy to add new components or
replace existing ones without affecting those components that stay in place **extensible**.

### Being Scalable
Scalability dimensions:
* **Size scalability**: A system can be scalable with respect to its size, meaning that we can easily add more users and resources to the system without  any noticeable loss of performance.
* **Geographical scalability**: is one in which the users and resources may lie far apart, but the fact that communication delays may be significant is hardly noticed
* **Administrative scalability**: is one that can still be managed even if it spans many independent administrative organizations. 

#### Size scalability
#### Geographical scalability
One of the main reasons why is difficult to scale distributed systems designed for local-area networks is that may of them are based on **synchronous communication**.
This approach generally works fine in LANs where communication between two machines is often at worst a few hundred microseconds.
However, in a wide-area system, we need to take into account that interprocess communication may be hundreds of milliseconds, three orders of magnitude slower

Communication in wide-area networks is inherently much less reliable than in local-area networks.In addition, we also need to deal with limited bandwidth

Wide-area systems generally have only very limited facilities for multipoint communication. In contrast, local-area networks often support efficient broadcasting mechanisms.
Such mechanisms have proven to be extremely useful for discovering components and services, which is essential from a management point of view. In wide-area systems, we need to develop
separate services, such as naming and directory services to which queries can be sent.

#### Administrative scalability
Across multiple, independent administrative domains. A major problem that needs to be solved is that of conflicting policies with respect to resource usage (and payment), management, and security.

To illustrate, for many years scientists have been looking for solutions to share their (often expensive) equipment in what is known as a **computational grid**. In these grids, a global distributed system is constructed as a federation
of local distributed systems, allowing a program running on a computer at organization A to directly access resources at organization B.

If a distributed system expands to another domain, two types of security measures need to be taken:
* the distributed system has to protect itself against malicious attacks from the new domain
* the new domain has to protect itself against malicious attacks from the distributed system

As a counterexample of distributed systems spanning multiple administrative domains that apparently do not suffer from administrative scalability problems, consider modern **file-sharing peer-to-peer networks**.
Oher examples - Skype/Spotify. What these distributed systems have in common is that **end users**, and not administrative entities, collaborate to keep the system up and running

#### Scaling techniques
* **scaling up**
* **scaling out** - _hiding communication latencies_, _distribution of work_, _replication_

##### Hiding communication latencies
Applicable in the case of geographical scalability. Idea: "try to avoid waiting for responses to remote-service requests as much as possible".
Essentially, this means constructing the requesting application in such a way that it uses only **asynchronous communication**.

In such cases (interactive apps/filling informs), a much better solution is to reduce the overall communication, for example, by moving part of the computation that is normally done at the server to the client process requesting the service.

##### Partitioning and distribution
taking a component, splitting it into smaller parts, and subsequently spreading those parts across the system.
Eg: Internet Domain Name System (DNS). The DNS name space is hierarchically organized into a tree of domains, which are divided into nonoverlapping zones
How the naming service, as provided by DNS, is distributed across several machines, thus avoiding that a single server has to deal with all requests for name resolution.

##### Replication
increases availability/helps to balance the load between components leading to better performance/geographical location

**Catching** is a special form of replication in the proximity of the client accessing that resource.

Catching/replication drawback that may adversely affect scalability. many copies -> **consistency problems**.
Replication therefore often requires some global synchronization mechanism.

### Pitfalls
Distributed systems differ from traditional software because components are dispersed across a network.</br>
False assumptions:
* The network is reliable
* The network is secure
* The network is homogeneous
* The topology does not change
* Latency is zero
* Bandwidth is infinite
* Transport cost is zero
* There is one administrator </br>
Reliable networks simply do not exist and lead to the impossibility of achieving failure transparency.<\br>
When discussing replication for solving scalability problems, we are essentially tackling latency and bandwidth problems.

## 1.3 Types of distributed systems
We make a distinction between 
* distributed computing systems
* distributed information systems
* pervasive systems (which are naturally distributed)

### High performance distributed computing
* Cluster computing - similar underlying hardware, closely connected by means of a high-speed local-area network. Same OS
* Grid computing - federation of computer systems (fall under a different administrative domain) and may be different when it comes to hardware/software/network topology.

From the perspective of grid computing, a next logical step is to simply _outsource_ the entire infrastructure that is needed for compute-intensive applications.
In essence, this is what **cloud computing** is all about: providing the facilities to dynamically construct an infrastructure and compose what
is needed from available services. Unlike grid computing, which is strongly associated with high-performance computing, cloud computing is much more
than just providing lots of resources.

#### Note - Parallel processing
High-performance computing more or less started with the introduction of **multiprocessor machines**. In this case, multiple CPUs are organized in such a way
that they all have access to the same physical memory.</br>
In contrast, in a **multicomputer system** several computers are connected through a network and there is no sharing of main memory.

![Multiprocessor vs Multicomputer architectures](../misc/distributed_systems/c1/fig1_6.png)

This shift also meant that many programs had to make use of message passing instead of modifying shared data as a means of communication and synchronization between threads
Unfortunately, message-passing models have proven to be much more difficult and error-prone compared to the shared-memory programming models.

#### Cluster computing
It became financially and technically attractive to build a supercomputer using off-the-shelf technology by simply hooking up a collection of relatively simple computers in a high-speed network.
In virtually all cases, cluster computing is used for **parallel programming in which a single** (compute intensive) **program is run in parallel on multiple machines**.

![Cluster computing](../misc/distributed_systems/c1/fig1_7.png)

Each cluster consists of a collection of compute nodes that are controlled and accessed by means of a single master node. The master typically
handles the allocation of nodes to a particular parallel program, maintains a batch queue of submitted jobs, and provides an interface for the users of
the system. As such, the master actually runs the middleware needed for the execution of programs and management of the cluster, while the compute
nodes are equipped with a standard operating system extended with typical middleware functions for communication, storage, fault tolerance, and so on.
Apart from the master node, the compute nodes are thus seen to be highly identical.

#### Grid computing
A characteristic feature of traditional **cluster computing is its homogeneity**.</br>
In grid computing systems: no assumptions are made concerning similarity of hardware, operating systems, networks, administrative domains, security policies, etc.</br>
A key issue in a grid-computing system is that resources from different organizations are brought together to allow the collaboration of a group of
people from different institutions, indeed forming a federation of systems. Such a collaboration is realized in the form of a **virtual organization**.</br>
Given its nature, much of the software for realizing grid computing evolves around providing access to resources from different administrative domains,
and to only those users and applications that belong to a specific virtual organization. For this reason, focus is often on architectural issues.

![Grid computing](../misc/distributed_systems/c1/fig1_8.png)

* fabric layer - interfaces to local resources at a specific site. (query the state and capabilities of a resource + resource management (locking resources))
* connectivity layer - communication protocols for supporting grid transactions that span the usage of multiple resources(protocols to transfer data/ access resources)
* resource layer - is responsible for managing a single resource - it uses the connectivity layer and calls directly the fabric layer interfaces 
* collective layer - handles access to multiple resources (resource discovery/allocation/scheduling of tasks/data replication)
* application layer - apps that operate within a virtual org</br>
Typically the collective, connectivity, and resource layer form the heart of what could be called a grid middleware layer.
These layers jointly provide access to and management of resources that are potentially dispersed across multiple sites.

#### Cloud computing
**utility computing** by which a customer could upload tasks to a data center and be charged on a per-resource basis. Utility
computing formed the basis for what is now called **cloud computing**.</br>
Cloud computing is characterized by an easily usable and accessible pool of virtualized resources.The link to utility computing is formed by the fact that cloud
computing is generally based on a pay-per-use model in which guarantees are offered by means of customized service level agreements (SLAs).

![cloud computing](../misc/distributed_systems/c1/fig1_9.png)

Clouds are organized in 4 layers:
* Hardware - at data center level
* Infrastructure - it deploys virtualization techniques (virtual storage and computing resources)
* Platform - 
* Application</br>
Cloud-computing providers offer these layers to their customers through various interfaces
* IaaS (hardware/infrastructure layer)
* PaaS (platform layer)
* SaaS (app layer)

### Distributed information systems
It is found in organizations that were confronted with a wealth of networked applications (servers+databases), but for which interoperability turned out to be a painful experience. Many of the existing
middleware solutions are the result of working with an infrastructure in which it was easier to integrate applications into an enterprise-wide information system.</br>
We can distinguish several levels at which integration can take place. Integration at the lowest level allows clients to wrap a number of requests, possibly for different
servers, into a single larger request and have it executed as a distributed transaction. The key idea is that all, or none of the requests are executed.</br>

As applications became more sophisticated and were gradually separated into independent components (notably distinguishing database components
from processing components), it became clear that integration should also take place by letting applications communicate directly with each other. This
has now lead to a huge industry that concentrates on Enterprise Application Integration (EAI).

#### Distributed transaction processing
we concentrate on database applications. operations on a database are carried out in the form of **transactions**. Programming
using transactions requires special primitives that must either be supplied by the underlying distributed system or by the language runtime
system. In a mail system, there might be primitives to send, receive, and forward mail.</br>
Ordinary statements, procedure calls, and so on, are also allowed inside a transaction. In particular, **remote procedure calls**
(**RPC**s), that is, procedure calls to remote servers, are often also encapsulated in a transaction, leading to what is known as a **transactional RPC**.

![Primitives for transaction](../misc/distributed_systems/c1/fig1_10.png)

In distributed systems, transactions are often constructed as a number of subtransactions, jointly forming a **nested transaction**.
The top-level transaction may fork off children that run in parallel with one another, on different machines, to gain performance or simplify programming.
Each of these children may also execute one or more subtransactions, or fork off its own children.

![Nested transaction](../misc/distributed_systems/c1/fig1_11.png)

Nested transactions are important in distributed systems, for they provide a natural way of distributing a transaction across multiple machines. They
follow a logical division of the work of the original transaction.</br>
In the early days of enterprise middleware systems, the component that handled distributed (or nested) transactions formed the core for integrating
applications at the server or database level. This component was called a **transaction processing monitor** or **TP monitor** for short. Its main task was
to allow an application to access multiple server/databases by offering it a transactional programming model, as shown in Figure 1.12. Essentially, the TP
monitor coordinated the commitment of subtransactions following a standard protocol known as **distributed commit**.

![TP monitor](../misc/distributed_systems/c1/fig1_12.png)

An important observation is that applications wanting to coordinate several subtransactions into a single transaction did not have to implement this
coordination themselves. This is exactly where middleware comes into play: it implements services that are useful for many applications avoiding that
such services have to be reimplemented over and over again by application developers

#### Enterprise application integration
The more applications became decoupled from the databases they were built upon, the more evident it became that facilities were needed
to integrate applications independently from their databases. In particular, application components should be able to communicate directly with each other
and not merely by means of the request/reply behavior that was supported by transaction processing systems.</br>
The main idea was that existing applications could directly exchange information.

![Middleware as communication](../misc/distributed_systems/c1/fig1_12.png)

Several types of communication middleware exist. With remote procedure calls (RPC), an application component can effectively send a request to another
application component by doing a local procedure call, which results in the request being packaged as a message and sent to the callee. Likewise, the
result will be sent back and returned to the application as the result of the procedure call.
As the popularity of object technology increased, techniques were developed to allow calls to remote objects, leading to what is known as **remote method invocations (RMI)**.
An RMI is essentially the same as an RPC, except that it **operates on objects instead of functions**.</br>
RPC and RMI have the disadvantage that the caller and callee both need to be up and running at the time of communication. In addition, they need
to know exactly how to refer to each other. This tight coupling is often experienced as a serious drawback, and has lead to what is known as **message-oriented
middleware**, or simply **MOM**. In this case, applications send messages to logical contact points, often described by means of a subject. Likewise,
applications can indicate their interest for a specific type of message, after which the communication middleware will take care that those messages are
delivered to those applications. These so-called **publish/subscribe** systems form an important and expanding class of distributed systems.

##### Enterprise application integration - on integrating apps
Supporting enterprise application integration is an important goal for many middleware products.
* File transfer (drawback - file format and layout, file management, update propagation-notification of clients aka trigger)
* Shared database (better than file transfer. drawback - design common data schema, many reads/updates leads to performance bottleneck)
* Remote procedure call 
  * In such cases it is not really the change of data that is important, but the execution of a series of actions)
  * In essence, an RPC allows an application A to make use of the information available only to application B, without giving A direct access  to that information
* Messaging
  * A main drawback of RPCs is that caller and callee need to be up and running at the same time in order for the call to succeed. However, in
    many scenarios this simultaneous activity is often difficult or impossible to guarantee.</br>
  
Middleware (in the form of a distributed system), however, can significantly help in integration by providing the right facilities such as support for RPCs or messaging.

### Pervasive systems
The distributed systems discussed so far are largely characterized by their stability: nodes are fixed and have a more or less permanent and high-quality
connection to a network.</br>
However, matters have changed since the introduction of mobile and embedded computing devices, leading to what are generally referred to as **pervasive systems**.
As its name suggests, pervasive systems are intended to naturally blend into our environment.</br>
What makes them unique in comparison to the computing and information systems described so far, is that the separation between users and system
components is much more blurred. There is often no single dedicated interface, such as a screen/keyboard combination. Instead, a pervasive system is often
equipped with many **sensors** that pick up various aspects of a user’s behavior. Likewise, it may have a myriad of **actuators** to provide information and
feedback, often even purposefully aiming to steer behavior.</br>
Three different types of pervasive systems:
* Ubiquitous computing systems (present, appearing, or found everywhere.)
* Mobile systems
* sensor networks

#### Ubiquitous computing systems
In a ubiquitous computing system we go one step further: the system is pervasive and continuously present. The latter means that a user will be continuously
interacting with the system, often not even being aware that interaction is taking place.</br>
Core requirements for a ubiquitous computing system:
* (Distribution) Devices are networked, distributed, and accessible in a transparent manner
* (Interaction) Interaction between users and devices is highly unobtrusive
* (Context awareness) The system is aware of a user’s context in order to optimize interaction
* (Autonomy) Devices operate autonomously without human intervention, and are thus highly self-managed
* (Intelligence) The system as a whole can handle a wide range of dynamic actions and interactions

# Architectures
Make a distinction between, on the one hand, the **logical organization** of the collection of software components, and on the other hand the actual **physical realization**.</br>
The organization of distributed systems is mostly about the software components that constitute the system. These **software architectures** tell us
how the various software components are to be organized and how they should interact.</br>
An important goal of distributed systems is to separate applications from underlying platforms by providing a **middleware layer**. Adopting such a layer
is an important architectural decision, and its main purpose is to provide distribution transparency.</br>
The actual realization of a distributed system requires that we instantiate and place software components on real machines.
The final instantiation of a software architecture is also referred to as a **system architecture**.

## 2.1 Architectural styles
Considering the logical organization of a distributed system into software components, also referred to as its **software architecture**.</br>
An **architectural style** is important. Such a style is formulated in terms of components, the way that components are connected to each other, the data exchanged between components, and finally
how these elements are jointly configured into a system.</br> 
A **component** is a modular unit with well-defined required and provided **interfaces** that is _replaceable_ within its environment.</br>
A **connector**, which is generally described as a mechanism that mediates communication, coordination, or cooperation among components.(flow of control and data between components)</br>
Using components and connectors, we can come to various configurations, which, in turn, have been classified into architectural styles:
* Layered architectures
* Object-based architectures
* Resource-centered architectures
* Event-based architectures

### Layered architectures
The basic idea for the layered style is simple: components are organized in a layered fashion where a component at layer Lj can make a **downcall** to
a component at a lower-level layer Li (with i < j) and generally expects a response. Only in exceptional cases will an **upcall** be made to a higher-level component.

![Layered Architecture](../misc/distributed_systems/c2/fig2_1.png)

Figure 2.1(a) shows a standard organization in which only downcalls to the next lower layer are made. This organization is commonly deployed in the case of **network communication**.</br>
Finally, a special situation is shown in Figure 2.1(c). In some cases, it is convenient to have a lower layer do an upcall to its next higher layer. A typical example is when an operating system signals the occurrence of an
event, to which end it calls a user-defined operation for which an application had previously passed a reference (typically referred to as a handle).

#### Layered communication protocols
A well-known and ubiquitously applied layered architecture is that of so called **communication-protocol stacks**.</br>
In communication-protocol stacks, each layer implements one or several **communication services** allowing data to be sent from a destination to one
or several targets. To this end, each layer offers an interface specifying the functions that can be called.

![Layered Communication protocol stack](../misc/distributed_systems/c2/fig2_2.png)

#### Application layering
Logical layering of applications:
* The application-interface level
* The processing level
* The data level

### Object-based and service-oriented architectures
In essence, each object corresponds to what we have defined as a component, and these components are connected through a procedure call mechanism.
In the case of distributed systems, a procedure call can also take place over a network, that is, the calling object need not be executed on the same machine as the called object.</br>

![Object Based arch style](../misc/distributed_systems/c2/fig2_5.png)

Object-based architectures are attractive because they provide a natural way of encapsulating data (called an object’s state) and the operations that can
be performed on that data (which are referred to as an object’s methods) into a single entity. The **interface** offered by an object conceals implementation
details, essentially meaning that we, in principle, can consider an object completely independent of its environment.</br>
This separation between interfaces and the objects implementing these interfaces allows us to place an interface at one machine, while the object itself
resides on another machine. This organization, which is shown in Figure 2.6 is commonly referred to as a **distributed object**.

![Remote Object](../misc/distributed_systems/c2/fig2_6.png)

When a client **binds** to a distributed object, an implementation of the object’s interface, called a **proxy**, is then loaded into the client’s address space.
A proxy is analogous to a client stub in RPC systems. The only thing it does is marshal method invocations into messages and unmarshal reply messages
to return the result of the method invocation to the client. The actual object resides at a server machine, where it offers the same interface as it does on the client machine.</br>
A characteristic, but somewhat counterintuitive feature of most distributed objects is that their state is not distributed: it resides at a single machine.
Only the interfaces implemented by the object are made available on other machines. Such objects are also referred to as **remote objects**.
One could argue that object-based architectures form the foundation of encapsulating services into independent units. **Encapsulation** is the keyword
here: the service as a whole is realized as a self-contained entity, although it can possibly make use of other services. By clearly separating various services
such that they can operate independently, we are paving the road toward **service-oriented architectures**, generally abbreviated as **SOA**s.</br>
In a service-oriented architecture, a distributed application or system is essentially constructed as a composition of many different services.</br>
The problem of developing a distributed system is partly one of **service composition**.

### Resource-based architectures
One of the problems with **service composition** is that connecting various components can easily turn into an **integration** nightmare.</br>
As an alternative, one can also view a distributed system as a huge **collection of resources** that are individually managed by components.
This approach has now been widely adopted for the Web and is known as **Representational State Transfer (REST)** Characteristics:
* Resources are identified through a single naming scheme
* All services offer the same interface, consisting of at most four operation
* Messages sent to or from a service are fully self-described
* After executing and operation at a service, that component forgets everything about the caller (**stateless execution**)
* 
![Rest operations](../misc/distributed_systems/c2/fig2_7.png)

The RESTful architecture has become popular because of its simplicity. whether RESTful services are better than where services are specified by means of **service-specific interfaces**. 
The simplicity of RESTful architectures can easily prohibit easy solutions to intricate communication schemes. One example is where distributed transactions are needed, which generally requires that services keep track of
the state of execution. On the other hand, there are many examples in which RESTful architectures perfectly match a simple integration scheme of services,yet where the myriad of service interfaces will complicate matters.

### Publish-subscribe architectures
Architecture in which dependencies between processes become as loose as possible.
Architecture in which there is a strong separation between **processing** and **coordination**.
The idea is to view a system as a collection of autonomously operating processes. In this model, **coordination** encompasses
the **communication** and **cooperation** between processes.

![Coordination](../misc/distributed_systems/c2/fig2_9.png)

Shared data spaces are often combined with event-based coordination: a process subscribes to certain tuples by providing a search pattern; when a
process inserts a tuple into the data space, matching subscribers are notified. In both cases, we are dealing with a publish-subscribe architecture.

![EventBased/SharedDataspace architectural style](../misc/distributed_systems/c2/fig2_10.png)

We have also shown an abstraction of the mechanism by which publishers and subscribers are matched, known as an **event bus**.</br>
An important aspect of publish-subscribe systems is that communication takes place by describing the events that a subscriber is interested in. As a consequence, naming plays a crucial role.

![Publisher/Subscriber](../misc/distributed_systems/c2/fig2_12.png)

Clearly, in publish-subscribe systems such as these, the crucial issue is the efficient and scalable implementation of matching subscriptions to notifications.


## 2.2 Middleware organization
Actual organization of middleware, that is, independent of the overall organization of a distributed system or application.
There are two important types of **design patterns** that are often applied to the organization of middleware: **wrappers** and **interceptors**.
Each targets different problems, yet addresses the same goal for middleware: **achieving openness**.

### Wrappers
Enterprise application integration could be established through middleware as a communication facilitator, but there we still implicitly assumed that, in the end, components could be accessed through their native interfaces.</br>
A wrapper or adapter is a special component that offers an interface acceptable to a client application, of which the functions are transformed into those available at the component. In essence, it solves the problem of **incompatible interfaces**.</br>

Again, facilitating a reduction of the number of wrappers is typically done through middleware. One way of doing this is implementing a so called
**broker**, which is logically a centralized component that handles all the accesses between different applications. An often-used type is a **message broker**
In the case of a message broker, applications simply send requests to the broker containing information on what they need. The broker, having knowledge of all relevant
applications, contacts the appropriate applications, possibly combines and transforms the responses and returns the result to the initial application.

![Wrapper vs Broker](../misc/distributed_systems/c2/fig2_13.png)

### Interceptors
Conceptually, an **interceptor** is nothing but a software construct that will break the usual flow of control and allow other (application specific) code to be executed.
Interceptors are a primary means for adapting middleware to the specific needs of an application.</br>
To make matters concrete, consider interception as supported in many object-based distributed systems.

![Interceptor to handle remote invocations](../misc/distributed_systems/c2/fig2_14.png)

### Modifiable middleware
What wrappers and interceptors offer are means to extend and adapt the middleware. The need for adaptation comes from the fact that the environment
in which distributed applications are executed changes continuously. Changes include those resulting from mobility, a strong variance in the quality-ofservice
of networks, failing hardware, and battery drainage, amongst others. Rather than making applications responsible for reacting to changes, this task
is placed in the middleware. Moreover, as the size of a distributed system increases, changing its parts can rarely be done by temporarily shutting it
down. What is needed is being able to make changes on-the-fly.

## 2.3 System architecture
How many distributed systems are actually organized by considering where software components are placed.</br>
Deciding on software components, their interaction, and their placement leads to an instance of a software architecture, also known as a **system architecture**.</br>

### Centralized organizations
Thinking in terms of **clients** that request services from servers helps understanding and managing the complexity of distributed systems.

#### Simple client-server architecture
This client-server interaction, also known as request-reply behavior is shown in Figure 2.15 in the form of a message sequence chart.

![Request Replay](../misc/distributed_systems/c2/fig2_15.png)

Communication between a client and a server can be implemented by means of a simple connectionless protocol when the underlying network is
fairly reliable as in many local-area networks.</br>
Using a **connectionless** protocol has the obvious advantage of being efficient.As long as messages do not get lost or corrupted, the request/reply protocol just sketched works fine.</br>
When an operation can be repeated multiple times without harm, it is said to be idempotent.</br>
As an alternative, many client-server systems use a reliable **connection oriented** protocol. Although this solution is not entirely appropriate in a local-area network due to relatively low performance, it works perfectly fine
in wide-area systems in which communication is inherently unreliable.(reliable TCP/IP connections).In this case, whenever a client requests a service, it firstsets up a connection to the server before sending the request. The server generally uses that same connection to send the reply message, after which
the connection is torn down. The trouble may be that setting up and tearing down a connection is relatively costly, especially when the request and reply messages are small. </br>

#### Multitiered architectures
The distinction into three logical levels as discussed so far, suggests a number of possibilities for physically distributing a client-server application across several machines.
We make a distinction between only two kinds of machines: **client machines** and **server machines**, leading to what is also referred to as a **(physically) two-tiered architecture**.

![Tow Tiered architecture](../misc/distributed_systems/c2/fig2_16.png)

When distinguishing only client and server machines as we did so far, we miss the point that a server may sometimes need to act as a client, as shown
in Figure 2.17, leading to a **(physically) three-tiered architecture**.</br>
These organizations are used where the client machine is a PC or workstation, connected through a network to a distributed file system or database. Essentially, most of the
application is running on the client machine, but all operations on files or database entries go to the server. For example, many banking applications
run on an end-user’s machine where the user prepares transactions and such.

![Sever acting as a client](../misc/distributed_systems/c2/fig2_17.png)

In this architecture, traditionally programs that form part of the processing layer are executed by a separate server, but may additionally be partly
distributed across the client and server machines. A typical example of where a three-tiered architecture is used is in transaction processing. A separate
process, called the **transaction processing monitor**, coordinates all transactions across possibly different data servers.</br>
Another, but very different example were we often see a three-tiered architecture is in the organization of Web sites. In this case, a Web server acts
as an entry point to a site, passing requests to an application server where the actual processing takes place. This application server, in turn, interacts with a
database server. For example, an application server may be responsible for running the code to inspect the available inventory of some goods as offered
by an electronic bookstore. To do so, it may need to interact with a database containing the raw inventory data.

### Decentralized organizations: peer-to-peer systems
In many business environments, distributed processing is equivalent to organizing a client-server application
as a multitiered architecture. We refer to this type of distribution as **vertical distribution**. The characteristic feature of vertical distribution is that it is
achieved by placing logically different components on different machines.</br>
In modern architectures, it is often the distribution of the clients and the servers that counts, which we refer to as **horizontal distribution**. In this type of distribution, a client or server may be physically split up into
logically equivalent parts, but each part is operating on its own share of the complete data set, thus balancing the load. In this section we will take a look at a class of modern system architectures that support horizontal distribution,
known as **peer-to-peer systems**.</br>
From a high-level perspective, the processes that constitute a peer-to-peer system are all equal. This means that the functions that need to be carried out are represented by every process that constitutes the distributed system.
As a consequence, much of the interaction between processes is symmetric: each process will act as a client and a server at the same time (which is also referred to as acting as a **servant**).</br>
Given this symmetric behavior, peer-to-peer architectures evolve around the question how to organize the processes in an **overlay network** a network in which the nodes are formed by the processes and the links
represent the possible communication channels.A node may not be able to communicate directly with an arbitrary other node, but is required to send messages through the available communication channels. Two types of overlay networks exist: those that are structured and those that are not.</br>

#### Structured peer-to-peer systems
As its name suggests, in a structured peer-to-peer system the nodes (i.e., processes) are organized in an overlay that adheres to a specific, deterministic
topology: a ring, a binary tree, a grid, etc. This topology is used to efficiently look up data. Characteristic for structured peer-to-peer systems, is that they
are generally based on using a so-called semantic-free index. What this means is that each data item that is to be maintained by the system, is uniquely
associated with a key, and that this key is subsequently used as an index. To this end, it is common to use a hash function, so that we get:</br>
_key(data item) = hash(data item’s value).</br>_
The peer-to-peer system as a whole is now responsible for storing (key,value) pairs. To this end, each node is assigned an identifier from the same set
of all possible hash values, and each node is made responsible for storing data associated with a specific subset of keys. In essence, the system is
thus seen to implement a **distributed hash table**, generally abbreviated to a **DHT**</br>
Following this approach now reduces the essence of structured peer-to-peer systems to being able to look up a data item by means of its key. That
is, the system provides an efficient implementation of a function **lookup** that maps a key to an **existing** node:</br>
_existing node:existing node = lookup(key)</br>_
This is where the topology of a structured peer-to-peer system plays a crucial role. Any node can be asked to look up a given key, which then boils down to
efficiently routing that lookup request to the node responsible for storing the data associated with the given key.

#### Unstructured peer-to-peer systems
Structured peer-to-peer systems attempt to maintain a specific, deterministic overlay network. In contrast, in an unstructured peer-to-peer system each
node maintains an ad hoc list of neighbors. The resulting overlay resembles what is known as a **random graph**.</br>
In an unstructured peer-to-peer system, when a node joins it often contacts a well-known node to obtain a starting list of other peers in the system. This
list can then be used to find more peers, and perhaps ignore others, and so on. In practice, a node generally changes its local list almost continuously.</br>
Unlike structured peer-to-peer systems, looking up data cannot follow a predetermined route when lists of neighbors are constructed in an ad hoc
fashion. Instead, in an unstructured peer-to-peer systems we really need to resort to _searching_ for data. Let us look at two
extremes and consider the case in which we are requested to search for specific data.
* Flooding: an issuing node u simply passes a request for a data item to all its neighbors.
* Random walks: an issuing node u can simply try to find a data item by asking a randomly chosen neighbor

#### Hierarchically organized peer-to-peer networks
Notably in unstructured peer-to-peer systems, locating relevant data items can become problematic as the network grows. The reason for this scalability
problem is simple: as there is no deterministic way of routing a lookup request to a specific data item, essentially the only technique a node can resort to is
searching for the request by means of flooding or randomly walking through the network. As an alternative many peer-to-peer systems have proposed to make use of special nodes that maintain an index of data items. </br>
There are other situations in which abandoning the symmetric nature of peer-to-peer systems is sensible. Consider a collaboration of nodes that
offer resources to each other. For example, in a collaborative content delivery network (CDN), nodes may offer storage for hosting copies ofWeb documents
allowing Web clients to access pages nearby, and thus to access them quickly. What is needed is a means to find out where documents can be stored best.
In that case, making use of a **broker** that collects data on resource usage and availability for a number of nodes that are in each other’s proximity will allow
to quickly select a node with sufficient resources.</br>
Nodes such as those maintaining an index or acting as a broker are generally referred to as **super peers**. As the name suggests, super peers are often
also organized in a peer-to-peer network, leading to a hierarchical organization.(fig 2.20) In this organization, every regular
peer, now referred to as a **weak peer**, is connected as a client to a super peer.
All communication from and to a weak peer proceeds through that peer’s associated super peer.</br>
In many cases, the association between a weak peer and its super peer is fixed: whenever a weak peer joins the network, it attaches to one of the
super peers and remains attached until it leaves the network. Obviously, it is expected that super peers are long-lived processes with high availability.

![Super-peer network](../misc/distributed_systems/c2/fig2_20.png)

As we have seen, peer-to-peer networks offer a flexible means for nodes to join and leave the network. However, with super-peer networks a new
problem is introduced, namely how to select the nodes that are eligible to become super peer.</br>
Initial Skype arch.

### Hybrid Architectures
Distributed systems in which client-server solutions are combined with decentralized architectures.

#### Edge-server systems
These systems are deployed on the Internet where servers are placed “at the edge” of the network.
This edge is formed by the boundary between enterprise networks and the actual Internet, for example, as provided by an **Internet Service Provider (ISP)**.
Likewise, where end users at home connect to the Internet through their ISP, the ISP can be considered as residing at the edge of the Internet.

![Edge services](../misc/distributed_systems/c2/fig2_21.png)

This concept of edge-server systems is now often taken a step further: taking cloud computing as implemented in a data center as the core, additional
servers at the edge of the network are used to assist in computations and storage, essentially leading to distributed cloud systems.

#### Collaborative distributed systems
Hybrid structures are notably deployed in collaborative distributed systems. The main issue in many of these systems is to first get started, for which often
a traditional client-server scheme is deployed. Once a node has joined the system, it can use a fully decentralized scheme for collaboration.

To make matters concrete, let us consider the widely popular **BitTorrent file-sharing system**.BitTorrent is a peer-to-peer file downloading system
The basic idea is that when an end user is looking for a file, he downloads chunks of the file from other users until the downloaded chunks can be assembled together yielding the complete file. An important design goal was to ensure collaboration.
In most file-sharing systems, a significant fraction of participants merely download files but otherwise contribute close to nothing a phenomenon referred to as **free
riding**. To prevent this situation, in BitTorrent a file can be downloaded only when the downloading client is providing content to someone else.

![BitTorrent](../misc/distributed_systems/c2/fig2_22.png)

To download a file, a user needs to access a global directory, which is generally just one of a few well-known Web sites. Such a directory contains
references to what are called torrent files. A **torrent file** contains the information that is needed to download a specific file. In particular, it contains a link
to what is known as a **tracker**, which is a server that is keeping an accurate account of active nodes that have (chunks of) the requested file.


