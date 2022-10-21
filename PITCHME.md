---
marp: false
title: Project Reactor and WebFlux
description: Hosting Marp slide deck on the web
theme: default
paginate: true
_paginate: false

---
![bg](./assets/bg-main.png)

# [DRAFT] Project Reactor and WebFlux


### Introduction to Reactive Programming and WebFlux perfomance showcase

Vadim Axelrod
Senior Software Engineer at EPAM

<style scoped>a { color: #eee; }</style>

<!-- 
Hi, everyone.
My name is Vadim Axelrod, I'm a Java developer at EPAM, and today I'm going to show you an introduction to Reactive Programming and present Spring WebFlux - the reactive-stack web framework, as well as a comparison of Spring WebFlux perfomance over a Spring MVC.
-->

---
![bg](./assets/bg-secondary.png)

# Agenda

- Spring WebFlux
- Introduction to Reactive Programming
- Reactive Streams and Project Reactor
- JVM Threads and Linux Processes
- WebFlux and MVC - Thread models
- WebFlux and MVC - Perfomance test

<!--
We will start with quick introduction of Spring WebFlux, followed by an overview of Reactive Programming as a paradigm and Project Reactor as its implementation in Java, then we'll scratch a surface of JVM Threads in Linux, take a good look at WebFlux and MVC thread models and finish with a demo application using both of them to proove today's point. 
-->

---
![bg](./assets/bg-secondary.png)

# Today's point

## Spring WebFlux is better than Spring MVC

<!--
What point?
My point for today is pretty simple. Spring WebFlux is better than Spring MVC.
-->

---
![bg](./assets/bg-secondary.png)

# Why?

```
- Vim лучше, чем Emacs.
- Чем?
- Чем Emacs.
```
Russian dad joke (untranslatable, sorry)

<!--
Why, you may ask?
Well, behing me there is untranslatable russian dad joke.
Vim is better, than Emacs.
Why? Because it is... Better. That's why.

But, aside from jokes, WebFlux is better because of perfomance.
-->

---
![bg](./assets/bg-secondary.png)

# Spring WebFlux

WebFlux is a Spring reactive-stack web framework.

It is fully non-blocking, supports Reactive Streams back pressure, and runs on such servers as Netty, Undertow, and Servlet 3.1+ containers.

<!--
Okay, what is Spring WebFlux.

I believe, everyone, who uses Java, have used Spring MVC at least once.

The original web framework included in the Spring Framework, Spring Web MVC, was purpose-built for the Servlet API and Servlet containers. The reactive-stack web framework, Spring WebFlux, was added later in version 5.0. It is fully non-blocking, supports Reactive Streams back pressure, and runs on such servers as Netty, Undertow, and Servlet 3.1+ containers.
-->

---
![bg](./assets/bg-secondary.png)

# Reactive Programming

## Definition

Reactive programming is a declarative programming paradigm concerned with data streams and the propagation of change.

<!--
Just to recap, WebFlux is based on Reactive Programming paradigm.
Let's start with the definition of Reactive Programming.
-->

---
![bg](./assets/bg-secondary.png)

# Resources talks

Non-blocking > Blocking

<!--
Why do we need this after all?

Because resources talks.
Your application should not consume more, than you actually need, and Spring MVC paradighm can easily led you to do so.
Resource consumption is the main advantage of Reactive Programming in general, and the main advantage of WebFlux over MVC in particular.

Just to be clear. Reactive Programming does not "lighter", it does not use less resources that classic paradigm in total. Reactive Programming provides you ability to use the already allocated resources more efficient.
Basically it means, that you haven't got a threads in your application which does nothing.
-->

---
![bg](./assets/bg-secondary.png)

# Blocking Can Be Wasteful

Two ways to improve a program’s performance:
- parallelize to use more threads and more hardware resources.
- seek more efficiency in how current resources are used.

Reactive Programming aims to the second option by reducing wasting of resources.

<!--
Modern applications can reach huge numbers of concurrent users, and, even though the capabilities of modern hardware have continued to improve, performance of modern software is still a key concern.
There are, broadly, two ways one can improve a program’s performance:
parallelize to use more threads and more hardware resources.
seek more efficiency in how current resources are used.
Usually, Java developers write programs by using blocking code. This practice is fine until there is a performance bottleneck. Then it is time to introduce additional threads, running similar blocking code. But this scaling in resource utilization can quickly introduce contention and concurrency problems.
Worse still, blocking wastes resources. If you look closely, as soon as a program involves some latency (notably I/O, such as a database request or a network call), resources are wasted because threads (possibly many threads) now sit idle, waiting for data.
So the parallelization approach is not a silver bullet. It is necessary to access the full power of the hardware, but it is also complex to reason about and susceptible to resource wasting.
-->

---
![bg](./assets/bg-secondary.png)

# Reactive Programming

- [The Reactive Manifesto](https://www.reactivemanifesto.org)
- [The Reactive Principles](https://www.reactiveprinciples.org)

Reactive Systems are:
- Responsive: The system responds in a timely manner if at all possible.
- Resilient: The system stays responsive in the face of failure.
- Elastic: The system stays responsive under varying workload.
- Message Driven: Reactive Systems rely on asynchronous message-passing to establish a boundary between components that ensures loose coupling, isolation and location transparency.

<!--
As well as Agile Manifesto, for example, there is a Reactive Manifesto which stands for quality attributes, that proper reactive system should have.

So, Reactive System should be responsive, resilient, elastic and message-driven.
You can check out this manifesto as well as The Reactive Principles on the websites above.
-->

---
![bg](./assets/bg-secondary.png)

# Reactive Streams and Project Reactor

1. [Reactive Streams](http://www.reactive-streams.org) is an initiative to provide a standard for asynchronous stream processing with non-blocking back pressure.
2. The interfaces, which are 1:1 semantically equivalent to Reactive Streams, are available in [JDK9 java.util.concurrent.Flow](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/Flow.html).
3. [Project Reactor](https://projectreactor.io/docs/core/release/reference/) is an implementation of the Reactive Programming paradigm.

<!--
Reactive Programming is presented in Java by Reactive Streams as interfaces and Project Reactor as implementation.
-->

---
![bg](./assets/bg-secondary.png)

# java.util.concurrent.Flow

- **Publisher**
  - A producer of items (and related control messages) received by **Subscribers**.
- **Subscriber**
  - A receiver of messages.  The methods in this interface are invoked in strict sequential order for each **Subscription**.
- **Subscription**
  - Message control linking a **Publisher** and **Subscriber**.
- **Processor**
  - A component that acts as both a **Subscriber** and **Publisher**.

<!--
Let's take a closer look on them.
Just four of them.
-->

---
![bg](./assets/bg-secondary.png)

# Project Reactor
## Producers

**Mono** and **Flux** implements Publisher interface.

### Mono
```
Mono<String> getMessage() {
  return Mono.just("The only message from Mono");
}
```

### Flux
```
Flux<String> getMessages() {
  return Flux.just("First message from Flux", "Second message from Flux");
}
```

<!--
How to use them in your application?
Just like this. 
If your method should return a single response - use Mono, otherwise - use Flux.
-->

---
![bg](./assets/bg-secondary.png)

# Project Reactor
## Running Mono and Flux

```
System.out.println(getMessage().block());
getMessages().subscribe(System.out::println);
```

### out
```
The only message from Mono
First message from Flux
Second message from Flux

Process finished with exit code 0
```

<!--
How to use them.
The main idea is, that nothing happens until you call subscribe().
No framework would somehow find and run your Mono's and Flux'es for you.
You should call them by yourself, or, for example, in case of WebFlux, it would be done by web-server.
-->

---
![bg](./assets/bg-secondary.png)

# Hot vs Cold

- A Cold sequence starts anew for each Subscriber, including at the source of data.
- A Hot sequence does not start from scratch for each Subscriber. Rather, late subscribers receive signals emitted after they subscribed.

<!--
The Rx family of reactive libraries distinguishes two broad categories of reactive sequences: hot and cold. This distinction mainly has to do with how the reactive stream reacts to subscribers:
A Cold sequence starts anew for each Subscriber, including at the source of data. For example, if the source wraps an HTTP call, a new HTTP request is made for each subscription.
A Hot sequence does not start from scratch for each Subscriber. Rather, late subscribers receive signals emitted after they subscribed. Note, however, that some hot reactive streams can cache or replay the history of emissions totally or partially. From a general perspective, a hot sequence can even emit when no subscriber is listening (an exception to the “nothing happens before you subscribe” rule).
-->

---
![bg](./assets/bg-secondary.png)

# So, what about threads?

Project Reactor not enforce a concurrency model.

`subscribe()` on Mono or Flux does not mean, that it runs in a dedicated Thread.
In Reactor, the execution model is determined by the `Scheduler`.
`Scheduler` is similar to an `ExecutorService`, and can either have no execution context (run on current thread) or specific reusable Thread or Thread pool.

WebFlux, as a framework, would handle Threads for you.

---
![bg](./assets/bg-secondary.png)


# Java Threads and Linux Processes

Threads are expensive.

Java thread creation is expensive because there is a fair bit of work involved:

- A large block of memory has to be allocated and initialized for the thread stack.
- System calls need to be made to create / register the native thread with the host OS.
- Descriptors need to be created, initialized and added to JVM-internal data structures.
- It is also expensive in the sense that the thread ties down resources as long as it is alive; e.g. the thread stack, any objects reachable from the stack, the JVM thread descriptors, the OS native thread descriptors.

The costs of all of these things are platform specific, but they are not cheap on any Java platform I've ever come across.

https://www.ibm.com/docs/en/sdk-java-technology/7.1?topic=tstt-understanding-java-native-thread-details-1

<!--
Going back to the main topic - WebFlux.
Why is it bad to keep using Spring MVC.
Let's focus on Thread models.
-->

---
![bg](./assets/bg-secondary.png)

# Java Threads
## Stack size

1024 KB

```
vadim@ubuntu:~$ java -XX:+PrintFlagsFinal -version | grep ThreadStack 
     intx ThreadStackSize                          = 1024                                   {pd product} {default}
```

I.e. any Java Thread object allocates 1 MB.

https://www.demo2s.com/java/java-17-data-types-stack-and-heap-memory.html

---
![bg](./assets/bg-secondary.png)

# Java Threads
## Why 1024 KB?

A JVM stack, also called thread stack, is a data area in the JVM memory created for a single execution thread. The JVM stack of a thread is used by the thread to store local variables, partial results, and data for method invocations and returns.

A new frame is created when a new method is invoked.

<!--
Stack Frame size?

https://alvinalexander.com/scala/fp-book/recursion-jvm-stacks-stack-frames/

http://www.herongyang.com/JVM/Stack-Overflow-What-Is-JVM-Stack.html


What Is a Stack Frame? A JVM stack data area is actually organized as a stack of frames. The life cycle of a frame looks like this:

A new frame is created when a new method is invoked.
The new frame is added to the top of the stack with the execution control is transferred to the new method.
The new frame is used during the execution of the new method to store local variables, partial results, and data for subsequent method invocations and returns.
The new frame is removed from the top of the stack when the execution control is returned from the new method.

What Is a StackOverflowError? A StackOverflowError is an exception thrown by a thread, when it's stack has no more room to add a new frame to make the next method call. You may use the JVM -Xss option to increase JVM stack size to avoid StackOverflowError exceptions.

2.5.2. Java Virtual Machine Stacks

Each Java Virtual Machine thread has a private Java Virtual Machine stack, created at the same time as the thread. A Java Virtual Machine stack stores frames (§2.6). A Java Virtual Machine stack is analogous to the stack of a conventional language such as C: it holds local variables and partial results, and plays a part in method invocation and return. Because the Java Virtual Machine stack is never manipulated directly except to push and pop frames, frames may be heap allocated. The memory for a Java Virtual Machine stack does not need to be contiguous.

In The Java Virtual Machine Specification, First Edition, the Java Virtual Machine stack was known as the Java stack.

This specification permits Java Virtual Machine stacks either to be of a fixed size or to dynamically expand and contract as required by the computation. If the Java Virtual Machine stacks are of a fixed size, the size of each Java Virtual Machine stack may be chosen independently when that stack is created.

A Java Virtual Machine implementation may provide the programmer or the user control over the initial size of Java Virtual Machine stacks, as well as, in the case of dynamically expanding or contracting Java Virtual Machine stacks, control over the maximum and minimum sizes.

The following exceptional conditions are associated with Java Virtual Machine stacks:

If the computation in a thread requires a larger Java Virtual Machine stack than is permitted, the Java Virtual Machine throws a StackOverflowError.

If Java Virtual Machine stacks can be dynamically expanded, and expansion is attempted but insufficient memory can be made available to effect the expansion, or if insufficient memory can be made available to create the initial Java Virtual Machine stack for a new thread, the Java Virtual Machine throws an OutOfMemoryError.


https://docs.oracle.com/javase/specs/jvms/se7/html/jvms-2.html#jvms-2.5.2
https://docs.oracle.com/javase/specs/jvms/se7/html/jvms-2.html#jvms-2.6


2.6. Frames

A frame is used to store data and partial results, as well as to perform dynamic linking, return values for methods, and dispatch exceptions.

A new frame is created each time a method is invoked. A frame is destroyed when its method invocation completes, whether that completion is normal or abrupt (it throws an uncaught exception). Frames are allocated from the Java Virtual Machine stack (§2.5.2) of the thread creating the frame. Each frame has its own array of local variables (§2.6.1), its own operand stack (§2.6.2), and a reference to the run-time constant pool (§2.5.5) of the class of the current method.

A frame may be extended with additional implementation-specific information, such as debugging information.

The sizes of the local variable array and the operand stack are determined at compile-time and are supplied along with the code for the method associated with the frame (§4.7.3). Thus the size of the frame data structure depends only on the implementation of the Java Virtual Machine, and the memory for these structures can be allocated simultaneously on method invocation.

Only one frame, the frame for the executing method, is active at any point in a given thread of control. This frame is referred to as the current frame, and its method is known as the current method. The class in which the current method is defined is the current class. Operations on local variables and the operand stack are typically with reference to the current frame.

A frame ceases to be current if its method invokes another method or if its method completes. When a method is invoked, a new frame is created and becomes current when control transfers to the new method. On method return, the current frame passes back the result of its method invocation, if any, to the previous frame. The current frame is then discarded as the previous frame becomes the current one.

Note that a frame created by a thread is local to that thread and cannot be referenced by any other thread.

4.7.3. The Code Attribute

The Code attribute is a variable-length attribute in the attributes table of a method_info (§4.6) structure. A Code attribute contains the Java Virtual Machine instructions and auxiliary information for a single method, instance initialization method (§2.9), or class or interface initialization method (§2.9). Every Java Virtual Machine implementation must recognize Code attributes. If the method is either native or abstract, its method_info structure must not have a Code attribute. Otherwise, its method_info structure must have exactly one Code attribute.

The Code attribute has the following format:

Code_attribute {
    u2 attribute_name_index;
    u4 attribute_length;
    u2 max_stack;
    u2 max_locals;
    u4 code_length;
    u1 code[code_length];
    u2 exception_table_length;
    {   u2 start_pc;
        u2 end_pc;
        u2 handler_pc;
        u2 catch_type;
    } exception_table[exception_table_length];
    u2 attributes_count;
    attribute_info attributes[attributes_count];
}
The items of the Code_attribute structure are as follows:

attribute_name_index
The value of the attribute_name_index item must be a valid index into the constant_pool table. The constant_pool entry at that index must be a CONSTANT_Utf8_info (§4.4.7) structure representing the string "Code".

attribute_length
The value of the attribute_length item indicates the length of the attribute, excluding the initial six bytes.

max_stack
The value of the max_stack item gives the maximum depth of the operand stack of this method (§2.6.2) at any point during execution of the method.

max_locals
The value of the max_locals item gives the number of local variables in the local variable array allocated upon invocation of this method (§2.6.1), including the local variables used to pass parameters to the method on its invocation.

The greatest local variable index for a value of type long or double is max_locals - 2. The greatest local variable index for a value of any other type is max_locals - 1.

code_length
The value of the code_length item gives the number of bytes in the code array for this method. The value of code_length must be greater than zero; the code array must not be empty.

code[]
The code array gives the actual bytes of Java Virtual Machine code that implement the method.

When the code array is read into memory on a byte-addressable machine, if the first byte of the array is aligned on a 4-byte boundary, the tableswitch and lookupswitch 32-bit offsets will be 4-byte aligned. (Refer to the descriptions of those instructions for more information on the consequences of code array alignment.)

The detailed constraints on the contents of the code array are extensive and are given in a separate section (§4.9).

exception_table_length
The value of the exception_table_length item gives the number of entries in the exception_table table.

exception_table[]
Each entry in the exception_table array describes one exception handler in the code array. The order of the handlers in the exception_table array is significant (§2.10).

Each exception_table entry contains the following four items:

start_pc, end_pc
The values of the two items start_pc and end_pc indicate the ranges in the code array at which the exception handler is active. The value of start_pc must be a valid index into the code array of the opcode of an instruction. The value of end_pc either must be a valid index into the code array of the opcode of an instruction or must be equal to code_length, the length of the code array. The value of start_pc must be less than the value of end_pc.

The start_pc is inclusive and end_pc is exclusive; that is, the exception handler must be active while the program counter is within the interval [start_pc, end_pc).

The fact that end_pc is exclusive is a historical mistake in the design of the Java Virtual Machine: if the Java Virtual Machine code for a method is exactly 65535 bytes long and ends with an instruction that is 1 byte long, then that instruction cannot be protected by an exception handler. A compiler writer can work around this bug by limiting the maximum size of the generated Java Virtual Machine code for any method, instance initialization method, or static initializer (the size of any code array) to 65534 bytes.

handler_pc
The value of the handler_pc item indicates the start of the exception handler. The value of the item must be a valid index into the code array and must be the index of the opcode of an instruction.

catch_type
If the value of the catch_type item is nonzero, it must be a valid index into the constant_pool table. The constant_pool entry at that index must be a CONSTANT_Class_info structure (§4.4.1) representing a class of exceptions that this exception handler is designated to catch. The exception handler will be called only if the thrown exception is an instance of the given class or one of its subclasses.

If the value of the catch_type item is zero, this exception handler is called for all exceptions. This is used to implement finally (§3.13).

attributes_count
The value of the attributes_count item indicates the number of attributes of the Code attribute.

attributes[]
Each value of the attributes table must be an attribute structure (§4.7). A Code attribute can have any number of optional attributes associated with it.

The only attributes defined by this specification as appearing in the attributes table of a Code attribute are the LineNumberTable (§4.7.12), LocalVariableTable (§4.7.13), LocalVariableTypeTable (§4.7.14), and StackMapTable (§4.7.4) attributes.

If a Java Virtual Machine implementation recognizes class files whose version number is 50.0 or above, it must recognize and correctly read StackMapTable (§4.7.4) attributes found in the attributes table of a Code attribute of a class file whose version number is 50.0 or above.

A Java Virtual Machine implementation is required to silently ignore any or all attributes in the attributes table of a Code attribute that it does not recognize. Attributes not defined in this specification are not allowed to affect the semantics of the class file, but only to provide additional descriptive information (§4.7.1).
-->

---
![bg](./assets/bg-secondary.png)

# Tomcat vs Netty - Thread Models

## Spring MVC -> Tomcat
Uses Servlet Engine. 
`spring-boot-starter-web` uses Tomcat as an embedded container by default.

The Servlet Container automatically creates a new Java thread for every servlet request it receives. 
When the servlet’s done running the HTTP service method for that client’s request, the thread completes (i.e. dies).

<!--
https://docs.spring.io/spring-boot/docs/1.2.3.RELEASE/reference/html/howto-embedded-servlet-containers.html
https://docs.spring.io/spring-framework/docs/3.2.x/spring-framework-reference/html/mvc.html

Tomcat uses Thread-per-request model, which means, that 

Now let’s understand thread per request model, consider a traditional spring web application with spring mvc deployed on servlet container such as Tomcat.
-->

---
![bg](./assets/bg-secondary.png)

# Tomcat vs Netty - Thread Models

## Spring WebFlux -> Netty
Uses Reactive Streams API.
`spring-boot-starter-webflux` uses Netty as an embedded reactive web server.

Netty uses EventLoops. An EventLoop is powered by one single thread. The number of EventLoops to handle the request is equal to no of cores in your machine.
Not requires new Thread.

---
![bg](./assets/bg-secondary.png)

# Tomcat vs Netty - ThreadLocal vs Reactive Context

One of the big technical challenges - Netty do not use ThreadLocal.

Slf4J relies on TheadLocal.
Headers relies on ThreadLocal.

The usual workaround for ThreadLocal usage is to move the contextual data, C, along your business data, T, in the sequence, by using (for instance) Tuple2<T, C>. This does not look good and leaks an orthogonal concern (the contextual data) into your method and Flux signatures.

<!--
One of the big technical challenges encountered when switching from an imperative programming perspective to a reactive programming mindset lies in how you deal with threading.
Contrary to what you might be used to, in reactive programming, you can use a Thread to process several asynchronous sequences that run at roughly the same time (actually, in non-blocking locksteps). The execution can also easily and often jump from one thread to another.
This arrangement is especially hard for developers that use features dependent on the threading model being more “stable,” such as ThreadLocal. As it lets you associate data with a thread, it becomes tricky to use in a reactive context. As a result, libraries that rely on ThreadLocal at least introduce new challenges when used with Reactor. At worst, they work badly or even fail. Using the MDC of Logback to store and log correlation IDs is a prime example of such a situation.
The usual workaround for ThreadLocal usage is to move the contextual data, C, along your business data, T, in the sequence, by using (for instance) Tuple2<T, C>. This does not look good and leaks an orthogonal concern (the contextual data) into your method and Flux signatures.
-->

---
![bg](./assets/bg-secondary.png)

EventLoop
A Netty EventLoop is a loop that keeps looking for new events, e.g. incoming data from network sockets (from SocketChannel) instances). When an event occurs, the event is passed on to the appropriate event handler, for instance a ChannelHandler.

SocketChannel
A Netty SocketChannel represents a TCP connection to another computer over a network. Whether you are using Netty as client or server, all data exchanged with other computers on the network are passed through a SocketChannel instance representing the TCP connection between the computers.

A SocektChannel is managed by an EventLoop, and always only by that same EventLoop. Since an EventLoop is always executed by the same thread, a SocketChannel instance is also only accessed by the same thread. Therefore you don't have to worry about synchronization when reading from a SocketChannel.

http://webcache.googleusercontent.com/search?q=cache:SpCkD2jN4pEJ:tutorials.jenkov.com/netty/overview.html&cd=7&hl=ru&ct=clnk&gl=ge&client=safari

---
![bg](./assets/bg-secondary.png)

# How much threads?

- EventLoops
- ThreadPool

## Initial

Fruit | Colour | Amount | Cost
-----|------|:-----:|--------:
Banana | Yellow | 4 | £1.00
Apple | Red | 2 | £0.60
Orange | Orange | 10 | £2.50
Coconut | Brown | 1 | £1.50


## 100 concurrent users

---
![bg](./assets/bg-secondary.png)

# Thread Dump for MVC
Live threads: 28
Daemon threads: 24

## Tomcat
- Catalina-utility-1
- Catalina-utility-2
- container-0
- http-nio-8081-exec-[1;10]
- http-nio-8081-Poller
- http-nio-8081-Acceptor

Total: 15

<!--
- http-nio-8081-Poller - The background thread that adds sockets to the Poller, checks the poller for triggered events and hands the associated socket off to an appropriate processor as events occur.
- http-nio-8081-Acceptor - The background thread that listens for incoming TCP/IP connections and hands them off to an appropriate processor.
-->

---
![bg](./assets/bg-secondary.png)

# Thread Dump for WebFlux
Live threads: 16
Daemon threads: 14

## Netty
- boundedElastic-evictor-1
- reactor-http-nio-1
- server

Total: 3

---
![bg](./assets/bg-secondary.png)

# Load test

### Environment
- CPU: Apple M1 Pro
- Memory: 32 Gb
- OS: macOS Monterey 12.5.1
- Java: openjdk 18.0.2 2022-07-19

### Profile
- Number of threads (users): 500
- Ramp-up period (seconds): 100
- Loop count: 200000
- Total requests: 100000000

---
# Load test
## MVC
- Total requests: 100 000 000 
- Total time: 00:59:18
- Throughput: 28096 req/sec

![bg](./assets/mvc-1kk-30sec.png)

---

# Load test
## WebFlux 
- Total requests: 100 000 000
- Total time: 00:22:21
- Throughput: 74585 req/sec (265%)

![bg](./assets/webflux-1kk-30sec.png)
