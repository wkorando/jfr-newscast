Thank you Nicolai,

Hey java developers, 

running into a difficult to debug production issue? 

Frustrating performance issue?

Or just want to get a detailed understanding of how your application is performing in production? 

All the above?

We have all been there at one point or another in our careers

Well for these occasions JDK Flight recorder might be the tool for you! 

JDK Flight Recorder, 
JFR, 

previously known as Java Flight Recorder before being open sourced as part of JDK 11, 

is a JVM tool for performing diagnostics and profiling of Java applications and the JVM. 

## Using JFR

A key characteristic of JFR is it has very low overhead, 

often less than 1%. 

Though this can vary depending upon system 

and how JFR is configured.

This means JFR can be used in production, even under heavy load, without meaningfully impacting performance.

Allowing you to use JFR to find those hard to detect performance issues 

or bugs that only seem to show up in production

If you have a running Java application, 

you can actually try out a JFR right now, 

no really!

From your command line run jcmd -l

You will get back a list of Java processes running on your system, 

You can see an example of what this looks like right here

You can take the pid of the java process you want to observe 

and then run 

jcmd <pid> JFR.start 

And JFR will start recording events across the entire stack, 

from the OS layer, to the JVM, to the Java application itself. 

Giving you a good picture of what's happening. 

The events are initially recorded to an in-memory buffer, 

which is part of how JFR is able to keep it's performance impact minimal. 

To pull information from the buffer, it we again use J Command

With jcmd <pid> JFR.dump

This will dump the data currently in the JFR buffer to the directory the java process is running from.

So what do we do with a JFR recording once we have it?

### Reading a JFR file

For lighter analysis of a JFR file, there is the jfr command.

If you already have a concrete idea of what you are looking for, 

the jfr command can be a great way to quickly analyze a JFR file. 

However for most analysis you will need a tool like  

JDK Mission Control (JMC). 

Opening our recording in mission control, 

we are given an overall report about performance, 

and then on the left side of the screen

are several tabs for digging down into what is happening 

in the application and the JVM.

Threads, 

I/O, 

Garbage Collection, 

Class Loading, 

Method Profiling, 

and more can be analyzed. 

If you look under memory, 

you might notice it looking a bit empty. 

If you are using JDK 16, 

that is because of a change introduced in the latest version of Java.

### JFR Bug 

Link: https://www.oracle.com/java/technologies/javase/16-all-relnotes.html#JDK-8257602

With JDK 16, a new JFR event was added, 

jdk.ObjectAllocationSample, 

which allows low-overhead memory allocation profiling. 

JMC 8.1 when released, will support rendering this event, 

but until then if we want to see memory allocation in JMC, 

we will need to update our JFC files. 

This is good, 

as this will give us an opportunity to look at how to configure JFR. 

### Configure JFR

The JFC files are located under 

<java_home>/lib/jfr directory.

In this directory there are two provided JFC files, default, and profile. 

Previously the events 

jdk.ObjectAllocationInNewTLAB, and jdk.ObjectAllocationOutsideTLAB 

have been used to track memory allocation. 

But have been disabled by default, both in the default and profile settings

because they have somewhat high overhead. 

By setting enabled to true for both of these, 

we can track memory allocations.

Additional JFC files can be defined,

 and there is a lot of options for configuration, 
 
 but that's beyond the scope of this segment.

For these to take affect, 

we will have to restart our Java application

After doing so, 

and going throuh the earlier J Comand steps, 

we can check see that a more detailed memory allocation is provided.

### Configure JFR at Commandline

So far when starting JFR with jcmd, 

we simply have simply done JFR.start

However we are able to pass in several values that can modify JFR's behavior

Which you can see over the right side of the screen

There are a few key ones, filename, 

for prodiving a file path and name thats more descriptive than the one generated

Settings, as expected default is are the default settigns used, 

we could also use profile, 

or the name of the file for any other JFC file we create

Maxsize limits the size of the buffer JFR will write, 

the default is 250 MB, 

which is likely larger than needed in most cases  

### GraalVM Support

For GraalVM users, JFR support was recetnly announed 

And is available in GraalVM 21.2 

A link to a blog post on redhat developer is in the description that covers this announcement

## Conclusion

We only scratched the sure of JFR today, 

there's a whole lot more to cover, 

we provided some links to some great talks in the description 

and this may be a topic we return in the future on this channel 

Until then this is Billy Korando with the Inside Java newscast, 

Nicolai back to you in the studio

https://twitter.com/gunnarmorling/status/1418547439099854849