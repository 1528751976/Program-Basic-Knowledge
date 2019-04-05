1.0 简要描述如何安装配置apache的一个开源hadoop，只描述即可，无需列出具体步骤，列出具体步骤更好。<br>

答：第一题：1使用root账户登录<br>

2 修改IP<br>

3 修改host主机名<br>

4 配置SSH免密码登录<br>

5 关闭防火墙<br>

6 安装JDK<br>

6 解压hadoop安装包<br>

7 配置hadoop的核心文件 hadoop-env.sh，core-site.xml , mapred-site.xml ， hdfs-site.xml<br>

8 配置hadoop环境变量<br>

9 格式化 hadoop namenode-format<br>

10 启动节点start-all.sh<br>

2.0 请列出正常的hadoop集群中hadoop都分别需要启动 哪些进程，他们的作用分别都是什么，请尽量列的详细一些。<br>

答：namenode：负责管理hdfs中文件块的元数据，响应客户端请求，管理datanode上文件block的均衡，维持副本数量<br>

Secondname:主要负责做checkpoint操作；也可以做冷备，对一定范围内数据做快照性备份。<br>

Datanode:存储数据块，负责客户端对数据块的io请求<br>

Jobtracker :管理任务，并将任务分配给 tasktracker。<br>

Tasktracker: 执行JobTracker分配的任务。<br>

Resourcemanager<br>

Nodemanager<br>

Journalnode<br>

Zookeeper<br>

Zkfc<br>

3.0请写出以下的shell命令<br>

（1）杀死一个job<br>

（2）删除hdfs上的 /tmp/aaa目录<br>

（3）加入一个新的存储节点和删除一个节点需要执行的命令<br>

答：（1）hadoop job –list 得到job的id，然后执 行 hadoop job -kill jobId就可以杀死一个指定jobId的job工作了。<br>

（2）hadoopfs -rmr /tmp/aaa<br>

(3) 增加一个新的节点在新的几点上执行<br>

Hadoop daemon.sh start datanode<br>

Hadooop daemon.sh start tasktracker/nodemanager<br>

下线时，要在conf目录下的excludes文件中列出要下线的datanode机器主机名<br>

然后在主节点中执行 hadoop dfsadmin -refreshnodes à下线一个datanode<br>

删除一个节点的时候，只需要在主节点执行<br>

hadoop mradmin -refreshnodes ---à下线一个tasktracker/nodemanager<br>

4.0 请列出你所知道的hadoop调度器，并简要说明其工作方法<br>
<br>
答：Fifo schedular :默认，先进先出的原则<br>

Capacity schedular :计算能力调度器，选择占用最小、优先级高的先执行，依此类推。<br>

Fair schedular:公平调度，所有的 job 具有相同的资源。<br>

5.0 请列出你在工作中使用过的开发mapreduce的语言<br>

答：java，hive，（python，c++）hadoop streaming<br>

6.0 当前日志采样格式为<br>

a , b , c , d<br>

b , b , f , e<br>

a , a , c , f<br>

请你用最熟悉的语言编写mapreduce，计算第四列每个元素出现的个数<br>

答：<br>

public classWordCount1 {<br>

public static final String INPUT_PATH ="hdfs://hadoop0:9000/in";<br>

public static final String OUT_PATH ="hdfs://hadoop0:9000/out";<br>
<br>
public static void main(String[] args)throws Exception {<br>

Configuration conf = newConfiguration();<br>

FileSystem fileSystem =FileSystem.get(conf);<br>

if(fileSystem.exists(newPath(OUT_PATH))){}<br>

fileSystem.delete(newPath(OUT_PATH),true);<br>

Job job = newJob(conf,WordCount1.class.getSimpleName());<br>

//1.0读取文件，解析成key,value对<br>

FileInputFormat.setInputPaths(job,newPath(INPUT_PATH));<br>

//2.0写上自己的逻辑，对输入的可以，value进行处理，转换成新的key,value对进行输出<br>

job.setMapperClass(MyMapper.class);<br>

job.setMapOutputKeyClass(Text.class);<br>

job.setMapOutputValueClass(LongWritable.class);<br>

//3.0对输出后的数据进行分区<br>

//4.0对分区后的数据进行排序，分组，相同key的value放到一个集合中<br>

//5.0对分组后的数据进行规约<br>

//6.0对通过网络将map输出的数据拷贝到reduce节点<br>

//7.0 写上自己的reduce函数逻辑，对map输出的数据进行处理<br>

job.setReducerClass(MyReducer.class);
<br>
job.setOutputKeyClass(Text.class);
<br>
job.setOutputValueClass(LongWritable.class);
<br>
FileOutputFormat.setOutputPath(job,new Path(OUT_PATH));
<br>
job.waitForCompletion(true);
<br>
}
<br>
static class MyMapper extendsMapper<LongWritable, Text, Text, LongWritable>{
<br>
@Override<br><br>

protected void map(LongWritablek1, Text v1,<br>

org.apache.hadoop.mapreduce.Mapper.Contextcontext)<br>

throws IOException,InterruptedException {<br>

String[] split =v1.toString().split("\t");<br>

for(String words :split){<br>

context.write(split[3],1);<br>

}<br>

}<br>

}<br>

static class MyReducer extends Reducer<Text,LongWritable, Text, LongWritable>{<br>

protected void reduce(Text k2,Iterable<LongWritable> v2,<br>

org.apache.hadoop.mapreduce.Reducer.Contextcontext)<br>

throws IOException,InterruptedException {<br>

Long count = 0L;<br>

for(LongWritable time :v2){<br>

count += time.get();<br>

}<br>

context.write(v2, newLongWritable(count));<br>

}<br>

}<br>

}<br>

