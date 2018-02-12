------------------------------------------------------------------------------------------
JVM Internals
------------------------------------------------------------------------------------------

Questions taken from

http://www.roseindia.net/java/java-virtual-machine.shtml
http://www.interviewjava.com/2007/04/inside-java-virtual-machine.html
http://www.javabeat.net/2010/08/jvmjrejava-compiler-interview-questions/2/

Answers taken from 

http://www.artima.com/insidejvm/ed2/jvm.html

------------------------------------------------------------------------------------------

1. What is Java Virtual Machine?
2. Memory Management with JVM ?
3. Explain the Difference amongst JVM Specification, JVM Implementation, JVM Runtime.
4. Why is the source file named after the class?
5. Which class should be compiled first?
6. What are the steps in compiling a class?
7. Where is my compiled class file?
8. How can I ensure my compiler will locate the SAX package?
9. What are undefined and undeclared variables?
10. Why doesn’t the compiler warn about stack overflow problems?
11. What is a Java Virtual Machine (JVM)?
12. What does a JVM consist of?
13. What is a class loader and what is its responsibilities?
14. What is heap and stack?
15. How is your Java program executed inside JVM?
16. What is Java class file's magic number?
17. How JVM performs Thread Synchronization?
18. If your program is I/O bound or running in native methods, do these activities engage JVM?
19. What is the difference between interpreted code and compiled code?
20. Why Java based GUI intensive program has performance issues?
21. What is 64 bit Java ?
22. What is the difference between JVM and JRE?
23. Explain the processes performed by java virtual machine, i.e. loading, linking, initialization.
24. What is javap?
25. Explain the purpose of garbage collection that the JVM uses
26. How JVM performs Garbage Collection?
27. How to profile heap usage?
    Use jconsole, hprof, jstat for example.
28. What will you do if VM exits while printing "OutOfMemoryError" and increasing max heap size doesn't help?
    Same as above
29. Should one pool objects to help Garbage Collector? Should one call System.gc() periodically?
    No.
30. An application has a lot of threads and is running out of memory, why?
31. Types of GC
32. Generations of heap space
