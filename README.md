# oozie-example
oozie使用示例
创建目录：oozie-example
其目录下面包含文件job.property,workflow.xml,lib，并将打好的jar包放在lib目录下;

workflow.xml:
```xml
<workflow-app xmlns='uri:oozie:workflow:0.1' name='java-wf'>
<start to="java1"/>

<action name="java1">
<java>
<job-tracker>${jobTracker}</job-tracker>
<name-node>${nameNode}</name-node>
<configuration>
<property>
<name>mapred.job.queue.name</name>
<value>${queueName}</value>
</property>
</configuration>
<main-class>zx.soft.oozie.examples.test.HelloOozie</main-class>
</java>

<ok to ="success"/>
<error to="fail"/>
</action>
<kill name="fail">
<message> fail </message>
</kill>
<end name="success"/>
</workflow-app> 
```
job.property:

nameNode=hdfs://kafka04:8020
jobTracker=kafka04:8032
queueName=default
examplesRoot=oozie-example
oozie.wf.application.path=${nameNode}/user/hdfs/${examplesRoot}/

将本地的oozie-example上传到hdfs上和oozie.wf.application.path的路径一致的位置;

运行：
oozie job -oozie http://kafka04:11000/oozie -config oozie-example/job.property -run 

查看运行状态：
命令行方式: oozie job -oozie http://kafka04:11000/oozie -info jobID
web控制台： http://kafka04:11000/oozie

杀死挂起的job:
oozie job -oozie http://kafka04:11000/oozie -kill jobID

oozie日志位置：kafka04    /var/log/oozie
