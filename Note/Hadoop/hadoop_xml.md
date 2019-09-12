---

title: Hadoop配置xml文件模板

date: 2019-09-10 10:41:48
tags: [Hadoop, Template]
categories: [Note]

---

[RAWCODE](https://raw.githubusercontent.com/qrsforever/code_blog_post/master/Note/Hadoop/hadoop_xml.md)

<!-- vim-markdown-toc GFM -->

* [XML](#xml)
    * [core-site](#core-site)
    * [hdfs-site](#hdfs-site)
    * [mapred-site](#mapred-site)
    * [yarn-site](#yarn-site)
    * [hdbase-site](#hdbase-site)
* [Default Ports](#default-ports)

<!-- vim-markdown-toc -->

<!-- more -->

# XML

## core-site

current: r3.1.1

[core-default.xml](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/core-default.xml)

```{.xml .numberLines startFrom="1"}
<?xml version="1.0" encoding="UTF-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>

<configuration>

    <!-- 指定hdfs的nameservice为bigha(hdfs-site.xml指定), 端口号默认9000 -->
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://bigha</value>
    </property>

    <!-- 指定hadoop运行时产生文件的存储路径 -->
    <property>
        <name>hadoop.tmp.dir</name>
        <value>/opt/ws/hadoop/tmp</value>
    </property>

    <!-- 删除的文件垃圾箱存放时间(以分钟为单位) -->
    <property>
        <name>fs.trash.interval</name>
        <value>1440</value>
    </property>

    <!-- 来设置SequenceFile中用到的读/写缓存大小(一页4k的倍数, 字节为单位) -->
    <property>
        <name>io.file.buffer.size</name>
        <value>65536</value>
    </property>

    <!-- 指定zookeeper地址，多个用,分割 -->
    <property>
        <name>ha.zookeeper.quorum</name>
        <value>node2:2181,node3:2181,node4:2181</value>
    </property>

    <!-- 设置zookeeper 心跳超时时间 -->
    <property>
        <name>ha.zookeeper.session-timeout.ms</name>
        <value>300000</value>
    </property>
</configuration>
```

## hdfs-site

current: r3.1.1

[hdfs-default.xml](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/hdfs-default.xml)

```{.xml .numberLines startFrom="1"}
<?xml version="1.0" encoding="UTF-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>

<configuration>
    <!-- dfs.nameservices 命名空间的逻辑名称，多个用,分割 -->
    <property>
        <name>dfs.nameservices</name>
        <value>bigha</value>
    </property>

    <!-- 指定ns1下有两个namenode，分别是nn1,nn2 -->
    <property>
        <name>dfs.ha.namenodes.bigha</name>
        <value>nn1,nn2</value>
    </property>

    <!-- 指定nn1的RPC通信地址 -->
    <property>
        <name>dfs.namenode.rpc-address.bigha.nn1</name>
        <value>node0:8020</value>
    </property>

    <!-- 指定nn1的HTTP通信地址 -->
    <property>
        <name>dfs.namenode.http-address.bigha.nn1</name>
        <value>node0:50070</value>
    </property>

    <!-- 指定nn2的RPC通信地址 -->
    <property>
        <name>dfs.namenode.rpc-address.bigha.nn2</name>
        <value>node1:8020</value>
    </property>

    <!-- 指定nn2的HTTP通信地址 -->
    <property>
        <name>dfs.namenode.http-address.bigha.nn2</name>
        <value>node1:50070</value>
    </property>

    <!-- 指定namenode的元数据存放的Journal Node的地址，必须基数，至少三个 -->
    <property>
        <name>dfs.namenode.shared.edits.dir</name>
        <value>qjournal://node2:8485;node3:8485;node4:8485/bigha</value>
    </property>

    <!--这是JournalNode进程保持逻辑状态的路径。这是在linux服务器文件的绝对路径-->
    <property>
        <name>dfs.journalnode.edits.dir</name>
        <value>/opt/ws/hadoop/journal/</value>
    </property>

    <!-- 开启namenode失败后自动切换 -->
    <property>
        <name>dfs.ha.automatic-failover.enabled</name>
        <value>true</value>
    </property>

    <!-- 配置失败自动切换实现方式 -->
    <property>
        <name>dfs.client.failover.proxy.provider.bigha</name>
        <value>org.apache.hadoop.hdfs.server.namenode.ha.ConfiguredFailoverProxyProvider</value>
    </property>

    <!-- 配置隔离机制方法，多个机制用换行分割 -->
    <property>
        <name>dfs.ha.fencing.methods</name>
        <value>
            sshfence
            shell(/bin/true)
        </value>
    </property>

    <!-- 使用sshfence隔离机制时需要ssh免登陆 -->
    <property>
        <name>dfs.ha.fencing.ssh.private-key-files</name>
        <value>/home/lidong/.ssh/id_rsa</value>
    </property>

    <!-- 配置sshfence隔离机制超时时间30秒 -->
    <property>
        <name>dfs.ha.fencing.ssh.connect-timeout</name>
        <value>30000</value>
    </property>

    <!-- 指定磁盘预留多少空间，防止磁盘被撑满用完，单位为bytes -->
    <property>
        <name>dfs.datanode.du.reserved</name>
        <value>2147483648</value>
    </property>

    <!--指定namenode名称空间的存储地址-->
    <property>
        <name>dfs.namenode.name.dir</name>
        <value>file:///opt/ws/hadoop/hdfs/name</value>
    </property>

    <!--指定datanode数据存储地址-->
    <property>
        <name>dfs.datanode.data.dir</name>
        <value>file:///opt/ws/hadoop/hdfs/data</value>
    </property>

    <!--指定数据冗余份数-->
    <property>
        <name>dfs.replication</name>
        <value>3</value>
    </property>

    <!--指定可以通过web访问hdfs目录-->
    <property>
        <name>dfs.webhdfs.enabled</name>
        <value>true</value>
    </property>

    <!-- 处理namenode线程数 -->
    <property>
        <name>dfs.namenode.handler.count</name>
        <value>200</value>
        <description>The number of server threads for the namenode.</description>
    </property>

    <!-- 处理datanode线程数 -->
    <property>
        <name>dfs.datanode.handler.count</name>
        <value>200</value>
        <description>The number of server threads for the datanode.</description>
    </property>

    <!-- 数据传输最大线程数-->
    <property>
        <name>dfs.datanode.max.transfer.threads</name>
        <value>1024</value>
    </property>

    <!-- 设置块大小 -->
    <property>
        <name>dfs.blocksize</name>
        <value>5242880</value>
    </property>

    <!-- 设置日志节点写入超时时间 -->
    <property>
        <name>dfs.qjournal.write-txns.timeout.ms</name>
        <value>300000</value>
    </property>

<!--     <property>
   -         <name>dfs.namenode.fs-limits.min-block-size</name>
   -         <value>1048576</value>
   -     </property>
   -
   -     <property>
   -         <name>dfs.namenode.fs-limits.max-blocks-per-file</name>
   -         <value>1048576</value>
   -     </property> -->

</configuration>
```

## mapred-site

current: r3.1.1

[mapred-default.xml](https://hadoop.apache.org/docs/current/hadoop-mapreduce-client/hadoop-mapreduce-client-core/mapred-default.xml)

```{.xml .numberLines startFrom="1"}
<?xml version="1.0" encoding="UTF-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>

<configuration>
    <!-- 框架MR运行在YARN -->
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>

    <!-- 设置每个job的map任务数 -->
    <property>
        <name>mapreduce.job.maps</name>
        <value>4</value>
    </property>

    <!-- 设置每个job的reduce任务数 -->
    <property>
        <name>mapreduce.job.reduces</name>
        <value>4</value>
    </property>

    <!-- 实际物理内存量，默认是1024 -->
    <property>
        <name>mapreduce.map.memory.mb</name>
        <value>1024</value>
    </property>

    <property>
        <name>mapreduce.reduce.memory.mb</name>
        <value>1024</value>
    </property>

    <!-- 设置每个任务的JVM参数, 默认是-Xmx200m (80% of memory.mb) -->
    <property>
        <name>mapreduce.map.java.opts</name>
        <value>-Xmx200m</value>
    </property>

    <property>
        <name>mapreduce.reduce.java.opts</name>
        <value>-Xmx200m</value>
    </property>

    <!-- CPU数目，默认是1 -->
    <!-- <property>
       -     <name>mapreduce.map.cpu.vcores</name>
       -     <value>1</value>
       - </property>  -->

    <!-- <property>
       -     <name>mapreduce.reduce.cpu.vcores</name>
       -     <value>1</value>
       - </property>  -->

    <!-- 设置AppMaster内存 -->
    <property>
        <name>yarn.app.mapreduce.am.resource.mb</name>
        <value>512</value>
    </property>

    <property>
        <name>mapreduce.jobhistory.address</name>
        <value>node0:10020</value>
    </property>

    <!-- 设置WEB访问jobhistory -->
    <property>
        <name>mapreduce.jobhistory.webapp.address</name>
        <value>node0:19888</value>
    </property>

</configuration>
```

## yarn-site

current: r3.1.1

[yarn-default.xml](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-common/yarn-default.xml)

```{.xml .numberLines startFrom="1"}
<?xml version="1.0" encoding="UTF-8"?>

<configuration>

    <!-- 使能日志聚合 -->
    <property>
        <name>yarn.log-aggregation-enable</name>
        <value>true</value>
    </property>

    <!-- 聚合日志在DFS文件系统保留时间 -->
    <property>
        <name>yarn.log-aggregation.retain-seconds</name>
        <value>4320</value>
    </property>

    <!-- Aggregate log (bigha fs) -->
    <property>
        <name>yarn.nodemanager.remote-app-log-dir</name>
        <value>/tmp/logs</value>
    </property>

    <!--RM失联后重新链接的时间-->
    <property>
        <name>yarn.resourcemanager.connect.retry-interval.ms</name>
        <value>2000</value>
    </property>

    <!-- 设置zookeeper服务器地址 -->
    <property>
        <name>yarn.resourcemanager.zk-address</name>
        <value>node2:2181,node3:2181,node4:2181</value>
    </property>

    <!-- 不太懂: 集群ID, 确保RM不会作为其他集群的active -->
    <property>
        <name>yarn.resourcemanager.cluster-id</name>
        <value>bigcluster</value>
    </property>

    <!--开启RM HA -->
    <property>
        <name>yarn.resourcemanager.ha.enabled</name>
        <value>true</value>
    </property>

    <!-- RM的逻辑id列表 -->
    <property>
        <name>yarn.resourcemanager.ha.rm-ids</name>
        <value>rm1,rm2</value>
    </property>

    <!-- 每个rm-id的主机名 -->
    <property>
        <name>yarn.resourcemanager.hostname.rm1</name>
        <value>node0</value>
    </property>

    <!-- 每个rm-id的主机名 -->
    <property>
        <name>yarn.resourcemanager.hostname.rm2</name>
        <value>node5</value>
    </property>

    <property>
        <name>yarn.resourcemanager.address.rm1</name>
        <value>node0:8032</value>
    </property>

    <property>
        <name>yarn.resourcemanager.address.rm2</name>
        <value>node5:8032</value>
    </property>

    <property>
        <name>yarn.resourcemanager.scheduler.address.rm1</name>
        <value>node0:8030</value>
    </property>

    <property>
        <name>yarn.resourcemanager.scheduler.address.rm2</name>
        <value>node5:8030</value>
    </property>

    <property>
        <name>yarn.resourcemanager.resource-tracker.address.rm1</name>
        <value>node0:8031</value>
    </property>

    <property>
        <name>yarn.resourcemanager.resource-tracker.address.rm2</name>
        <value>node5:8031</value>
    </property>

    <property>
        <name>yarn.resourcemanager.webapp.address.rm1</name>
        <value>node0:8088</value>
    </property>

    <property>
        <name>yarn.resourcemanager.webapp.address.rm2</name>
        <value>node5:8088</value>
    </property>

    <!--开启故障自动切换-->
    <property>
        <name>yarn.resourcemanager.ha.automatic-failover.enabled</name>
        <value>true</value>
    </property>

    <property>
        <name>yarn.resourcemanager.ha.automatic-failover.embedded</name>
        <value>true</value>
    </property>

    <property>
        <name>yarn.resourcemanager.ha.automatic-failover.zk-base-path</name>
        <value>/yarn-leader-election</value>
    </property>

    <!--开启自动恢复功能-->
    <property>
        <name>yarn.resourcemanager.recovery.enabled</name>
        <value>true</value>
    </property>

    <!-- AM启动任务不会继承父进程的classpath, 可以通过该属性告知, 或者运行jar包 -libjar指定 -->
    <property>
        <name>yarn.application.classpath</name>
        <value>
            /opt/hadoop/,
            /opt/hadoop/etc/hadoop/*,
            /opt/hadoop/share/hadoop/common/*,/opt/hadoop/share/hadoop/common/lib/*,
            /opt/hadoop/share/hadoop/hdfs/*,/opt/hadoop/share/hadoop/hdfs/lib/*,
            /opt/hadoop/share/hadoop/mapreduce/*,/opt/hadoop/share/hadoop/mapreduce/lib/*,
            /opt/hadoop/share/hadoop/yarn/*,/opt/hadoop/share/hadoop/yarn/lib/*,
            /opt/hadoop/share/hadoop/tools/lib/*,
            /opt/hbase/conf/,/opt/hbase/lib/*
        </value>
    </property>

    <property>
        <name>yarn.resourcemanager.store.class</name>
        <value>org.apache.hadoop.yarn.server.resourcemanager.recovery.ZKRMStateStore</value>
    </property>

    <!-- 设置zookeeper中数据存储目录 -->
    <property>
        <name>yarn.resourcemanager.zk-state-store.parent-path</name>
        <value>/rmstore</value>
    </property>

    <!-- Reducer取数据的方式是mapreduce_shuffle -->
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>

    <!-- <property>
       -     <name>yarn.nodemanager.aux-services.mapreduce_shuffle.class</name>
       -     <value>org.apache.hadoop.mapred.ShuffleHandler</value>
       - </property>     -->

    <!-- 总的可用物理内存量，默认是8096 -->
    <property>
        <name>yarn.nodemanager.resource.memory-mb</name>
        <value>1024</value>
    </property>

    <!-- 总的可用CPU数目，默认是8 -->
    <property>
        <name>yarn.nodemanager.resource.cpu-vcores</name>
        <value>1</value>
    </property>

    <!-- 最小可申请内存量，默认是1024 -->
    <property>
        <name>yarn.scheduler.minimum-allocation-mb</name>
        <value>256</value>
    </property>

    <!-- 最小可申请CPU数，默认是1 -->
    <property>
        <name>yarn.scheduler.minimum-allocation-vcores</name>
        <value>1</value>
    </property>

    <!-- 最大可申请内存量，默认是8096 -->
    <property>
        <name>yarn.scheduler.maximum-allocation-mb</name>
        <value>1024</value>
    </property>

    <!-- 最大可申请CPU数，默认是4 -->
    <property>
        <name>yarn.scheduler.maximum-allocation-vcores</name>
        <value>1</value>
    </property>

    <!-- 使能物理内存限制, 当大于mapreduce.reduce|map.memory.mb抛异常" -->
    <property>
        <name>yarn.nodemanager.pmem-check-enabled</name>
        <value>true</value>
    </property>

    <!-- 使能虚拟内存限制, 当大于yarn.nodemanager.vmem-pmem-ratio倍mapreduce.reduce|map.memory.mb抛异常 -->
    <property>
        <name>yarn.nodemanager.vmem-check-enabled</name>
        <value>true</value>
    </property>

    <!-- 设置虚拟内存与物理内存的倍数, 默认2.1 -->
    <property>
        <name>yarn.nodemanager.vmem-pmem-ratio</name>
        <value>6.0</value>
    </property>

    <!-- YARN 日志 -->
    <property>
        <name>yarn.log.server.url</name>
        <value>http://node0:19888/jobhistory/logs</value>
    </property>

</configuration>
```

## hdbase-site

```{.xml .numberLines startFrom="1"}
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>
    <!-- 存储在HADOOP HDFS上文件根目录路径, 如果不是HA集群, 必须与core-site.xml文件配置保持完全一致 -->
    <property>
        <name>hbase.rootdir</name>
        <value>hdfs://bigha/hbase</value>
    </property>

    <property>
        <name>zookeeper.znode.parent</name>
        <value>/hbase</value>
    </property>

    <!-- 采用分布式模式 -->
    <property>
        <name>hbase.cluster.distributed</name>
        <value>true</value>
    </property>

    <!-- zookeeper地址，端口(默认为2181) -->
    <property>
        <name>hbase.zookeeper.quorum</name>
        <value>node2,node3,node4</value>
    </property>

    <!-- hbase临时文件存储目录，比如一些数据表的预分区信息等等 -->
    <property>
        <name>hbase.tmp.dir</name>
        <value>/opt/ws/hbase/tmp</value>
    </property>

    <!-- zookeeper存储数据位置(与zoo.cfg保持一致) -->
    <property>
        <name>hbase.zookeeper.property.dataDir</name>
        <value>/opt/ws/zookeeper/data</value>
    </property>

    <!-- 指定zk的连接端口 -->
    <property>
        <name>hbase.zookeeper.property.clientPort</name>
        <value>2181</value>
    </property>

    <!-- 设置Master并发最大线程数 -->
    <property>
        <name>hbase.regionserver.handler.count</name>
        <value>10</value>
    </property>

    <!-- RegionServer与Zookeeper间的连接超时时间。
      当超时时间到后，ReigonServer会被Zookeeper从RS集群清单中移除，HMaster收到移除通知后，
      会对这台server负责的regions重新balance，让其他存活的RegionServer接管. -->
    <property>
        <name>zookeeper.session.timeout</name>
        <value>30000</value>
    </property>

    <!--一个edit版本在内存中的cache时长，默认3600000毫秒-->
    <property>
        <name>hbase.regionserver.optionalcacheflushinterval</name>
        <value>7200000</value>
    </property>

    <!--分配给HFile/StoreFile的block cache占最大堆(-Xmx setting)的比例。默认0.4意思是分配40%，设置为0就是禁用，但不推荐。-->
    <property>
        <name>hfile.block.cache.size</name>
        <value>0.3</value>
    </property>

    <!-- 设置HStoreFile的大小，当大于这个数时，就会split 成两个文件 -->
    <property>
        <name>hbase.hregion.max.filesize</name>
        <value>134217728</value>
    </property>

    <!--设置memstore的大小，当大于这个值时，写入磁盘-->
    <property>
        <name>hbase.hregion.memstore.flush.size</name>
        <value>134217728</value>
    </property>

    <!-- 设置HDFS客户端最大超时时间，尽量改大 -->
    <property>
        <name>dfs.client.socket-timeout</name>
        <value>60000 </value>
    </property>

    <!-- 端口默认:
      -     <property >
      -         <name>hbase.master.port</name>
      -         <value>60000</value>
      -     </property>
      -
      -     <property>
      -         <name>hbase.master.info.port</name>
      -         <value>60010</value>
      -     </property>
      -
      -     <property>
      -         <name>hbase.regionserver.port</name>
      -         <value>60020</value>
      -     </property>
      -
      -     <property>
      -         <name>hbase.regionserver.info.port</name>
      -         <value>60030</value>
      -     </property>
      -  -->
</configuration>
```

# Default Ports


PORT | CONFIG NAME | CONFIG VALUE
|:---: | :--- | :--- |
0 |     dfs.balancer.address | 0.0.0.0:0
9866 | dfs.datanode.address | 0.0.0.0:9866
9864 | dfs.datanode.http.address | 0.0.0.0:9864
9865 | dfs.datanode.https.address | 0.0.0.0:9865
9867 | dfs.datanode.ipc.address | 0.0.0.0:9867
8111 | dfs.federation.router.admin-address | 0.0.0.0:8111
50071 | dfs.federation.router.http-address | 0.0.0.0:50071
50072 | dfs.federation.router.https-address | 0.0.0.0:50072
8888 | dfs.federation.router.rpc-address | 0.0.0.0:8888
8480 | dfs.journalnode.http-address | 0.0.0.0:8480
8481 | dfs.journalnode.https-address | 0.0.0.0:8481
8485 | dfs.journalnode.rpc-address | 0.0.0.0:8485
0 |     dfs.mover.address | 0.0.0.0:0
50100 | dfs.namenode.backup.address | 0.0.0.0:50100
50105 | dfs.namenode.backup.http-address | 0.0.0.0:50105
9870 | dfs.namenode.http-address | 0.0.0.0:9870
9871 | dfs.namenode.https-address | 0.0.0.0:9871
9868 | dfs.namenode.secondary.http-address | 0.0.0.0:9868
9869 | dfs.namenode.secondary.https-address | 0.0.0.0:9869
50200 | dfs.provided.aliasmap.inmemory.dnrpc-address | 0.0.0.0:50200
2181 | hadoop.registry.zk.quorum | localhost:2181
10020 | mapreduce.jobhistory.address | 0.0.0.0:10020
10033 | mapreduce.jobhistory.admin.address | 0.0.0.0:10033
19888 | mapreduce.jobhistory.webapp.address | 0.0.0.0:19888
19890 | mapreduce.jobhistory.webapp.https.address | 0.0.0.0:19890
0 |     yarn.nodemanager.address | ${yarn.nodemanager.hostname}:0
8049 | yarn.nodemanager.amrmproxy.address | 0.0.0.0:8049
8048 | yarn.nodemanager.collector-service.address | ${yarn.nodemanager.hostname}:8048
8040 | yarn.nodemanager.localizer.address | ${yarn.nodemanager.hostname}:8040
8042 | yarn.nodemanager.webapp.address | ${yarn.nodemanager.hostname}:8042
8044 | yarn.nodemanager.webapp.https.address | 0.0.0.0:8044
8032 | yarn.resourcemanager.address | ${yarn.resourcemanager.hostname}:8032
8033 | yarn.resourcemanager.admin.address | ${yarn.resourcemanager.hostname}:8033
8031 | yarn.resourcemanager.resource-tracker.address | ${yarn.resourcemanager.hostname}:8031
8030 | yarn.resourcemanager.scheduler.address | ${yarn.resourcemanager.hostname}:8030
8088 | yarn.resourcemanager.webapp.address | ${yarn.resourcemanager.hostname}:8088
8090 | yarn.resourcemanager.webapp.https.address | ${yarn.resourcemanager.hostname}:8090
8089 | yarn.router.webapp.address | 0.0.0.0:8089
8091 | yarn.router.webapp.https.address | 0.0.0.0:8091
8047 | yarn.sharedcache.admin.address | 0.0.0.0:8047
8045 | yarn.sharedcache.client-server.address | 0.0.0.0:8045
8046 | yarn.sharedcache.uploader.server.address | 0.0.0.0:8046
8788 | yarn.sharedcache.webapp.address | 0.0.0.0:8788
10200 | yarn.timeline-service.address | ${yarn.timeline-service.hostname}:10200
8188 | yarn.timeline-service.webapp.address | ${yarn.timeline-service.hostname}:8188
8190 | yarn.timeline-service.webapp.https.address | ${yarn.timeline-service.hostname}:8190
