# Hadoop
HDFS: 不支持文件修改 write one read many。支持数据移动。适合大数据场景，不适合小数据场景
HDFS: 主从结构，一个主角色NameNode，多个从角色DataNode。数据分块存储，并且冗余存储。NameNode需要维护元数据（即数据存储的时间、位置等等）
元数据包含两类数据，第一类是文件自身数据（名称、权限、修改时间、大小等等），第二类是文件映射信息（记录在那个DataNode上）
NameNode还需要维护Namespace（即文件的树形结构）
# Hadoop 常见命令
hadoop fs -mkdir [-p] <path> -p表示目录不存在则自动创建
hadoop fs -ls [-h] [-R] <path> -R表示递归查找子目录，-h表示人性化显示文件大小
