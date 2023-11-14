# Hadoop
HDFS: 不支持文件修改 write one read many。支持数据移动。适合大数据场景，不适合小数据场景  
HDFS: 主从结构，一个主角色NameNode，多个从角色DataNode。数据分块存储，并且冗余存储。NameNode需要维护元数据（即数据存储的时间、位置等等  
元数据包含两类数据，第一类是文件自身数据（名称、权限、修改时间、大小等等），第二类是文件映射信息（记录在那个DataNode上）  
NameNode还需要维护Namespace（即文件的树形结构  
Secondary NameNode: 不能替代NameNode，只是辅助NameNode进行数据处理等  
NameNode 需要大内存，DataNode需要大磁盘  
# Hadoop 常见命令
hadoop fs -mkdir [-p] <path> -p表示目录不存在则自动创建  
hadoop fs -ls [-h] [-R] <path> -R表示递归查找子目录，-h表示人性化显示文件大小  
上传文件 hadoop fs -put [-p] [-f] <本地文件系统> <目标文件系统>  -f表示覆盖已有文件，-p表示保留访问时间修改时间以及权限  
查看文件 hadoop fs -cat <src>  
下载文件 hadoop fs -put [-p] [-f] <目标文件系统> <本地文件系统>  -f表示覆盖已有文件，-p表示保留访问时间修改时间以及权限  
拷贝文件 hadoop fs -cp <src> <dst>  
追加文件写入 hadoop fs -appendToFile <localsrc> <dst> 将本地文件追加到目标文件  
# Hadoop 上传数据
通过管道方式，Hadoop客户端通过管道方式把数据传给DN1，DN1通过管道方式传给DN2，DN2通过管道传给DN3. 这样可以充分利用带宽。  
为了确保数据传输成功，例如DN1向DN2传输数据A，那么DN2则会返回DN1一个ACK表示收到  
三副本存储策略：第一个副本优先存储本地，第二个副本存储在与第一个副本不同机架，第三个副本存储在第一个副本相同机架不同机器  
上传数据第一步：客户端创建 DistributedFileSystem对象，调用其中create方法，通过RPC请求，让NameNode创建文件。NameNode判断是否需要创建。如果通过，就返回FSDataOutputStream对象给客户端用于写数据  
第二步：客户端将数据每64kb一组写入。 DataStreamer对象向NameNode申请三个副本地址，并且流式传输。并且反方向上进行数据校验。  
第三步：传递完成后DistributedFileSystem向NameNode打招呼说传递完成，等待NameNode确认。只要一个副本传递成功，就算成功。  
# Yarn
相当于一个分布式操作系统，为不同程序调度CPU、内存  
Yarn三大组件：ResourceManager（RM），NodeManager(NM)，ApplicationMaster(AM)。前两者是物理层面，第三者是App层面（告诉App应该做什么）  
RM：主角色，统筹工作，是否允许资源  
NM：从角色，本台机器资源管理  
AM: 跟随程序出现，每一个程序都有一个AM，监督程序内部情况  
yarn执行流程：  
第一步： 客户端向RM提交申请  
第二步： AM向RM提交资源申请  
第三步： NM向RM汇报  
# Hive
Hive 将文件映射成表格，包括哪个表格对应哪个文件，表的列对应哪个字段，文件字段之间分隔符是什么  
用户写完SQL后，Hive判断SQL是否正确，并且解读SQL含义，转换为MR程序执行。即把SQL转化为MapReduce程序  

