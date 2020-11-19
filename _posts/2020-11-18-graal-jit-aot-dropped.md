---
layout: post
title: "Support for GraalVM AOT and JIT in OpenJDK to be Dropped"
date: 2020-11-18
---

At the beginning of 2020 I gave at [QCon London](https://qconlondon.com) titled [How the HotSpot and Graal JVMs execute Java Code](https://www.infoq.com/presentations/hotspot-graalvm-code-execution).
You can find some of the notes on the examples in the talk published [here](https://jpgough.github.io/blog/2020/03/29/hacking-graal-compiler).
One thing I have seen from talking to Java User Group and performance groups is that the concept of Java compiling Java at runtime is really quite appealing.
Some of the live debugging that was demonstrated in the sessions and deep dives have helped Java developers gain accessibility into what happens under the hood in Java.
One other key experimental tool is `jaotc` the Java Ahead of Time Compiler, if this is mixed in the right way with the GraalVM JIT compiler there is a real opportunity to balance AOT with JIT.
One limitation with `jaotc` is that build tool support is essential to the concept taking off, and this has not happened yet. 
Build tools are important as the build time and runtime configuration have to match to be able to use the AOT code.

During [QCon+](https://plus.qconferences.com) I asked [@mon_beck](https://twitter.com/mon_beck) a question on the Windows ARM port to see if they planned to support support GraalVM/JVMCI in the port, as this was something I'd been pondering since a previous presentation at JDConf.
It was at this time I realised that I'd inadvertently asked an awkward question, as the response was this is no longer supported. 
I took a dive into the Open JDK mailing list and found the following post:

> We shipped Ahead-of-Time compilation (the jaotc tool) in JDK 9, as an experimental feature. 
> We shipped Graal as an experimental JIT compiler in JDK 10. 
> We haven't seen much use of these features, and the effort required to support and enhance them is significant.
>
> We therefore intend to disable these features in Oracle builds as of JDK 16.
>
> We'll leave the sources for these features in the repository, in case any one else is interested in building them. But we will not update or test them.
> We'll continue to build and ship JVMCI as an experimental feature in Oracle builds.

_[Link to Mailing List](https://mail.openjdk.java.net/pipermail/discuss/2020-November/005632.html)_

Following the mail thread it appears that the focus will remain on C2 improvements to _"keep Java vibrant and competitive"_. 
This is counter to other quotes I have read that highlights the opposite opinion and that a change might be exactly what is needed to keep Java competitive. 

> One of the key people behind the C2 compiler, Cliff Click, has said that he would never write a
> VM in C++ again, and we’ve just heard that Twitter’s JVM team have said that they think that C2
> have become stagnant and is past needing replacement because development is so difficult.

_[Link to Chris Seaton's Blog](https://chrisseaton.com/truffleruby/jokerconf17/)_

There's also the point about having mixed OOPS and mixing non OO concepts with OO models along with the vintage malloc approaches in the core of C2. 

So with GraalVM as a JIT on hold for now for now, what is happening with AOT?
It appears there is a new project called [Project Leyden](https://mail.openjdk.java.net/pipermail/discuss/2020-April/005429.html) that will help with this, however the project is really yet to get started.
For Java-on-Java there is also [Project Metropolis](https://openjdk.java.net/projects/metropolis/), perhaps we will see more here in the future. 

I do think it's a missed opportunity for innovation and making subsystems in the JVM more accessible to future developers. 
Any enhancements and improvements to C2 are of course welcome, and this is still bedrock of the OpenJDK.
My main motivation for this post is to make this decision more visible to developers who have enjoyed watching my talk on the potential future of the JVM. 
I would also be really interested to see if there is community motivation to keep this going in other builds of OpenJDK. 

There is also a discussion thread for this on [reddit](https://www.reddit.com/r/java/comments/jlkkz3/disable_aot_and_graal_in_oracle_openjdk/gas85j7/?utm_source=reddit&utm_medium=web2x&context=3).