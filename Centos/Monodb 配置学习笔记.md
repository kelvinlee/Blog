Docker 学习笔记.
---

Mongodb 安装
---

没什么好说的 = =!

Mongodb 主从复制
---

参照这个地址 : http://dockone.io/article/181

创建证书并 copy 给各个服务器.
```
root@node *:/# mkdir -p /home/core
root@node *:/# cd /home/core
root@node *:/# openssl rand -base64 741 > mongodb-keyfile
root@node *:/# chmod 600 mongodb-keyfile
root@node *:/# sudo chown 999 mongodb-keyfile
```

主要命令: `rs.initiate()` 开启副本集, `rs.conf()` 验证初始化副本集, `rs.add("node2.example.com")` 添加备节点, `rs.add("node3.example.com",{arbiterOnly: true})` 添加仲裁者节点.

```
--smallfiles \
--keyFile /opt/keyfile/mongodb-keyfile \
--replSet "rs0"
```

mongodb启动时需要增加的参数, keyFile 指定链接的关键密匙, replSet 统一集合名称.

Mongodb 从服务器可查
---

参照这个地址 : http://docs.mongodb.org/manual/reference/method/rs.slaveOk/

Mongodb 分片
---

分片就是将,数据分成几个碎片来进行管理,提升性能.

主从(集群)可以作为一个片进入.

需要 config 服务, mongos 分配服务, 和片(mongod).

如果所有服务都不在同一主机, 那么需要配置 keyFile 与 主从一样.

config: 
```
docker run -it --privileged=true \
-v /home/core/:/opt/keyfile \
-v /home/core/config/:/data/configdb \
-d -p 20000:27017 --name config \
centos-mongodb mongod --configsvr --port 27017 --keyFile /opt/keyfile/mongodb-keyfile --smallfiles
```

mongos:
```
docker run -it --privileged=true \
-v /home/core/:/opt/keyfile \
-p 30000:27017 -d --name mongos \
centos-mongodb mongos --configdb config.example.com:20000 --keyFile /opt/keyfile/mongodb-keyfile
```
mongos 需要配置好 config 后再配置, 配置好后 , 登陆可能会没有权限, 所以先创建 root 用户.
```
use admin
db.createUser({user:"root",pwd:"password",roles:[{role:"root",db:"admin"}]})
db.auth("root","password")
```
其他工具登陆也使用这个用户名和密码.

shard1:
```
docker run -it --privileged=true \
-v /home/core/:/opt/keyfile \
-v /home/core/shard/shard1/:/data/db \
-d -p 10001:27017 --name shard1 \
centos-mongodb mongod --shardsvr --port 27017 --keyFile /opt/keyfile/mongodb-keyfile --smallfiles
```

shard2:
```
docker run -it --privileged=true \
-v /home/core/:/opt/keyfile \
-v /home/core/shard/shard2/:/data/db \
-d -p 10002:27017 --name shard2 \
centos-mongodb mongod --shardsvr --port 27017 --keyFile /opt/keyfile/mongodb-keyfile --smallfiles
```

回到 mongos
```
sh.addShard("shard1.example.com:10001")
sh.addShard("shard2.example.com:10002")
```
同上选一种
```
db.runCommand({addShard:"shard1.example.com:10001"})
db.runCommand({addShard:"shard2.example.com:10002"})
```
删除片
```
db.runCommand({removeshard:"shard1.example.com:10001"})
```

为数据和表指定分片模式, 好像建议不选择 _id 为分片 key.
```
db.runCommand({enablesharding:"myTest"})
db.runCommand({shardcollection:"myTest.test",key:{_id:1}})
```

至此分片完成, 可以进行实际操作.