https://www.cnblogs.com/namedgx/p/18224165
https://www.hikunpeng.com/document/detail/zh/kunpengdbs/ecosystemEnable/MySQL/kunpengmysqlmasterslave_03_0056.html

# docker启动脚本：
docker compose -f [docker-compose-mysql.yml](docker-compose-mysql.yml) up -d

# 创建网络
docker network create test_mysql_slave
docker network connect test_mysql_slave 1d5205a09177
docker network connect test_mysql_slave 4d33a9ee994e

# 进入主库执行：
-- 创建slave用户 并且 设置密码
CREATE USER 'replication_user'@'%' IDENTIFIED BY '123456';
-- 授予复制权限
GRANT REPLICATION SLAVE ON *.* TO 'replication_user'@'%';
-- 刷新权限
FLUSH PRIVILEGES;

# 执行指令
show binary log status;
[//]: # (https://dev.mysql.com/doc/refman/8.4/en/show-binary-log-status.html)

# 进入从库执行：
CHANGE REPLICATION SOURCE TO SOURCE_HOST = "localhost",
SOURCE_USER = 'replication_user', 
SOURCE_PASSWORD = '123456', 
SOURCE_PORT = 3310, 
SOURCE_LOG_FILE = 'binlog.000002', 
SOURCE_LOG_POS = 888,
GET_SOURCE_PUBLIC_KEY =1;

[//]: # (GET_SOURCE_PUBLIC_KEY =1 为了解决这个问题Message: Authentication plugin 'caching_sha2_password' reported error: Authentication requires secure connection. Error_code: MY-002061)

START REPLICA;

[//]: # (https://mysql.net.cn/doc/refman/8.0/en/change-replication-source-to.html)

# 进入主库
查看从节点 
SHOW REPLICAS
SHOW REPLICA STATUS