7.0 你认为用java ， streaming ， pipe方式开发map/reduce ， 各有哪些优点
<br>
就用过 java 和 hiveQL。
<br>
Java 写 mapreduce 可以实现复杂的逻辑，如果需求简单，则显得繁琐。
<br>
HiveQL 基本都是针对 hive 中的表数据进行编写，但对复杂的逻辑（杂）很难进行实现。写起来简单。
<br>
8.0 hive有哪些方式保存元数据，各有哪些优点
<br>
三种：自带内嵌数据库derby，挺小，不常用，只能用于单节点
<br>
mysql常用<br>

上网上找了下专业名称：single user mode..multiuser mode...remote user mode<br>

9.0 请简述hadoop怎样实现二级排序（就是对key和value双排序）<br>

第一种方法是，Reducer将给定key的所有值都缓存起来，然后对它们再做一个Reducer内排序。但是，由于Reducer需要保存给定key的所有值，可能会导致出现内存耗尽的错误。
<br>
第二种方法是，将值的一部分或整个值加入原始key，生成一个组合key。这两种方法各有优势，第一种方法编写简单，但并发度小，数据量大的情况下速度慢(有内存耗尽的危险)，
<br>
第二种方法则是将排序的任务交给MapReduce框架shuffle，更符合Hadoop/Reduce的设计思想。这篇文章里选择的是第二种。我们将编写一个Partitioner，确保拥有相同key(原始key，不包括添加的部分)的所有数据被发往同一个Reducer，还将编写一个Comparator，以便数据到达Reducer后即按原始key分组。
<br><br><br><br><br><br><br><br><br>
10.简述hadoop实现jion的几种方法
<br><br><br><br><br><br><br><br><br>
Map side join----大小表join的场景，可以借助distributed cache
<br><br><br><br><br><br><br><br><br>
Reduce side join<br><br><br><br><br><br><br><br><br>

11.0 请用java实现非递归二分查询<br><br><br><br><br><br><br><br>

1. public class BinarySearchClass<br><br><br><br><br><br><br><br>
<br><br><br><br><br><br><br><br>
2. {<br><br><br><br><br><br><br><br>

3.

4. public static int binary_search(int[] array, int value)<br><br><br><br><br><br><br>

5. {<br><br><br><br><br><br><br>

6. int beginIndex = 0;// 低位下标<br><br><br><br><br><br><br>

7. int endIndex = array.length - 1;// 高位下标<br><br><br><br><br><br><br>

8. int midIndex = -1;<br><br><br><br><br><br>

9. while (beginIndex <= endIndex) {<br><br><br><br><br><br>

10.<br><br><br><br><br><br>

midIndex = beginIndex + (endIndex - beginIndex) / 2;//防止溢出<br><br><br><br><br><br>

11. if (value == array[midIndex]) {<br><br><br><br><br><br>

12.

return midIndex;<br><br><br><br><br>

13. } else if (value < array[midIndex]) {<br><br><br><br><br>

14.
<br><br><br><br><br>
endIndex = midIndex - 1;
<br><br><br><br><br>
15. } else {
<br><br><br><br><br>
16.

beginIndex = midIndex + 1;<br><br><br><br>

17. }
<br><br><br><br>
18.
<br><br><br><br>
}<br><br><br><br>

19. return -1;<br><br><br>

20.<br><br><br>

//找到了，返回找到的数值的下标，没找到，返回-1<br><br><br>

21. }<br><br><br>

22.

23.

24.

//start 提示：自动阅卷起始唯一标识，请勿删除或增加。<br><br>

25. public static void main(String[] args)<br><br>

26.
<br><br>
{

27. System.out.println("Start...");<br><br>

28.<br><br>

int[] myArray = new int[] { 1, 2, 3, 5, 6, 7, 8, 9 };<br><br>

29. System.out.println("查找数字8的下标：");
<br><br>
30.<br><br>

System.out.println(binary_search(myArray, 8));<br>

31. }
<br>
32.

//end //提示：自动阅卷结束唯一标识，请勿删除或增加。
<br>
33. }<br>

