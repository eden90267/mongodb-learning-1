# 節點崩潰後的復原 #

primary shard node cash後恢復，會成為secondary，但並不會負載了，需要重新調整Primary

- cfg = rs.conf()
- cfg.members[0].priority = 1
- cfg.members[1].priority = 0.5
- cfg.members[2].priority = 0.5
- rs.reconfig(cfg);

重新調整primary的方式類似replica Set初始化設定檔

## DEMO ##

Server02：

    $ ./mongo --port 20001
    > rs.status();
    
將Server01連線中斷

    > rs.status(); // 等待更新... Server02 的 stateStr 就會變 PRIMARY
    
將Server01連線接回

    > rs.status(); // 等待更新... Server01 的 stateStr 就會變 SECONDARY
    
重寫config

    > cfg = rs.config();
    > cfg.members[0].priority=1
    > cfg.members[0].priority=0.5
    > cfg.members[0].priority=0.5
    > cfg
    > rs.reconfig(cfg); // 就可以看到 Server01 的 stateStr重新變 PRIMARY 了