由于公司要上redis cluster，但对于安全性要求较高，需要密码验证，此时codis2.x 并没有支持，3.0 支持，但3.0 各方面资料并不多，多方考虑，开始尝试redis cluster 。

但此时redis cluster 原生自带的redis-trib.rb 经过测试，我是发现不支持密码验证，为此对于集群维护就显得相当麻烦，而redis 原生命令却能很好的支持redis cluster密码认证，包括集群复制。

为此需一种自动管理节点数据迁移工具，所以利用perl编写自动化管理工具。

但不是很完善，希望能给大家一个借鉴。
##使用中难免有BUG的地方，有问题可以发送内容到979835161@qq.com 邮箱

#自动分片槽位
auto Resharding all slot to set master : 

./redis_cluster_data_move -t reshard -h host -p port -P redis 密码

#自动迁移槽位
move slot:移动slot，此时槽位为空，也就是当cluster down 时，快速将16383槽位移走，不是涉及迁移数据，保证cluster 可用

./redis_cluster_data_move -t ms -h host -p port -d target_id-r 0-16383 -P redis 密码

#move data: 
移动槽位及数据

./redis_cluster_data_move -t md -h source_host:port-target_host2:port2 -s source_id -d target_id -r 0-16383 -P redis 密码

#删除节点
del redis node: 
在集群host：port删除 node_id 

./redis_cluster_data_move -t del -h host -p port -n node_id  -P redis 密码

#add redis node:添加节点

在集群source_host:port 添加目标target_host:target_port

./redis_cluster_data_move -t add -h source_host:source_port-target_host:target_port  -P redis 密码

#add redis slave node:将节点添加为从

将host:port 添加为node_id 的从节点

./redis_cluster_data_move -t add_slave -h host -p port -n node_id -P redis 密码

#update redis slave to master :

将slave手动升级为master ，在升级时使用将host port 上级为master 

./redis_cluster_data_move -u up -h host -p port  -P redis 密码


命令参数解释：

-t 任务类型

-h     主机

-p 端口

-d 节点id

-s 源节点id

-r 槽位范围

-n 节点id

-P redis 密码


