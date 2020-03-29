---
layout: post
title: "Hacking with the Graal Compiler for Java"
date: 2020-03-29
---

Earlier this month I gave a talk at [QCon London](https://qconlondon.com) titled [How the HotSpot and Graal JVMs execute Java Code](https://qconlondon.com/london2020/presentation/hotspot-graal-jvms-execute-java-code).
I have re-branded this talk to have the slightly more descriptive title for the LJC "How the JVM Executes Java: Evolving Compilation with Graal".
There is a video of this talk from the [NY Java Sig](https://youtu.be/oIcU6Emxj_s). 
You can also find the [slides here](/assets/slide-decks/graal-compiler-java-printable.pdf).

This blog posts explores how to compile the Graal JIT compiler yourself and use it with a JVM installed on your machine. 
This is based on an excellent post [Understanding How Graal Works - a Java JIT Compiler Written in Java](https://chrisseaton.com/truffleruby/jokerconf17/) written by [Chris Seaton](https://chrisseaton.com) on the subject in 2017.
Some of the tooling has moved on, which makes it easier to play with Graal and Graal as a JIT compiler. 

### Graal Source Code

The first task is to obtain the source code for Graal, which also contains the Open Source code for the Graal Compiler.

```shell
mkdir graal-dev
cd graal-dev
git clone https://github.com/oracle/graal.git
```

### MX Tool

> [mx](https://github.com/graalvm/mx) is a command line based tool for managing the development of (primarily) Java code. 
> It includes a mechanism for specifying the dependencies as well as making it simple to build, test, run, update, etc the code and built artifacts. 

We will use the `mx` tool to build and patch our build of the compiler into a running version of Java.
We can use the `graal-dev` folder to keep our tools together, you can add `mx` into your shell profile. 

```shell
git clone https://github.com/graalvm/mx.git
export PATH=$PWD/mx:$PATH 
```

### Building the Compiler

Now we have the source code and the `mx` tool we will check that we can build the Graal Compiler.

```shell
cd graal/compiler/
mx build
```

You might get an error about not being able to find a JDK e.g. **Could not find a JDK**.

You can fix this by specifying the location of a Java installation directory, either whilst running the `mx` command or by following the error to configure the JDK for the suite.
A suite is an `mx` term for a series of project components, which is configured at a directory level.

On my machine I simply run:

`mx --java-home="/Library/Java/JavaVirtualMachines/adoptopenjdk-11.jdk/Contents/Home" build`

### Using Our Version of the Compiler

Running `mx vm` within the folder we have just compiled in will patch the JIT compiler into our version of Java. 
For example:

```shell
$mx --java-home="/Library/Java/JavaVirtualMachines/adoptopenjdk-11.jdk/Contents/Home" vm -version 
Updating/creating /Users/jpgough/graal-dev/graal/compiler/mxbuild/darwin-amd64/graaljdks/jdk11-cmp from /Library/Java/JavaVirtualMachines/adoptopenjdk-11.jdk/Contents/Home using intermediate directory /Users/jpgough/graal-dev/graal/compiler/mxbuild/darwin-amd64/graaljdks/tmpcrv05W/jdk11-cmp since /Users/jpgough/graal-dev/graal/compiler/mxbuild/darwin-amd64/graaljdks/jdk11-cmp does not exist
openjdk version "11.0.6" 2020-01-14
OpenJDK Runtime Environment AdoptOpenJDK (build 11.0.6+10)
OpenJDK 64-Bit Server VM AdoptOpenJDK (build 11.0.6+10, mixed mode, sharing)
```

So far we can't see any evidence that we are using the compiler that we have just built, we will now try and make an edit and apply it to running a Java application.

### Editing the Compiler

The `mx` tool also supports setup for the well known IDEs by running `mx ideinit`:

`mx --java-home="/Library/Java/JavaVirtualMachines/adoptopenjdk-11.jdk/Contents/Home" ideinit`

We can now open the project in Intellij at `graal-dev/graal/compiler` and it will pick up the settings of the project structure.
We can make a simple edit in the HotSpotGraalCompiler `compileMethod(CompilationRequest request)` function.

<p align="center">
  <img class="blog-image-terminal" src="/assets/images/blog/graal-hacking/hotspot-graal-compiler.png">
</p>

```java
 @Override
    public CompilationRequestResult compileMethod(CompilationRequest request) {
        System.out.println("Jim's Compiler Version Compiling: " + request.getMethod());
        return compileMethod(request, true, graalRuntime.getOptions());
    }
```

We can now recompile using `mx build` and `mx vm -version` to see it apply to our JVM.

### Triggering JIT Compilation

We need a simple class to run our example and observe JIT compilation, let's adopt a modified `HelloWorld`.
It might be easiest to place this in `graal/compiler` for now, and compile it using `javac`.

```java
public class HelloWorld {
       public static void main(String[] args) {
                for(int i=0; i < 1_000_000; i++) {
                        printInt(i);
                }
        }

        public static void printInt(int number) {
                System.err.println("Hello World" + number);
        }
}
```

Running the following command we should now see output from our compiler. 

```
mx --java-home /Library/Java/JavaVirtualMachines/adoptopenjdk-11.jdk/Contents/Home vm -XX:+UnlockExperimentalVMOptions -XX:+EnableJVMCI -XX:+UseJVMCICompiler -XX:-TieredCompilation HelloWorld 2> /dev/null 
Jim's Compiler Version Compiling: HotSpotMethod<StringLatin1.equals(byte[], byte[])>
Jim's Compiler Version Compiling: HotSpotMethod<StringLatin1.hashCode(byte[])>
```

You will see quite a lot of output from this, this is the result of the Graal Compiler compiling itself. 
This seems weird, but if compilation happens often it makes sense that parts of the compiler written in Java should also be native code.
If you want to filter the compilation to just `HelloWorld` you can also add the flag `-XX:CompileOnly=HelloWorld`.

How is a version of Java installed elsewhere on the machine using the Graal compiler?
The running JVM is using the [JVMCI - JVM Compiler Interface](https://openjdk.java.net/jeps/243) to allow an alternative compiler to be used at runtime. 
`-XX:+UnlockExperimentalVMOptions -XX:+EnableJVMCI -XX:+UseJVMCICompiler` are the flags that set this up.

### Debugging the Compiler

It is also possible to debug the compiler, which can be really useful in understanding the process of JIT compilation.

`mx -d` triggers the debugger to begin listening for the IDE to connect e.g.

`Listening for transport dt_socket at address: 8000`

The `ideinit` also included a debug profile, so you can use this and add a debug into the `HotSpotGraalCompiler` class and start debugging. 

<p align="center">
  <img class="blog-image-terminal" src="/assets/images/blog/graal-hacking/debug.png">
</p>

### Installing and Using IGV

IGV - [Ideal Graph Visualizer](https://www.oracle.com/downloads/graalvm-downloads.html) is an Enterprise tool from Oracle.
The tool allows you to observe the effects that compilation phases have on the graph. 
You can find the installation for IGV at the bottom of the page (you can add this into your `graal-dev` folder).
You will need an account with Oracle to use this tool. 
The run command is the bin folder, if you're on a mac you may find it easier to use `sudo spctl --master-disable` temporarily.
You will need to add the following flag `-Dgraal.Dump` and rerun the application. 

<p align="center">
  <img class="blog-image-terminal" src="/assets/images/blog/graal-hacking/igv.png">
</p>

The view of the Graal Graph will then feed into IGV live. 

### Next Steps

Now you can start exploring or hacking with the Graal Compiler. 
All of the Graal compilation tasks are organized into classes named `*Phase`, so you can use this to take a look at any of the steps you are interested in. 

