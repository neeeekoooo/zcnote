副本集可以在多个mongodb服务器间进行数据同步，保障了数据的安全性，可用性，灾难恢复等。副本集可以自动进行故障切换。

## 命令
```bash
#初始化副本集
rs.initiate({
            "_id" : "rs0",
            "members" : [
                    {
                            "_id" : 0,
                            "host" : "0.0.0.0:27017"
                    },
                    {
                            "_id" : 1,
                            "host" : "0.0.0.0:27018"
                    },
                    {
                            "_id" : 2,
                            "host" : "0.0.0.0:27019"
                    }
            ]
})
#查看当前配置
rs.conf()
#查看副本集状态
rs.status()
#查看所有mongod的进程命令
ps -ef | grep mongod
```