12.0 请简述mapreduce中的combine和partition的作用<br>

答：combiner是发生在map的最后一个阶段，其原理也是一个小型的reducer，主要作用是减少输出到reduce的数据量，缓解网络传输瓶颈，提高reducer的执行效率。<br>

partition的主要作用将map阶段产生的所有kv对分配给不同的reducer task处理，可以将reduce阶段的处理负载进行分摊<br>

13.0 hive内部表和外部表的区别<br>

Hive 向内部表导入数据时，会将数据移动到数据仓库指向的路径；若是外部表，数据的具体存放目录由用户建表时指定<br>

在删除表的时候，内部表的元数据和数据会被一起删除，<br>

而外部表只删除元数据，不删除数据。<br>

这样外部表相对来说更加安全些，数据组织也更加灵活，方便共享源数据。
<br>
14. Hbase的rowKey怎么创建比较好？列簇怎么创建比较好？

答：

rowKey最好要创建有规则的rowKey，即最好是有序的。<br>

经常需要批量读取的数据应该让他们的rowkey连续；<br>

将经常需要作为条件查询的关键词组织到rowkey中；<br>

列族的创建：<br>

按照业务特点，把数据归类，不同类别的放在不同列族<br><br>

15. 用mapreduce怎么处理数据倾斜问题
<br><br>
本质：让各分区的数据分布均匀
<br><br>
可以根据业务特点，设置合适的partition策略
<br><br>
如果事先根本不知道数据的分布规律，利用随机抽样器抽样后生成partition策略再处理
<br><br>
16. hadoop框架怎么来优化
<br>
答：

可以从很多方面来进行：比如hdfs怎么优化，mapreduce程序怎么优化，yarn的job调度怎么优化，hbase优化，hive优化。。。。。。。
<br>
17. hbase内部机制是什么
<br>
答：

Hbase是一个能适应联机业务的数据库系统<br>

物理存储：hbase的持久化数据是存放在hdfs上<br>

存储管理：一个表是划分为很多region的，这些region分布式地存放在很多regionserver上
<br>
Region内部还可以划分为store，store内部有memstore和storefile
<br>
版本管理：hbase中的数据更新本质上是不断追加新的版本，通过compact操作来做版本间的文件合并

Region的split<br>

集群管理：zookeeper + hmaster（职责） + hregionserver（职责）<br>

18. 我们在开发分布式计算job的时候，是否可以去掉reduce阶段<br>

答：可以，例如我们的集群就是为了存储文件而设计的，不涉及到数据的计算，就可以将mapReduce都省掉。

比如，流量运营项目中的行为轨迹增强功能部分

怎么样才能实现去掉reduce阶段

去掉之后就不排序了，不进行shuffle操作了

19 hadoop中常用的数据压缩算法

答：

Lzo

Gzip

Default

Snapyy

如果要对数据进行压缩，最好是将原始数据转为SequenceFile 或者 Parquet File（spark）

20. mapreduce的调度模式（题意模糊，可以理解为yarn的调度模式，也可以理解为mr的内部工作流程）

答： appmaster作为调度主管，管理maptask和reducetask

Appmaster负责启动、监控maptask和reducetask

Maptask处理完成之后，appmaster会监控到，然后将其输出结果通知给reducetask，然后reducetask从map端拉取文件，然后处理；

当reduce阶段全部完成之后，appmaster还要向resourcemanager注销自己

21. hive底层与数据库交互原理

答：

Hive的查询功能是由hdfs + mapreduce结合起来实现的

Hive与mysql的关系：只是借用mysql来存储hive中的表的元数据信息，称为metastore

22. hbase过滤器实现原则

答：可以说一下过滤器的父类（比较过滤器，专用过滤器）

过滤器有什么用途：

增强hbase查询数据的功能

减少服务端返回给客户端的数据量

23. reduce之后数据的输出量有多大（结合具体场景，比如pi）

Sca阶段的增强日志（1.5T---2T）

过滤性质的mr程序，输出比输入少

解析性质的mr程序，输出比输入多（找共同朋友）

24. 现场出问题测试mapreduce掌握情况和hive的ql语言掌握情况

25.datanode在什么情况下不会备份数据

答：在客户端上传文件时指定文件副本数量为1

26.combine出现在哪个过程

答：shuffle过程中

具体来说，是在maptask输出的数据从内存溢出到磁盘，可能会调多次

