# 實際配置 Mongo DB 叢集 #

## 配置目標圖 ##

|        	| Server01              	| Server02              	| Server03              	|
|--------	|-----------------------	|-----------------------	|-----------------------	|
| Shard1 	|  Primary Port：20001  	| Secondary Port：20001 	|  Arbiter Port：20001  	|
| Shard2 	|  Arbiter Port：20002  	|  Primary Port：20002  	| Secondary Port：20002 	|
| Shard3 	| Secondary Port：20003 	|  Arbiter Port：20003  	|  Primary Port：20003  	|
| Config 	|   Config Port：30000  	|   Config Port：30000  	|   Config Port：30000  	|
| Route  	|   Route Port：40000   	|   Route Port：40000   	|   Route Port：40000   	|


## 角色與程式對應 ##

- Shared Server

    `mongod`

- Config Server

    `mongod`

- Route Server

    `mongos`

---

## 配置指令(Shard Server) ##

    $ mongod --shardsvr --replSet shard1 --port 20001 \
             --dbpath /data/shard1 --logpath /data/shard1/shard1.log --fork

- `--shardsvr`：指定角色
- `--replSet shard1`：指定複製組名稱

## 配置指令(Config Server) ##

可配置一台**或**三台(此範例使用三台)

    $ mongod --configsvr --port 30000 \
             --dbpath /data/config --logpath /data/config/config.log --fork

## 配置指令(Route Server) ##

可配置多台(此範例使用三台)

    $ mongos --port 40000 --logpath /data/mongos.log --fork \
             --configdb Server01:30000,Server02:30000,Server03:30000

Mongos只是一個接口，不提供資料夾儲存資料

- `--configdb Server01:30000,Server02:30000,Server03:30000`：指定Config Server位址

### DEMO ###

    mkdir -p ~/data/shard1 ~/data/shard2 ~/data/config

    ./mongod --shardsvr --replSet shard1 --port 20001 --dbpath ~/data/shard1 --logpath ~/data/shard1/shard1.log --fork

    ./mongod --shardsvr --replSet shard2 --port 20002 --dbpath ~/data/shard2 --logpath ~/data/shard2/shard2.log --fork

    ./mongod --shardsvr --replSet shard3 --port 20003 --dbpath ~/data/shard3 --logpath ~/data/shard3/shard3.log --fork

    ./mongod --configsvr --port 30000 --dbpath ~/data/config --logpath ~/data/config/config.log --fork


    ./mongos --port 40000 --logpath ~/data/mongos.log --fork --configdb 192.168.1.201:30000,192.168.1.202:30000,192.168.1.203:30000

先跑mkdir、shardsvr與configsvr，再用`ps -aux | grep mongo`、`netstat -nao`看有沒有起來。

再跑mongos。

## 配置複製組(Replica Set) ##

1. 連入 Replica Set 中的任意 Shard Server

    **設定的那台將會變成Primary**

2. 編寫設定檔

        config = {
            _id: 'shard1',
            members:[
              {_id:0,host:'192.168.1.201:20001'},
              {_id:1,host:'192.168.1.202:20001'},
              {_id:2,host:'192.168.1.203:20001',arbiterOnly:'true'}
            ]
        };

3. 初始化複製組

    rs.initiate(config)

### DEMO ###

    config = {
        _id: 'shard1',
        members:[
          {_id:0,host:'192.168.1.201:20001'},
          {_id:1,host:'192.168.1.202:20001'},
          {_id:2,host:'192.168.1.203:20001',arbiterOnly:'true'}
        ]
    };

先進去Mongo DB：

    $ ./mongo --port 20001
    > config = {
        _id: 'shard1',
        members:[
          {_id:0,host:'192.168.1.201:20001'},
          {_id:1,host:'192.168.1.202:20001'},
          {_id:2,host:'192.168.1.203:20001',arbiterOnly:'true'}
        ]
    };
    > rs.initiate(config); // "ok" : 1，代表這整個Replia Set已經設定完成了
    > rs.status();

針對每個Replia Set都要做以上的動作，即完成初始化。