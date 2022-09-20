#MapReduce预习
##简介
1、定义：是一种用于在大型商用硬件集群中（成千上万的结点）对海量数据（多个兆字节数据集）实施可靠的、-高容错的分布式计算框架，也是一种经典的并行计算模型。
2、基本原理：将一个复杂的问题（数据集）分布若干个简单的子问题（数据块）进行解决（Map函数）；然后对-子问题的结果进行合并（Reduce函数），得到原有问题的解（结果）。

##一、MapReduce编程模型
1、简介：MapReduce是一种思想或是一种编程模型，是一个分布式计算框架，是它的一个基础组件。
2、MapReduce编程模型主要由Mapper类和Reduce类。Mapper类用以对切分过的原始数据进行处理，Reduce则是Mapper的结果进行汇总，得到最后的输出结果。
3、根据其工作原理，可将MapReduce编程模型分为：MapReduce简单模型和MapReduce复杂模型。：MapReduce简单模型：只有Mapper过程，由Mapper产生的数据直接写入HDFS；MapReduce复杂模型：对于大部分任务来说，都需要Reduce过程，并且由于任务繁重，会启动多个Reduce来进行汇总。
##二、MapReduce数据流
###1、分片、格式化数据源（InputFormat）
InputFormat主要两个任务：一个是对源文件进行分片，并确定Mapper的数量；另一个是对各个分片进行格式化，处理成<key value>形式的数据流并传给Mapper。
###2、Map过程
Mapper接收<key，value>形式的数据，并处理成<key value>形式的数据，具体的处理过程可由用户定义。
###3、Combiner过程。
每个map()都可能会产生大量的本地输出，Combiner()的作用就是对map()端的输出先做一次合并，以减少在Map和reduce结点之间的数据传输量，提高网络I/O性能，是MapReduce的优化手段之一。
###4、Shuffle过程
该过程是指从Mapper产生的直接输出结果，经过一系列的处理，成为最终的Reducer直接输入数据为止的整个过程，分为两个阶段：Mapper端的Shuffle和Reducer端的Shuffle。
###5、Reduce过程
Reducer接受<key,{value list}>形式的数据流，形成<key，value>形式的数据输出，输出数据直接写入HDFS。
##三、MapReduce任务运行流程
###1、MRv2基本组成
a.客户端（client）：客户端用于向Yarn集群提交任务，是MapReduce用户和Yarn集群通信的唯一途径。
b.MRAppMaster：监控的调度一整套MR任务流程，每个MR任务只产生一个MRAppMaster。
c.Map Task和Reduce Task:用户定义的Map函数和Reduce函数的实例化。
###2、Yarn基本组成
Yarn是一个资源管理平台，它监控和调度整个集群资源，并负责管理集群所有任务的运行和任务资源的分配。
a.Resource Managet(RM):Resource Schedule和Application Mananger。
b.NodeManager；
c.Applicationmaster(AM);
d.container.
###3、任务流程
在Yarn架构中的MapReduce任务运行流程主要分为两个部分：一是客户端向ResourceManager提交任务，ResourceManager通知相应的NodeManager启动MRAppMaster;二是MRAppMaster启动成功之后，则由它调度整个任务的运行，直到任务完成。



