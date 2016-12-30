# MongoDB Linux Install Step. #

## 1. 下載 mongodb ##

**Location**：https://www.mongodb.com/download-center?jmp=nav#community

**Version**：Ubuntu 16.04 Linux 64-bit x64

    $ wget https://www.mongodb.com/dr/fastdl.mongodb.org/linux/mongodb-linux-x86_64-ubuntu1604-3.4.1.tgz/download
    $ tar -xf mongodb-linux-x86_64-ubuntu1604-3.4.1.tgz

## 2. 創建 `/data/db` 資料夾 ##

    $ sudo mkdir -p /data && sudo mkdir -p /data/db

## 3. 啟動 mongodb server ##

	$ cd mongodb-linux-x86_64-ubuntu1604-3.4.1/bin

### 以前台方式啟動 ###

    $ sudo ./mongod

---

### 以Deamon方式啟動 ###

要用logpath + --fork(表示背後執行)，mongodb就知道要用deamon方式啟動

    $ sudo mongo --dbpath=/data/db --logpath=/data/mongo.log --fork
	$
	$ ps -aux | grep mongo
    $ netstat -nao | grep 27017