Combiner使用时候要特别谨慎，不能影响最后的逻辑结果
27. hdfs的体系结构

答：

集群架构：

namenode datanode secondarynamenode

(active namenode ,standby namenode)journalnode zkfc

内部工作机制：

数据是分布式存储的

对外提供一个统一的目录结构

对外提供一个具体的响应者（namenode）

数据的block机制，副本机制

Namenode和datanode的工作职责和机制

读写数据流程

28. flush的过程

答：flush是在内存的基础上进行的，首先写入文件的时候，会先将文件写到内存中，当内存写满的时候，一次性的将文件全部都写到硬盘中去保存，并清空缓存中的文件，

29. 什么是队列

答：是一种调度策略，机制是先进先出

30. List与set的区别

答：List和Set都是接口。他们各自有自己的实现类，有无顺序的实现类，也有有顺序的实现类。

最大的不同就是List是可以重复的。而Set是不能重复的。

List适合经常追加数据，插入，删除数据。但随即取数效率比较低。

Set适合经常地随即储存，插入，删除。但是在遍历时效率比较低。

31.数据的三范式

答：

第一范式（）无重复的列

第二范式（2NF）属性完全依赖于主键 [消除部分子函数依赖]

第三范式（3NF）属性不依赖于其它非主属性 [消除传递依赖]

32.三个datanode中当有一个datanode出现错误时会怎样？

答：

Namenode会通过心跳机制感知到datanode下线

会将这个datanode上的block块在集群中重新复制一份，恢复文件的副本数量

会引发运维团队快速响应，派出同事对下线datanode进行检测和修复，然后重新上线

33.sqoop在导入数据到mysql中，如何不重复导入数据，如果存在数据问题，sqoop如何处理？

答：FAILED java.util.NoSuchElementException

此错误的原因为sqoop解析文件的字段与MySql数据库的表的字段对应不上造成的。因此需要在执行的时候给sqoop增加参数，告诉sqoop文件的分隔符，使它能够正确的解析文件字段。

hive默认的字段分隔符为'\001'

34.描述一下hadoop中，有哪些地方使用到了缓存机制，作用分别是什么？

答：

Shuffle中

Hbase----客户端/regionserver

35.MapReduce优化经验

答：(1.)设置合理的map和reduce的个数。合理设置blocksize

(2.)避免出现数据倾斜

(3.combine函数

(4.对数据进行压缩

(5.小文件处理优化：事先合并成大文件，combineTextInputformat，在hdfs上用mapreduce将小文件合并成SequenceFile大文件（key:文件名，value：文件内容）

(6.参数优化

36.请列举出曾经修改过的/etc/下面的文件，并说明修改要解决什么问题？

答：/etc/profile这个文件，主要是用来配置环境变量。让hadoop命令可以在任意目录下面执行。

/ect/sudoers

/etc/hosts

/etc/sysconfig/network

/etc/inittab

37.请描述一下开发过程中如何对上面的程序进行性能分析，对性能分析进行优化的过程。

38. 现有 1 亿个整数均匀分布，如果要得到前 1K 个最大的数，求最优的算法。

参见《海量数据算法面试大全》

39.mapreduce的大致流程

答：主要分为八个步骤

1/对文件进行切片规划

2/启动相应数量的maptask进程

3/调用FileInputFormat中的RecordReader，读一行数据并封装为k1v1

4/调用自定义的map函数，并将k1v1传给map

5/收集map的输出，进行分区和排序

6/reduce task任务启动，并从map端拉取数据

7/reduce task调用自定义的reduce函数进行处理

8/调用outputformat的recordwriter将结果数据输出

41.用mapreduce实现sql语 select count (x) from a group by b;

44.搭建hadoop集群 ， master和slaves都运行哪些服务

答：master主要是运行我们的主节点，slaves主要是运行我们的从节点。

45. hadoop参数调优

46. pig , latin , hive语法有什么不同

答：

46. 描述Hbase，ZooKeeper搭建过程

48.hadoop运行原理

答：hadoop的主要核心是由两部分组成，HDFS和mapreduce，首先HDFS的原理就是分布式的文件存储系统，将一个大的文件，分割成多个小的文件，进行存储在多台服务器上。

Mapreduce的原理就是使用JobTracker和TaskTracker来进行作业的执行。Map就是将任务展开，reduce是汇总处理后的结果。

49.mapreduce的原理

答：mapreduce的原理就是将一个MapReduce框架由一个单独的master JobTracker和每个集群节点一个slave TaskTracker共同组成。master负责调度构成一个作业的所有任务，这些的slave上，master监控它们的执行，重新执行已经失败的任务。而slave仅负责执行由maste指派的任务。

50.HDFS存储机制

答：HDFS主要是一个分布式的文件存储系统，由namenode来接收用户的操作请求，然后根据文件大小，以及定义的block块的大小，将大的文件切分成多个block块来进行保存

51.举一个例子说明mapreduce是怎么运行的。

Wordcount

52.如何确认hadoop集群的健康状况

答：有完善的集群监控体系（ganglia，nagios）

Hdfs dfsadmin –report

Hdfs haadmin –getServiceState nn1

53.mapreduce作业，不让reduce输出，用什么代替reduce的功能。

54.hive如何调优

答：hive最终都会转化为mapreduce的job来运行，要想hive调优，实际上就是mapreduce调优，可以有下面几个方面的调优。解决收据倾斜问题，减少job数量，设置合理的map和reduce个数，对小文件进行合并，优化时把握整体，单个task最优不如整体最优。按照一定规则分区。

55.hive如何控制权限

我们公司没做，不需要

56.HBase写数据的原理是什么？

答：

57.hive能像关系型数据库那样建多个库吗？

答：当然能了。

58.HBase宕机如何处理

答：宕机分为HMaster宕机和HRegisoner宕机，如果是HRegisoner宕机，HMaster会将其所管理的region重新分布到其他活动的RegionServer上，由于数据和日志都持久在HDFS中，该操作不会导致数据丢失。所以数据的一致性和安全性是有保障的。

如果是HMaster宕机，HMaster没有单点问题，HBase中可以启动多个HMaster，通过Zookeeper的Master Election机制保证总有一个Master运行。即ZooKeeper会保证总会有一个HMaster在对外提供服务。

59.假设公司要建一个数据中心，你会如何处理？

先进行需求调查分析

设计功能划分

架构设计

吞吐量的估算

采用的技术类型

软硬件选型

成本效益的分析

项目管理

扩展性

安全性，稳定性

60. 单项选择题

1. 下面哪个程序负责 HDFS 数据存储。 答案 C

a)NameNode b)Jobtracker c)Datanoded)secondaryNameNode e)tasktracker

