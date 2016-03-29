# redis_clster_data_move
redis cluster 认证方式 管理工具

redis_cluster_data_move #程序名

auto Resharding all slot to set master : #自动分片槽位

./redis_cluster_data_move -t reshard -h host -p port

move slot:#移动slot，此时槽位为空，也就是当cluster down 时，快速将16383槽位移走，不是涉及迁移数据，保证cluster 可用

./redis_cluster_data_move -t ms -h host -p port -d target_id-r 0-16383

move data: #移动槽位及数据

./redis_cluster_data_move -t md -h source_host:port-target_host2:port2 -s source_id -d target_id -r 0-16383


del redis node: #删除节点

##在集群host：port删除 node_id

./redis_cluster_data_move -t del -h host -p port -n  node_id 

add redis node:#添加节点

#在集群source_host:port 添加目标target_host:target_port

./redis_cluster_data_move -t add -h source_host:source_port-target_host:target_port 

add redis slave node:#将节点添加为从

#将host:port 添加为node_id 的从节点

./redis_cluster_data_move -t add_slave -h host -p port -n node_id 

update redis slave to master : #将slave手动升级为master ，在升级时使用

#将host port 上级为master

./redis_cluster_data_move -u up  -h host -p port 
-t     任务类型
-h     主机
-p     端口
-d     节点id
-s     源节点id
-r     槽位范围
-n     节点id
