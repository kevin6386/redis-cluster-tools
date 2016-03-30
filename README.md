目前redis 集群最火的是codis 和 redis cluster （官方）,但 codis 2.x 不支持密码。那么需要密码认证使用redis 集群的同学要仔细看了哦。
 
### 相信大家很多人已经使用了redis cluster，而且也肯定会用到核心应用，你是否考虑过如下问题？

  1、redis cluster无密码，被改数据<br />  
  2、redis cluster无密码，被flushall (你是否有要哭的冲动哈哈)<br />  
  3、redis cluster无密码，数据在光天化日(你对用户不负责)<br />  
  4、redis cluster无密码，你要担心各种被黑（日志好苦）<br />  
### 此时你是否需要密码认证？（我猜你想立刻马上），哈哈问题来了，你在创建集群和管理时是否遇到如下麻烦？<br />  
  1、redis cluster 官方redis-trib.rb 不支持密码，你要手工用命令一个一个加入集群<br />  
  2、添加减节点不方便<br />  
  3、更重要的是你的分片工具不能用，你要抓狂么？<br />  
  4、管理需要手工<br />  
  5、你要疯掉<br />  
### 有没有办法解决呢？有，我相信很多公司已经会用认证方式来管理，只是目前我是没搜到相关资料。怎么办？<br />  
我有办法：
  1、原封不动的封装了redis cluster 集群添加减节点功能，并支持密码认证<br />  
  2、针对对同台机器多master挂掉后集群不可用时，自动快速迁移槽位进行修复，保证程序可用<br />  
  3、自动对新加节点迁移槽位<br />  
  4、自动迁移槽位和数据给指定节点<br />  
  5、自动根据当前结点master进行自动分片<br />  

### 你是不是已经心动了呢？那么接下来让你更想行动<br />  



但不是很完善，希望能给大家一个借鉴。


### 自动分片槽位
  auto Resharding all slot to set master : 
  ./redis_cluster_data_move -t reshard -h host -p port -P redis 密码

### 自动迁移槽位
  move slot:移动slot，此时槽位为空，也就是当cluster down 时，快速将16383槽位移走，不是涉及迁移数据，保证cluster 可用
  ./redis_cluster_data_move -t ms -h host -p port -d target_id-r 0-16383 -P redis 密码

### move data: 
  移动槽位及数据

  ./redis_cluster_data_move -t md -h source_host:port-target_host2:port2 -s source_id -d target_id -r 0-16383 -P redis 密码

### 删除节点
  del redis node: 
在集群host：port删除 node_id 

  ./redis_cluster_data_move -t del -h host -p port -n node_id  -P redis 密码

### add redis node:添加节点

在集群source_host:port 添加目标target_host:target_port

  ./redis_cluster_data_move -t add -h source_host:source_port-target_host:target_port  -P redis 密码

### add redis slave node:将节点添加为从

将host:port 添加为node_id 的从节点

  ./redis_cluster_data_move -t add_slave -h host -p port -n node_id -P redis 密码

### update redis slave to master :

将slave手动升级为master ，在升级时使用将host port 上级为master 

  ./redis_cluster_data_move -u up -h host -p port  -P redis 密码


### 命令参数解释：

  -t      任务类型
  -h      主机
  -p      端口
  -d      节点id
  -s      源节点id
  -r      槽位范围
  -n      节点id
  -P      redis 密码


### 使用中难免有BUG的地方，有问题可以发送内容到979835161@qq.com 邮箱