2. HDfS 中的 block 默认保存几份？ 答案 A

a)3 份 b)2 份 c)1 份 d)不确定

3. 下列哪个程序通常与 NameNode 在一个节点启动？

a)SecondaryNameNode b)DataNodec)TaskTracker d)Jobtracker e)zkfc

4. Hadoop 作者 答案D

a)Martin Fowler b)Kent Beck c)Doug cutting

5. HDFS 默认 Block Size 答案 B

a)32MB b)64MB c)128MB

6. 下列哪项通常是集群的最主要瓶颈 答案d

a)CPU b)网络 c)磁盘 d)内存

7. 关于 SecondaryNameNode 哪项是正确的？ 答案C

a)它是NameNode的热备

b)它对内存没有要求

c)它的目的是帮助 NameNode 合并编辑日志，减少 NameNode 启动时间

d)SecondaryNameNode 应与 NameNode 部署到一个节点

多选题：

8. 下列哪项可以作为集群的管理工具 答案 ABCD (此题出题有误)

a)Puppet b)Pdsh c)Cloudera Manager d)Zookeeper

9. 配置机架感知的下面哪项正确

答案 ABC

a)如果一个机架出问题，不会影响数据读写

b)写入数据的时候会写到不同机架的 DataNode 中

c)MapReduce 会根据机架获取离自己比较近的网络数据

10. Client 端上传文件的时候下列哪项正确 答案BC

a)数据经过 NameNode 传递给 DataNode

b)Client 端将文件切分为 Block，依次上传

c)Client 只上传数据到一台 DataNode，然后由 NameNode 负责 Block 复制工作

11. 下列哪个是 Hadoop 运行的模式 答案 ABC

a)单机版 b)伪分布式 c)分布式

12. Cloudera 提供哪几种安装 CDH 的方法 答案 ABCD

a)Cloudera manager b)Tar ball c)Yum d)Rpm

判断题：全部都是错误滴

13. Ganglia 不仅可以进行监控，也可以进行告警。（ ）

14. Block Size 是不可以修改的。（ ）

15. Nagios 不可以监控 Hadoop 集群，因为它不提供 Hadoop 支持。（ ）

16. 如果 NameNode 意外终止， SecondaryNameNode 会接替它使集群继续工作。（ ）

17. Cloudera CDH 是需要付费使用的。（ ）

18. Hadoop 是 Java 开发的，所以 MapReduce 只支持 Java 语言编写。（ ）

19. Hadoop 支持数据的随机读写。（ ）

20. NameNode 负责管理 metadata， client 端每次读写请求，它都会从磁盘中读取或则

会写入 metadata 信息并反馈 client 端。（ ）

