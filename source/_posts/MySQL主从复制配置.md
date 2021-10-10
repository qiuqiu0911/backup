---
title: MySQL主从复制配置
tags:
  - 运维
date: 2020-08-04 22:54:05
---


1. #### 业务场景
多地公用一套数据，但网络有时不通；数据在中心节点统一维护，其他子节点仅进行查询。
采用MySQL主从同步方案，在网络链接正常时将主库的数据同步到从库。子节点只读取本地的数据库。

2. #### 主库配置

    1. 修改my.cnf 开启二进制日志，并设置server-id
    ```shell
    vim /etc/my.cnf
    ```
    ```
    [mysqld]
    log-bin=mysql-bin #开启二进制日志
    server-id=1 #设置server-id，必须唯一
    # 不同步哪些数据库  
    binlog-ignore-db=mysql  
    binlog-ignore-db=information_schema
    binlog-ignore-db=performance_shcema
    binlog-ignore-db=sys
    ```
    
    2. 重启mysql，连接mysql查看master的状态，并记录二进制文件名(例：mysql-bin.000003)和位置(例：73)
    ```
    # mysql -h localhost -u name -p password
    mysql > SHOW MASTER STATUS;
    +------------------+----------+--------------+------------------+
    | File             | Position | Binlog_Do_DB | Binlog_Ignore_DB |
    +------------------+----------+--------------+------------------+
    | mysql-bin.000003 | 73       | test         | manual, mysql     |
    +------------------+----------+--------------+------------------+
    ```

3. #### 从库配置

    1. 修改/etc/my.cnf,设置server-id
    ```
    [mysqld]
    server-id=2 #设置server-id，必须唯一
    ```

    2. 重启mysql，打开mysql会话，执行同步SQL语句(需要主服务器主机名，登陆凭据，二进制文件的名称和位置)
    ```
    mysql> CHANGE MASTER TO
    ->     MASTER_HOST='主库ip',
    ->     MASTER_USER='主库用户名',
    ->     MASTER_PASSWORD='主库密码',
    ->     MASTER_LOG_FILE='mysql-bin.000003',
    ->     MASTER_LOG_POS=73;
    ```

    3. 启动slave同步进程
    ```
    mysql> start slave;
    ```

    4. 查看slave同步状态
    ```
     show slave status\G;
    *************************** 1. row ***************************
               Slave_IO_State: Waiting for master to send event
                  Master_Host: mysql-master
                  Master_User: root
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: mysql-bin.000013
          Read_Master_Log_Pos: 11662
               Relay_Log_File: mysqld-relay-bin.000022
                Relay_Log_Pos: 11765
        Relay_Master_Log_File: mysql-bin.000013
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
              Replicate_Do_DB: 
          Replicate_Ignore_DB: 
    ```
    当`Slave_IO_Running`和`Slave_SQL_Running`都为YES的时候就表示主从同步设置成功了。

4. #### 验证配置
    
  在主master数据库的一张表中插入一条数据，在slave的相同数据表中查看是否有新增的数据即可验证主从复制功能是否有效。
  还可以关闭slave（mysql>stop slave;）,然后再修改master，看slave是否也相应修改（停止slave后，master的修改不会同步到slave），就可以完成主从复制功能的验证了。

