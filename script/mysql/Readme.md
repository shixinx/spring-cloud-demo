https://www.cnblogs.com/namedgx/p/18224165

# docker启动脚本：
docker compose -f [docker-compose-mysql.yml](docker-compose-mysql.yml) up -d

# 进入主库执行：
-- 创建slave用户 并且 设置密码
CREATE USER 'replication_user'@'%' IDENTIFIED BY '123456';
-- 授予复制权限
GRANT REPLICATION SLAVE ON *.* TO 'replication_user'@'%';
-- 刷新权限
FLUSH PRIVILEGES;

# 执行指令
SHOW MASTER STATUS;
show binary log status;
[//]: # (https://dev.mysql.com/doc/refman/8.4/en/show-binary-log-status.html)

# 进入从库执行：
CHANGE MASTER TO MASTER_HOST='127.0.0.1',          #指定主库ip
MASTER_USER='replication_user',MASTER_PASSWORD='slave_password', MASTER_PORT=3310,     #指定主库创建的账号密码
MASTER_LOG_FILE='binlog.000003',MASTER_LOG_POS=17898411;               #指定主库刚查询到的file 