21. NameNode 本地磁盘保存了 Block 的位置信息。（ ）

22. DataNode 通过长连接与 NameNode 保持通信。（ ）

23. Hadoop 自身具有严格的权限管理和安全措施保障集群正常运行。（ ）

24. Slave节点要存储数据，所以它的磁盘越大越好。（ ）

25. hadoop dfsadmin –report 命令用于检测 HDFS 损坏块。（ ）

26. Hadoop 默认调度器策略为 FIFO（ ）

27. 集群内每个节点都应该配 RAID，这样避免单磁盘损坏，影响整个节点运行。（ ）

28. 因为 HDFS 有多个副本，所以 NameNode 是不存在单点问题的。（ ）

29. 每个 map 槽（进程）就是一个线程。（ ）

30. Mapreduce 的 input split 就是一个 block。（ ）

31. NameNode的默认Web UI 端口是 50030，它通过 jetty 启动的 Web 服务。（ ）

32. Hadoop 环境变量中的 HADOOP_HEAPSIZE 用于设置所有 Hadoop 守护线程的内存。它默认是200 GB。（ ）

33. DataNode 首次加入 cluster 的时候，如果 log中报告不兼容文件版本，那需要

NameNode执行“Hadoop namenode -format”操作格式化磁盘。（ ）

63. 谈谈 hadoop1 和 hadoop2 的区别

答：

hadoop1的主要结构是由HDFS和mapreduce组成的，HDFS主要是用来存储数据，mapreduce主要是用来计算的，那么HDFS的数据是由namenode来存储元数据信息，datanode来存储数据的。Jobtracker接收用户的操作请求之后去分配资源执行task任务。

在hadoop2中，首先避免了namenode单点故障的问题，使用两个namenode来组成namenode feduration的机构，两个namenode使用相同的命名空间，一个是standby状态，一个是active状态。用户访问的时候，访问standby状态，并且，使用journalnode来存储数据的原信息，一个namenode负责读取journalnode中的数据，一个namenode负责写入journalnode中的数据，这个平台组成了hadoop的HA就是high availableAbility高可靠。

然后在hadoop2中没有了jobtracker的概念了，统一的使用yarn平台来管理和调度资源，yarn平台是由resourceManager和NodeManager来共同组成的，ResourceManager来接收用户的操作请求之后，去NodeManager上面启动一个主线程负责资源分配的工作，然后分配好了资源之后告知ResourceManager，然后ResourceManager去对应的机器上面执行task任务。

64. 说说值对象与引用对象的区别？

65. 谈谈你对反射机制的理解及其用途？

答：java中的反射，首先我们写好的类，经过编译之后就编程了.class文件，我们可以获取这个类的.class文件，获取之后，再来操作这个类。这个就是java的反射机制。

66. ArrayList、Vector、LinkedList 的区别及其优缺点？HashMap、HashTable 的区别及其优缺点？

答：ArrayList 和Vector是采用数组方式存储数据， ，Vector由于使用了synchronized方法（线程安全）所以性能上比ArrayList要差，LinkedList使用双向链表实现存储，按序号索引数据需要进行向前或向后遍历，但是插入数据时只需要记录本项的前后项即可，所以插入数度较快！

HashMap和HashTable：Hashtable的方法是同步的，而HashMap的方法不是，Hashtable是基于陈旧的Dictionary类的，HashMap是Java 1.2引进的Map接口的一个实现。HashMap是一个线程不同步的，那么就意味着执行效率高，HashTable是一个线程同步的就意味着执行效率低，但是HashMap也可以将线程进行同步，这就意味着，我们以后再使用中，尽量使用HashMap这个类。

67. 文件大小默认为 64M，改为 128M 有啥影响？

答：更改文件的block块大小，需要根据我们的实际生产中来更改block的大小，如果block定义的太小，大的文件都会被切分成太多的小文件，减慢用户上传效率，如果block定义的太大，那么太多的小文件可能都会存到一个block块中，虽然不浪费硬盘资源，可是还是会增加namenode的管理内存压力。
69. RPC 原理？

答：

1.调用客户端句柄；执行传送参数

2.调用本地系统内核发送网络消息

3. 消息传送到远程主机

4. 服务器句柄得到消息并取得参数

5. 执行远程过程

6. 执行的过程将结果返回服务器句柄

7. 服务器句柄返回结果，调用远程系统内核

8. 消息传回本地主机

9. 客户句柄由内核接收消息

10. 客户接收句柄返回的数据

70. 对 Hadoop 有没有调优经验，没有什么使用心得？（调优从参数调优讲起）

dfs.block.size

Mapredure：

io.sort.mb

io.sort.spill.percent

mapred.local.dir

