说明：
	由于之前版本存在bug，为此原来的程序已经删掉，在原来的版本上进行了修复，程序采用Perl编写，我把代码发出来供大家参考，也可自行修改。此版本已经在正式环境使用，也通过测试。但未必很多场景都能测试到，也希望各位有优化的建议或修复的bug可发我邮箱kevin6386@163.com，共同完善。
	

	+------------------------------------------------------------------------+
	|Redis Cluster Tool for Redis Cluster					 |
	|version:V1.1 2016/09/12   支持Redis Cluster 密码			      |
	|只是对Redis Cluster 原生命令进行封装和计算,没有其他改动,放心使用	     |
	|			欢迎使用Redis Cluster Tool 工具	            |
	+------------------------------------------------------------------------+
	options :
                'help',         # 帮助
                't',            # 设置参数:
				ms:移动slot,reshard 重新分片,
				del:删除节点,
				add:添加节点,
				add_slave:添加slave,
				up:升级slave,
				create:创建集群,注意主从分别为不同物理机
				up:升级
				info:集群信息
				nodes:集群节点信息
                'h',            # 主机host:如
				-h 1.1.1.1:port ,
				多个主机时:-h '1.1.1.1:port,2.2.2.2:port'用','隔开  
                'p',            # 端口
                'n',            # node 节点,如: -n 'id' 如果2个时,-n 'source-id,destion-id'
                'r',              #指定slot 范围,如:-r 0-16383
                'b',            #指定backup 地址:如 -b ip:port,-b 'ip:port,ip2:port2'
	Sample :
	
	redis_cluster_tool -t nodes -h host:port
	#查看集群节点信息
	redis_cluster_tool -t info -h host:port
	#查看集群信息
	redis_cluster_tool -t create -h 'host1:port1,host2:port2,host3:port3' -b 'backip1:port1,backip2:port2,backip3:port3'
	#创建集群,并自动根据不同物理ip配置主从关系,尽量根据对应关系来进行创建如:host1:port1 对应的备份关系为backip1:port1
	#并在创建完集群后自动进行根据master个数分片
	redis_cluster_tool -t reshard -h host:port
	#自动根据当前master 进行将16384 进行重新分片,注意在集群创建初化时进行,当线上重新分配，其中的slot会有短暂中断情况
	#可以先人工计算，然后将slot以迁移方式进行	
	redis_cluster_tool -t ms -h host:port -n node_id -r 0-16383
	#对slot进行迁移,将0-16383范围内的host:prot 的slot迁移到 nodeid 为 -n指定的节点
	redis_cluster_tool -t del -h host:port -n  node_id 
	#在host:port 中删除删除节点id
	redis_cluster_tool -t add -h source_host:source_port,target_host:target_port 
	# 添加节点到source_host:port 中
	redis_cluster_tool -t add_slave -h host:port -n node_id
	#添加slave节点:将node_id 节点添加到host中
	redis_cluster_toole -t up  -h host:port
	#将host和port实例升级weimaster
	
  使用
	编辑程序redis_cluster_tool	
	
	my $pass='redis';#设置密码
