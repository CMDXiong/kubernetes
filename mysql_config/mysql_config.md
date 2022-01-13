master节点上操作: Docker安装Mysql Server
用一个容器运行数据库

# 创建Docker持久化配置和数据目录
1. 创建mysql数据目录    
    ```shell
    mkdir /opt/mysql
    mkdir /opt/mysql/conf.d
    mkdir /opt/mysql/data/
    chmod  -R 777 /opt/mysql/
    ```
2. 创建my.cnf配置文件
    ```shell
    vi /opt/mysql/my.cnf
    ```
    my.cnf添加内容如下：
    ```shell
    [mysqld]
    user=mysql
    character-set-server=utf8
    default_authentication_plugin=mysql_native_password
    secure_file_priv=/var/lib/mysql
    expire_logs_days=7
    sql_mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION
    max_connections=1000
    
    [client]
    default-character-set=utf8
    
    [mysql]
    default-character-set=utf8
    ```
    
# 运行 Mysql Docker 镜像
    ```shell
     docker run \
     --name mysql57 \
     -p 3306:3306 \
     -v /opt/mysql/data:/var/lib/mysql \
     -v /opt/mysql/log:/var/log/mysql \
     -v /opt/mysql/my.cnf:/etc/mysql/my.cnf:rw \
     -e MYSQL_ROOT_PASSWORD=password \
     -d registry.cn-beijing.aliyuncs.com/qingfeng666/mysql:5.7 --default-authentication-plugin=mysql_native_password
    ```

1. 进入镜像，登陆mysql容器，执行:
    ```shell
    UPDATE mysql.user SET host=’%’ WHERE user=‘root’;
    flush privileges;
    ```

# mysql 客户端安装
1. 安装 
    > yum install -y mysql
2. 登录
    > mysql -uroot -h127.0.0.1 -p
    
# 登录mysql server， 密码为 password
    > mysql -uroot -h127.0.0.1 -p

# 连接 Mysql Server
  创建数据库blogDB，并且选择字符集 UTF-8.
  也可以通过客户端创建数据库：
  > CREATE DATABASE `blogDB` CHARACTER SET utf8 COLLATE utf8_general_ci;



