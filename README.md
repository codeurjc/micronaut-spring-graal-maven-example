# micronaut-spring-graal-maven-example

*WARNING*: It still doesn't work (we are working on it)

This is a sample project to execute a SpringBoot application in a Graal native-image thanks to Micronaut (with Maven)

It is a translation from [the Micronaut-Spring official example](https://github.com/micronaut-projects/micronaut-spring/tree/master/examples/greeting-service) from Gradle to Maven.

I've use an official Micronaut example based on Maven and I've added new dependencies found in micronaut-spring example. 

You can execute this project using maven:

<pre>
$ mvn exec:java
</pre>

Then, open your browser in http://localhost:8080.

Also, if you compile and package your app with maven:

<pre>
$ mvn package
</pre>

You can execute it using the command:

<pre>
$ java -jar target/hello-world-0.1.jar
</pre>

To compile the application to a native image using Graal, you need to generate .jar file using previous command and execute the following container:

<pre>
$ docker run --rm -it -v "$PWD":/project -w /project oracle/graalvm-ce:1.0.0-rc8 /bin/bash build-native-image.sh
</pre>

*NOTE:* We are using GraalVM RC8 (instead of RC9, the latest at time of writing this doc, because in RC9 there are a [regression](https://github.com/oracle/graal/issues/494).

Unfortunately, executing this command leads to the following error:

<pre>
$ docker run --rm -it -v "$PWD":/project -w /project oracle/graalvm-ce:1.0.0-rc8 /bin/bash build-native-image.sh
Graal Class Loading Analysis Enabled.
Graal Class Loading Analysis Enabled.
Writing reflect.json file to destination: target/reflect.json
[greeting-service:41]    classlist:  15,330.92 ms
[greeting-service:41]        (cap):   1,707.94 ms
[greeting-service:41]        setup:   4,295.33 ms
[greeting-service:41]     analysis:  28,140.27 ms
fatal error: org.graalvm.compiler.java.BytecodeParser$BytecodeParserError: java.lang.ArrayStoreException
        at parsing io.micronaut.views.velocity$VelocityViewsRendererDefinitionClass.getBeanType(Unknown Source)
        at org.graalvm.compiler.java.BytecodeParser.throwParserError(BytecodeParser.java:2406)
        at com.oracle.svm.hosted.phases.SharedGraphBuilderPhase$SharedBytecodeParser.throwParserError(SharedGraphBuilderPhase.java:89)
        at org.graalvm.compiler.java.BytecodeParser.iterateBytecodesForBlock(BytecodeParser.java:3146)
        at org.graalvm.compiler.java.BytecodeParser.processBlock(BytecodeParser.java:2950)
        at org.graalvm.compiler.java.BytecodeParser.build(BytecodeParser.java:888)
        at org.graalvm.compiler.java.BytecodeParser.buildRootMethod(BytecodeParser.java:782)
        at org.graalvm.compiler.java.GraphBuilderPhase$Instance.run(GraphBuilderPhase.java:95)
        at org.graalvm.compiler.phases.Phase.run(Phase.java:49)
        at org.graalvm.compiler.phases.BasePhase.apply(BasePhase.java:197)
        at org.graalvm.compiler.phases.Phase.apply(Phase.java:42)
        at org.graalvm.compiler.phases.Phase.apply(Phase.java:38)
        at com.oracle.graal.pointsto.flow.MethodTypeFlowBuilder.parse(MethodTypeFlowBuilder.java:204)
        at com.oracle.graal.pointsto.flow.MethodTypeFlowBuilder.apply(MethodTypeFlowBuilder.java:323)
        at com.oracle.graal.pointsto.flow.MethodTypeFlow.doParse(MethodTypeFlow.java:310)
        at com.oracle.graal.pointsto.flow.MethodTypeFlow.ensureParsed(MethodTypeFlow.java:300)
        at com.oracle.graal.pointsto.flow.MethodTypeFlow.addContext(MethodTypeFlow.java:107)
        at com.oracle.graal.pointsto.DefaultAnalysisPolicy$DefaultVirtualInvokeTypeFlow.onObservedUpdate(DefaultAnalysisPolicy.java:186)
        at com.oracle.graal.pointsto.flow.TypeFlow.notifyObservers(TypeFlow.java:347)
        at com.oracle.graal.pointsto.flow.TypeFlow.update(TypeFlow.java:389)
        at com.oracle.graal.pointsto.BigBang$2.run(BigBang.java:508)
        at com.oracle.graal.pointsto.util.CompletionExecutor.lambda$execute$0(CompletionExecutor.java:174)
        at java.util.concurrent.ForkJoinTask$RunnableExecuteAction.exec(ForkJoinTask.java:1402)
        at java.util.concurrent.ForkJoinTask.doExec(ForkJoinTask.java:289)
        at java.util.concurrent.ForkJoinPool$WorkQueue.runTask(ForkJoinPool.java:1056)
        at java.util.concurrent.ForkJoinPool.runWorker(ForkJoinPool.java:1692)
        at java.util.concurrent.ForkJoinWorkerThread.run(ForkJoinWorkerThread.java:157)
Caused by: java.lang.ArrayStoreException
</pre> 


