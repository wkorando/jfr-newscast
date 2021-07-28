 #JFR Command
 
 ```
 jfr print recording.jfr

 jfr print --events CPULoad,GarbageCollection recording.jfr

 jfr print --json --events CPULoad recording.jfr

 jfr print --categories "GC,JVM,Java*" recording.jfr

 jfr print --events "jdk.*" --stack-depth 64 recording.jfr

 jfr summary recording.jfr

 jfr metadata recording.jfr
 ```
