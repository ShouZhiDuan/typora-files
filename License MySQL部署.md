# License MySQL部署

## 1、安装MySQL服务

### 1.1、docker-compose.yml

```yml
version: '3'
services:
   mysql:
    image: mysql:5.7
    container_name: mysql
    command: mysqld --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: Wentiliangkaihua@2021 #root帐号密码
    ports:
      - 3306:3306
    volumes:
      - ./mysql/data/db:/var/lib/mysql #MySQL系统磁盘文件
      - ./mysql/conf/my.cnf:/etc/mysql/conf.d/my.cnf #配置文件
      - ./mysql/log:/var/log/mysql #日志

```

### 1.2 my.cnf（mysql服务端配置文件）

```properties
[mysqld]
lower_case_table_names=1
max_connections=100
max_connect_errors=60
open_files_limit=1024
table_open_cache=128
read_buffer_size=20M
query_cache_size=8M
query_cache_limit=2M
```

## 2、初始化MySQL结构以及数据(数据库名见工程中datasourse配置)

![image-20210526094832227](C:\Users\dev\AppData\Roaming\Typora\typora-user-images\image-20210526094832227.png)

用工程里的medusa_license_db-mysql.sql脚本初始化。

