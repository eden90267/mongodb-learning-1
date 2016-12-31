# Mongo DB 權限設定 #

## 設定順序 ##

1. admin db 的帳號設定

    - 最高權限帳號
    - 告知開啟帳號控管

2. 個別 db 的帳號設定

## DEMO ##

    > use admin
    > db
    > db.createUser({"user":"user","pwd":"password",roles:["root"]});
    > db.getUsers();
    > db.getUser("user");
    
    > use test
    > db
    > db.createUser({"user":"test","pwd":"password",roles:["dbOwner"]});
    > db.createUser({"user":"test_readonly","pwd":"password",roles:["read","dbOwner"]});
    
### Mongo DB 開啟權限控管 ###

for test，先增加些小物件進去

    > a={"key":12345}
    > db.test.save(a)
    > show collections
    
重啟 Mongo DB
    
    > mongod.exe --auth
    
再進一次 Mongo DB

    > mongo.exe
    > db
    > show collections // error：unauthorized
    
    > db.auth("test","password");
    > show collections;
    > b={"keyb":12345}
    > db.test.save(b);
    
    > db.auth("test_readonly","password");
    > show collections;
    > c={"keyc":12345};
    > db.test.save(c); // error：Write error, unauthorized
    
## 結語 ##

可以對每個資料庫新增該帳號，但要記得，要先在admin db新增帳號, 告知 Mongo DB 要開啟帳號控管，以及開啟 Mongo DB 要加--auth這個參數，權限控管模組才會生效。