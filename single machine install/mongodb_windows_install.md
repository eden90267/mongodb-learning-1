# MongoDB Windows Install Step. #

## 1. 下載 mongodb ##

**Location**：https://www.mongodb.com/download-center?jmp=nav#community

**Version**：Windows Server 2008 R2 64-bit and later, with SSL support x64

**File Extension**：.msi

**P.S.** 安裝選"自訂"，調整安裝路徑：`C:\MongoDB`，其他就下一步下一步下一步..完成

## 2. 創建 `C:\data\db` 資料夾 ##

(mongodb 3.X 好像不用了，自己會創建)

## 3. 啟動 mongodb server ##

### 以前台方式啟動 ###

直接點選`C:\MongoDB\bin\mongod.exe`執行

### 以服務方式啟動 ###

    > mongod.exe --logpath=C:\data\mongo.log --dbpath=C:\data\db --journal --install

預設不寫log，可透過--logpath參數設置

--dbpath預設是C:\data\db

    > net start mongodb

可用工作管理員的服務確認是否有MongoDB服務在運作