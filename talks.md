## 2020

### QCon London
#### How the HotSpot and Graal JVMs execute Java Code

When Java was released in 1995 it was slow, a reputation it has carried for many years… 
Today Java can give performance that is comparable to C++ and can emit instructions that are more optimal than code which is statically compiled. But how?  

This talk will explore practical examples and the subsystems that are involved in interpreting, compiling and executing a simple Hello World Application. 
We will dive into JIT compilation and the arrival of the JVM Compiler Interface (JVMCI) to explore how optimisations are applied to boost the performance of our application. 
We will discuss HotSpot, explore Graal and the JVM ecosystem to discover performance benefits of a platform 25 years in the making.

The slides for the talk can be found [here](/assets/slide-decks/graal-compiler-java-printable.pdf).
There is also [video](https://youtu.be/oIcU6Emxj_s) given prior to the talk at the NY Java SIG when the talk was still in rough cut.

### O'Reilly Software Architecture Conference - New York
#### Building Specifying and Testing APIs with Microservices 

* Create microservices using Spring Boot, which exposes an API (and prove that’s not the hard part, with options offered for non-Java developers)
* Use either Pact or Spring Cloud Contract to test and progress the API with new features
* Discover how to use OpenAPI to specify an API and the balance between developing APIs with specification first or contract driven
* Use a specification to allow your API to be versioned and use semantic versioning to ensure that components in your architecture remain decoupled and free to move without breaking consumers
* Explore the reasons for an API gateway, why they’re architecturally important, and the concepts of a service mesh in the context of developing an API program

The workshop is Open Source and can be found on [GitHub](https://github.com/jpgough/api-workshop).

#### API Gateways: The good, the bad and the ugly

Microservices are everywhere, and when implemented properly can give significant architectural advantages. 
However, there’s also the counter to this where concerns aren’t thought through, and the bad or even ugly side of microservices can emerge. 
API gateways are a key architectural pattern that solve a whole range of problems, including routing and even security. 
They can be used off the shelf without considering the various factors and features—until you find yourself in a gun-slinging outage.

James Gough and Matthew Auburn outline why you should use an API gateway, the patterns you can use, and the problems it will solve for you. 
You’ll explore technical implementation details and concerns addressed in code for building API gateways and get a glimpse of the ugly side of getting things wrong. 
Along the way, you’ll discover why testing with gateways and microservices is key and how test containers and chaos engineering helps ensure your ecosystem works as planned and scales effectively. 
Join in to escape the microservices Wild West.

A [video](https://learning.oreilly.com/videos/oreilly-software-architecture/0636920333777/0636920333777-video329463) is available via the O'Reilly platform. 

## 2019

### O'Reilly Software Architecture Conference - Berlin
#### Building Specifying and Testing APIs with Microservices 

For more information see 2020.

The workshop is Open Source and can be found on [GitHub](https://github.com/jpgough/api-workshop).

### QCon Shanghai
#### API Gateways: The good, the bad and the ugly

For more information on the session see 2020.

There is a [video](https://www.youtube.com/watch?v=SuXfTm2xVPA) of QCon Shanghai available.

## 2018

### O'Reilly Software Architecture Conference - New York
#### Building APIs with microservices: Things I wish I’d known

Moving from a traditional monolithic architecture to microservices can be a serious challenge even before adding an API program on top of this, along with a culture shifting toward a more Agile way of working.

Jim Gough walks you through an introduction to understanding the rapidly changing world of APIs with microservices, including key technologies and patterns, approaches to API management, and instigating a culture change. 
The talk will be a balance between short snappy live code examples and slides for discussion points. 
There’s a lot to explore, so expect ideas to come thick and fast tying together to build an overview of this exciting technology space.

Topics include:

* Building a microservice using Docker and Spring Boot
* The sidecar pattern: A balance between libraries and services
* Why use an API gateway?
* Scheduling and running containers
* API gateways and Kubernetes
* Moving to a service mesh
* Managing API versioning
* Testing and validating API compatibility
* Developing an API in an Agile way across multiple teams
* Continuous deployment

The slides for this talk can be found [here](assets/slide-decks/api-microservices.pdf).
Videos of the demonstrations will be available soon.

### Devoxx UK
#### Optimizing Java: A brief tour of the JVM

At Devoxx UK I gave a presentation on Optimizing Java and a brief tour of how the JVM works. 
This was similar to the talk given at the LJC in 2017.
The slides can be found [here](/assets/slide-decks/optimizing-java.pdf) and an accompanying blog post can be found [here](/blog/2018/05/11/optimizing-java).


### New York Java SIG
#### Evolution not Revolution - the Java 9 view of memory & how we got here

Great artists steal. Great programming languages steal from each other. 
In this talk, Ben and Jim will explain the state of the art of the Java Memory Model - taking in such topics as Unsafe, method and var handles and how C++ and Java keep each other on their toes.


### Docklands LJC
#### Sidecar Lightning Talk

At the Docklands LJC I gave a very brief introduction to what a Sidecar is and the advantages and disadvantages.
The slides can be found [here](/assets/slide-decks/sidecar.pdf)

## 2017

### London Java Community
#### How the JVM Executes Java

When Java was released in 1995 it was slow, a reputation it has carried for many years... 
Today Java can give performance that is comparable to C++ and can emit instructions that are more optimal than code which is statically compiled. 
But how? This talk will take a tour of code and the journey through the JVM and the optimisations in between. 
Using practical examples, JVM flags and the Open Source JIT Watch we will explore what the JVM does in an adaptation of the classic Hello World program, 
you'll never look at Java in the same way again.

The video of there session is available [here](https://skillsmatter.com/skillscasts/10565-how-the-jvm-executes-java#video)

## 2016

### Docklands LJC
#### HotSpot Under the Hood and Microbenchmarking in Java

This talk will firstly attempt to persuade you that Microbenchmarking is not a good idea and in fact can lead to inaccurate 
assumptions and poor systems as a result. 
Despite this warning there will be times when it is necessary, particularly for API designers and writers.

The talk will introduce JMH and talk about how to use it and some of what is happening behind the scenes to make your 
benchmark as accurate as possible.

It is impossible to gain a good microbenchmarking result, so we will discuss some of the outputs from JMH and how 
they should be reasoned to attempt to avoid a poor assumption.

The video of this session is available [here](https://www.youtube.com/watch?v=Sdara8XLEfc)

## 2014 

In 2014 I recorded a short video for Night Hackers that was filmed at Oracle. 
The background noise was a little annoying as they decided to use that time to clean away the plates... 

<div align="center">
  <a href="https://www.youtube.com/watch?v=OIg9lNpMJew"><img src="https://img.youtube.com/vi/OIg9lNpMJew/0.jpg"></a>
</div>