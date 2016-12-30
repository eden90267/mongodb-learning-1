# 實際配置 Mongo DB 叢集 #

## 配置目標圖 ##

|        	| Server01              	| Server02              	| Server03              	|
|--------	|-----------------------	|-----------------------	|-----------------------	|
| Shard1 	|  Primary Port：20001  	| Secondary Port：20001 	|  Arbiter Port：20001  	|
| Shard2 	|  Arbiter Port：20001  	|  Primary Port：20002  	| Secondary Port：20002 	|
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

