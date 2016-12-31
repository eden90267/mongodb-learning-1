# 資料庫Shard配置與測試 #

## 分片配置(Shard) ##

1. 建立多個分片
2. 設定資料庫啟用分片
3. 設定資料庫中哪個 Collection 要啟用分片

### 建立分片 ###

1. 進入admin資料庫設定

    Use admin
    
        db.runCommand({addshard: "shard1/Server01:20001,Server02:20001"});
        
     - `shard1`：指定檔案的儲存位址
     - `Server01:20001,Server02:20001`：設定Shard的Replica Set內容(**Arbiter不需要列入**)
     
#### DEMO ####

    $/mongo --port 40000
    > use admin
    > db.runCommand({addshard:"shard1/192.168.1.201:20001,192.168.1.202:20001"});
    > db.runCommand({addshard:"shard2/192.168.1.202:20002,192.168.1.203:20002"});
    > db.runCommand({addshard:"shard3/192.168.1.203:20003,192.168.1.201:20003"});
    > db.printShardingStatus();
    
Server2、Server3依序操作

### 啟用資料庫分片 ###

1. 打開資料庫Sharding

    `db.runCommand({enablesharding:"[DB name]"})`

2. 設定哪個Collection要開啟Sharding

    `db.runCommand({shardcollection:"[DB name].[Collection name]",key:{_id:1}})`
    
**P.S. 要使用runCommand的指令，必須在Admin底下執行**

#### DEMO ####

    > use admin
    > db
    > db.runCommand({enablesharding:"test"});
    > use test
    > db.printShardingStatus()
    
    > use admin
    > db
    > db.runCommand({shardcollection:"test.users", key: {_id:1}})

### Shard 測試 ###

1. 鍵入假資料

        for (var i = 1; i <= 500000; i++) db.users.insert({age:i,addr:"123"})
        
    (MongoDB資料不夠多不會自動做Sharding，但有細部參數可以做設定)

2. 觀察資料在Shard中的分布變化

        db.users.stats()
        
    資料會先進Shard1，之後Mongo DB才會慢慢分配去Shard2、Shard3
    
#### DEMO ####

    > use test
    > db
    > db.users.drop()
    > db.printShardingStatus() // 確認test資料庫是否已啟動Sharding
    > for (var i = 1; i <= 50000; i++) db.users.insert({age:i,addr:"123"})
    > db.users.stats()
    > db.stats()