mapred.map.tasks &mapred.tasktracker.map.tasks.maximum

mapred.reduce.tasks &mapred.tasktracker.reduce.tasks.maximum

mapred.reduce.max.attempts

mapred.reduce.parallel.copies

mapreduce.reduce.shuffle.maxfetchfailures

mapred.child.java.opts

mapred.reduce.tasks.speculative.execution

mapred.compress.map.output &mapred.map.output.compression.codec

mapred.reduce.slowstart.completed.maps

72以你的实际经验，说下怎样预防全表扫描

答：

1.应尽量避免在where 子句中对字段进行null 值判断，否则将导致引擎放弃使用索引而进行全表扫描

2.应尽量避免在 where 子句中使用!=或<>操作符，否则将引擎放弃使用索引而进行全表扫

3.描应尽量避免在 where 子句中使用or 来连接条件，否则将导致引擎放弃使用索引而进行

全表扫描

4.in 和 not in，用具体的字段列表代替，不要返回用不到的任何字段。in 也要慎用，否则会导致全表扫描

5.避免使用模糊查询

6.任何地方都不要使用select* from t

73. zookeeper 优点，用在什么场合

答：极大方便分布式应用的开发；（轻量，成本低，性能好，稳定性和可靠性高）

75.把公钥追加到授权文件的命令？该命令是否在 root 用户下执行？

答：ssh-copy-id

哪个用户需要做免密登陆就在哪个用户身份下执行

76. HadoopHA 集群中各个服务的启动和关闭的顺序？

答：

77. 在 hadoop 开发过程中使用过哪些算法？其应用场景是什么？

答：排序，分组，topk，join，group

78. 在实际工作中使用过哪些集群的运维工具，请分别阐述期作用。

答：nmon ganglia nagios

79. 一台机器如何应对那么多的请求访问，高并发到底怎么实现，一个请求怎么产生的，

在服务端怎么处理的，最后怎么返回给用户的，整个的环节操作系统是怎么控制的？

80. java 是传值还是传址？

答：引用传递。传址

81. 问：你们的服务器有多少台？

100多台

82. 问：你们服务器的内存多大？

128G或者64G的

83. hbase 怎么预分区？

建表时可以通过shell命令预分区，也可以在代码中建表做预分区

《具体命令详见笔记汇总》

84. hbase 怎么给 web 前台提供接口来访问（HTABLE可以提供对 HBase的访问，但是怎么查询同一条记录的多个版本数据）？

答：使用HTable来提供对HBase的访问，可以使用时间戳来记录一条数据的多个版本。

85. .htable API 有没有线程安全问题，在程序中是单例还是多例？

多例：当多线程去访问同一个表的时候会有。

86. 你们的数据是用什么导入到数据库的？导入到什么数据库？

处理完成之后的导出：利用hive 处理完成之后的数据，通过sqoop 导出到 mysql 数据库

中，以供报表层使用。

87. 你们业务数据量多大？有多少行数据？(面试了三家，都问这个问题)

开发时使用的是部分数据，不是全量数据，有将近一亿行（8、9 千万，具体不详，一般开

发中也没人会特别关心这个问题）

88. 你们处理数据是直接读数据库的数据还是读文本数据？

将日志数据导入到 hdfs 之后进行处理

89. 你们写 hive 的 hql 语句，大概有多少条？

不清楚，我自己写的时候也没有做过统计

90. 你们提交的 job 任务大概有多少个？这些job 执行完大概用多少时间？(面试了三家，都问这个问题)

没统计过，加上测试的，会有很多

Sca阶段，一小时运行一个job，处理时间约12分钟

Etl阶段，有2千多个job，从凌晨12:00开始次第执行，到早上5点左右全部跑完

91. hive 跟 hbase 的区别是？

答：Hive和Hbase是两种基于Hadoop的不同技术--Hive是一种类SQL的引擎，并且运行MapReduce任务，Hbase是一种在Hadoop之上的NoSQL 的Key/vale数据库。当然，这两种工具是可以同时使用的。就像用Google来搜索，用FaceBook进行社交一样，Hive可以用来进行统计查询，HBase可以用来进行实时查询，数据也可以从Hive写到Hbase，设置再从Hbase写回Hive。

92. 你在项目中主要的工作任务是？

Leader

预处理系统、手机位置实时查询系统，详单系统，sca行为轨迹增强子系统，内容识别中的模板匹配抽取系统

设计、架构、技术选型、质量把控，进度节点把握。。。。。。
93. 你在项目中遇到了哪些难题，是怎么解决的？

Storm获取实时位置信息动态端口的需求

101. job 的运行流程(提交一个 job 的流程)？

102Hadoop 生态圈中各种框架的运用场景？

