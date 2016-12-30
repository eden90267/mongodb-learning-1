# 指令介面連線 #

使用mongo連線至mongo DB

參數簡介：

- --host：欲連線的伺服器，預設值 localhost
- --port：伺服器上的port，預設值 27017

    > mongo.exe --host 127.0.0.1 --port 10000
    
    > db
    
    > tmp = {"key1":12345};
    > db.test1.save(tmp);
    > db.test2.save(tmp);
    > db.test3.save(tmp);
    
    > db.test1.find();
    > db.test2.find();
    > db.test3.find();
    
- 查詢資料庫狀態

	- db.stats();
	- show collections (like show tables)
	- show databases

- 切換資料庫

    - use admin
    
- 關閉資料庫

    - db.shutdownServer();