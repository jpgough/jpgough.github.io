---
layout: post
title: "Hacking with the Graal Compiler for Java"
date: 2020-03-29
---

Earlier this month I gave a a talk a [QCon London](https://qconlondon.com) titled [How the HotSpot and Graal JVMs execute Java Code](https://qconlondon.com/london2020/presentation/hotspot-graal-jvms-execute-java-code).
I have re-branded this talk to have the slightly more descriptive title for the LJC "How the JVM Executes Java: Evolving Compilation with Graal".
There is a video of this talk from the [NY Java Sig](https://youtu.be/oIcU6Emxj_s). 
You can also find the [slides here](/assets/slide-decks/graal-compiler-java-printable.pdf).

This blog posts goes into how to compile the Graal JIT and use it with the JVM. 
This is based on an excellent post [Understanding How Graal Works - a Java JIT Compiler Written in Java](https://chrisseaton.com/truffleruby/jokerconf17/) written by [Chris Seaton](https://chrisseaton.com) on the subject in 2017.
Some of the tooling has moved on, which makes it much easier to play with Graal and Graal as a JIT compiler. 

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
We can use our `graal-dev` folder to keep our tools together, you can add mx into your shell profile if desired. 

```shell
git clone https://github.com/graalvm/mx.git
export PATH=$PWD/mx:$PATH 
```

### Building the Compiler

Before we proceed we can check out if we can build the Graal Compiler.

```shell
cd graal/compiler/
mx build
```

You might get an error about not being able to find a JDK e.g. **Could not find a JDK**.

You fix this by specifying a Java Home either with the command or following the error to configure these for the suite.
A suite is an mx term for a series of components, that is also a directory.

On my machine I simply run:

`mx --java-home="/Library/Java/JavaVirtualMachines/adoptopenjdk-11.jdk/Contents/Home" build`

### Using Our Version of the Compiler

Running `mx vm` within the folder we have just compiled in will patch the compiler into our version of java. 
For example:

```shell
$mx --java-home="/Library/Java/JavaVirtualMachines/adoptopenjdk-11.jdk/Contents/Home" vm -version 
Updating/creating /Users/jpgough/graal-dev/graal/compiler/mxbuild/darwin-amd64/graaljdks/jdk11-cmp from /Library/Java/JavaVirtualMachines/adoptopenjdk-11.jdk/Contents/Home using intermediate directory /Users/jpgough/graal-dev/graal/compiler/mxbuild/darwin-amd64/graaljdks/tmpcrv05W/jdk11-cmp since /Users/jpgough/graal-dev/graal/compiler/mxbuild/darwin-amd64/graaljdks/jdk11-cmp does not exist
openjdk version "11.0.6" 2020-01-14
OpenJDK Runtime Environment AdoptOpenJDK (build 11.0.6+10)
OpenJDK 64-Bit Server VM AdoptOpenJDK (build 11.0.6+10, mixed mode, sharing)
```

Let's prove this is using our compiler.

### Editing the Compiler

For the next step we will make a very simple change to the compiler and observe the output.
The `mx` tool also supports setup for the well known IDEs by running `mx ideinit`:

`mx --java-home="/Library/Java/JavaVirtualMachines/adoptopenjdk-11.jdk/Contents/Home" ideinit`

We can now open the project in Intellij. I open the project at `graal-dev/graal/compiler`.
We can now make a simple edit in the HotSpotGraalCompiler `compileMethod(CompilationRequest request)` function.

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

We also need a simple class to run this with, let's adopt a modified `HelloWorld`.
It might be easiest to place this in graal/compiler for now, and compile it using `javac`.

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

Running this we should now see output from our compiler. 

```
mx --java-home /Library/Java/JavaVirtualMachines/adoptopenjdk-11.jdk/Contents/Home vm -XX:+UnlockExperimentalVMOptions -XX:+EnableJVMCI -XX:+UseJVMCICompiler -XX:-TieredCompilation HelloWorld 2> /dev/null 
Jim's Compiler Version Compiling: HotSpotMethod<StringLatin1.equals(byte[], byte[])>
Jim's Compiler Version Compiling: HotSpotMethod<StringLatin1.hashCode(byte[])>
```

You will see quite a lot of output from this, this is the result of the Graal Compiler compiling itself. 
This seems weird, but if compilation happens often it makes sense that this should also be native code.
If you want to filter the compilation to just `HelloWorld` you can also add the flag `-XX:CompileOnly=HelloWorld`.

### Debugging the Compiler

It is also possible to debug the compiler, which can be really useful in understanding the process of JIT compilation.
`mx -d` trigger the debugger to begin listening e.g.

`Listening for transport dt_socket at address: 8000`

The `ideinit` also included a debug profile, so you can use this and add a debug into the `HotSpotGraalCompiler` class and start debugging. 

<p align="center">
  <img class="blog-image-terminal" src="/assets/images/blog/graal-hacking/debug.png">
</p>

### Installing and Using IGV

IGV - [Ideal Graph Visualizer](https://www.oracle.com/downloads/graalvm-downloads.html) is an Enterprise tool from Oracle.
The tool allows you to observe the effects that compilation phases have on the graph. 
The install you can find at the bottom of the page (you can add this into you `graal-dev` folder).
You will need an account with Oracle to use this tool. 
The too in in the bin folder, if you're on a mac you may find it easier to use `sudo spctl --master-disable` temporarily.
You will need to add the following flag `-Dgraal.Dump` and rerun the application. 

<p align="center">
  <img class="blog-image-terminal" src="/assets/images/blog/graal-hacking/igv.png">
</p>

The view of the Graal Graph will then feed into IGV live. 

### Next Steps

Now you can start exploring or hacking with the Graal Compiler. 
All of the Graal comilation tasks are organized into `*Phase`, so you can use this to take a look at any of the steps you are interested in. 