103. hive 中的压缩格式 RCFile、TextFile、SequenceFile

[M5] 各有什么区别？

以上 3 种格式一样大的文件哪个占用空间大小..等等

采用RCfile的格式读取的数据量（373.94MB）远远小于sequenceFile的读取量（2.59GB）

2、执行速度前者(68秒)比后者(194秒)快很多

从以上的运行进度看，snappy的执行进度远远高于bz的执行进度。

在hive中使用压缩需要灵活的方式，如果是数据源的话，采用RCFile+bz或RCFile+gz的方式，这样可以很大程度上节省磁盘空间；而在计算的过程中，为了不影响执行的速度，可以浪费一点磁盘空间，建议采用RCFile+snappy的方式，这样可以整体提升hive的执行速度。

至于lzo的方式，也可以在计算过程中使用，只不过综合考虑（速度和压缩比）还是考虑snappy适宜。

104假如：Flume 收集到的数据很多个小文件,我需要写 MR 处理时将这些文件合并

(是在 MR 中进行优化,不让一个小文件一个 MapReduce)

他们公司主要做的是中国电信的流量计费为主,专门写 MR。

105. 解释“hadoop”和“hadoop 生态系统”两个概念

109. MapReduce 2.0”与“YARN”是否等同，尝试解释说明

MapReduce 2.0 --àmapreduce + yarn

110. MapReduce 2.0 中，MRAppMaster 主要作用是什么，MRAppMaster 如何实现任务

容错的？

111. 为什么会产生 yarn,它解决了什么问题，有什么优势？

114. 数据备份,你们是多少份,如果数据超过存储容量,你们怎么处理？

3份，多加几个节点

115. 怎么提升多个 JOB 同时执行带来的压力,如何优化,说说思路？

增加运算能力

116. 你们用 HBASE 存储什么数据？

流量详单

117. 你们的 hive 处理数据能达到的指标是多少？

118.hadoop中RecorderReader的作用是什么？？？

1、 在hadoop中定义的主要公用InputFormat中，哪个是默认值？ FileInputFormat

2、 两个类TextInputFormat和KeyValueInputFormat的区别是什么？

答：TextInputFormat主要是用来格式化输入的文本文件的，KeyValueInputFormat则主要是用来指定输入输出的key,value类型的

3、 在一个运行的hadoop任务中，什么是InputSplit？

InputSplit是InputFormat中的一个方法，主要是用来切割输入文件的，将输入文件切分成多个小文件，

然后每个小文件对应一个map任务

4、 Hadoop框架中文件拆分是怎么调用的？

InputFormat --> TextInputFormat -->RecordReader --> LineRecordReader --> LineReader

5、 参考下列M/R系统的场景：hdfs块大小为64MB，输入类为FileInputFormat，有3个文件的大小分别为64KB, 65MB, 127MB

会产生多少个maptask 4个 65M这个文件只有一个切片《原因参见笔记汇总TextInputformat源码分析部分》

8、 如果没有自定义partitioner，那数据在被送达reducer前是如何被分区的？

hadoop有一个默认的分区类，HashPartioer类，通过对输入的k2去hash值来确认map输出的k2,v2送到哪一个reduce中去执行。

10、分别举例什么情况要使用 combiner，什么情况不使用？

求平均数的时候就不需要用combiner，因为不会减少reduce执行数量。在其他的时候，可以依据情况，使用combiner，来减少map的输出数量，减少拷贝到reduce的文件，从而减轻reduce的压力，节省网络开销，提升执行效率

11、Hadoop中job和tasks之间的区别是什么？

Job是我们对一个完整的mapreduce程序的抽象封装

Task是job运行时，每一个处理阶段的具体实例，如map task，reduce task，maptask和reduce task都会有多个并发运行的实例

12、hadoop中通过拆分任务到多个节点运行来实现并行计算，但某些节点运行较慢会拖慢整个任务的运行，hadoop采用全程机制应对这个情况？

Speculate 推测执行

14、有可能使hadoop任务输出到多个目录中吗？如果可以，怎么做？

自定义outputformat或者用multioutputs工具

15、如何为一个hadoop任务设置mappers的数量？

Split机制

16、如何为一个hadoop任务设置要创建reduder的数量？

可以通过代码设置

具体设置多少个，应该根据硬件配置和业务处理的类型来决定

下面是HBASE我非常不懂的地方：

1.hbase怎么预分区？

2.hbase怎么给web前台提供接口来访问（HTABLE可以提供对HTABLE的访问，但是怎么查询同一条记录的多个版本数据）？

3.htable API有没有线程安全问题，在程序中是单例还是多例？

4.我们的hbase大概在公司业务中（主要是网上商城）大概4个表，几个表簇，大概都存什么样的数据？
