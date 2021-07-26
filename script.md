Thank you Nicolai,

Hey java developers, running into difficult to debug production issues? Frustrating performance issues? Or just want to get a detailed understanding of how your application is performing in production? All the above?

I think we have all been there at one point or another in our careers

[aside] or for you newer developers out there, you will be eventually

Well for these occasionas JDK Flight recorder might be the tool for you! 

JDK Flight Recorder, JFR, previously known as Java Flight Recorder before being open sourced as part of JDK 11, JEP 328, is a JVM tool for performing diagnostics and profiling of Java applications and the JVM. 

I actually covered JFR in a recent SipOfJava. You can check that out with a link in the description. 

## Using JFR

JFR is enabled through the -XX:StartFlightRecording JVM arg. 

What is key about JFR is it has very low overhead, with an overhead of less than 1% in most use cases. Though this can vary depending upon system and how JFR is configured.

This allows JFR to be used in production settings, even under very heavy load production settings, without meaningfully impacting performance.

JFR records events across the entire stack, from the OS layer, to the JVM, to the Java application itself. This allows you to get a clear picture of what is happening  through to  is able to have a small performance impact by writing to an in-memory buffer instead of to disk. 

At initialization JVM will print to console the PID the Java application is running on, and how to deform a dump using JCMD (J Command) pulling data from the JVM.  

Running a JCMD dump, will generate a JFR, java flight recording, file. 

### Reading a JFR file

JDK Mission Control (JMC) can be used for reading JFR files. 

Opening up the JFR file in mission control, we are given an overall report about performance, and then on the left are several tabs for digging down into what is happening in the application and the JVM.

### JFR Bug 

If we go into the Memory tab... we see a bit of an issue, we aren't seeing anything recorded here? 

Link: https://www.oracle.com/java/technologies/javase/16-all-relnotes.html#JDK-8257602

With JDK 16, a new JFR event was added, jdk.ObjectAllocationSample, which allows low-overhead allocation profiling. When JMC 8.1 is released, this event will be captured, but untilthen, if we want to capture detailed memory allocation information, we will need to update the JFC files. 

This is good, as this will give us an opportunity to begin looking at configuring JFR. 

### Configure JFR

The JDK 16 release note gives us information on how to resolve this issue. 

As mentioned a new JFR event was added, jdk.ObjectAllocationSample, which has lower overhead than the jdk.ObjectAllocationInNewTLAB, and dk.ObjectAllocationOutsideTLAB events, which have been disabled by default in the default and profile JFR configuration settings. 

To enable capturing, jdk.ObjectAllocationInNewTLAB, and dk.ObjectAllocationOutsideTLAB events we will need to edit the jfc files. 

These files are located in  <java_home>/lib/jfr.

There are two files already located here; default and profile. 

....

Streams updates to JFR?
GraalVM works as well:
https://twitter.com/gunnarmorling/status/1418547439099854849