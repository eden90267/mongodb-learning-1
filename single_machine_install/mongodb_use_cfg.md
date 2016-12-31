# MongoDB設定檔修改 #

## 建立mongo.cfg ##

    bind_ip=127.0.0.1
    port=10000
    dbpath=C:\data\db
    logpath=C:\mongod.log
    logappend=true
    journal=true

參數簡介：

- dbpath： DB 檔案所存放的目錄，預設值 `/data/db` (Linux)、`C:\data\db` (Windows)
- logpath：Log 檔案所存放的目錄，預設不存log
- bind_ip：Mongo DB 所綁定的 IP，預設值 `0.0.0.0`

    (求安全性可設置`127.0.0.1` or 192.168開頭的內網IP)

- port：Mongo DB 所綁定的 Port，預設值`27017`

    (求安全性可設置其他Port)

- logappend：是否 疊加上去 還是 每次啟動都創建一個然後清除舊的
- journal：有沒有一個暫存的地方

## 使用參數設定檔開啟 Mongo DB ##

- `./mongod -f /etc/mongodb.cfg` (Linux)
- `mongod.exe -f C:\mongodb.cfg` (Windows)

啟動會在背後執行。

	> netstat -nao