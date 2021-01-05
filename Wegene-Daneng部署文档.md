# Wegene、Daneng 系统部署文档

## 一、达能-锘崴

### 1、部署内容

```tex
1、ra-client
2、daneng-web-server
3、redis
```

### 2、部署前置

```tex
2-1、
mkdir -p /home/nvx_hz/daneng  创建部署目录,用于存放compose文件。

2-2、
cd /home/nvx_hz/daneng    进入当前目录
touch  .env               创建docker-compose启动加载外部环境变量文件。配置规则见下面第3部分.env。
touch  docker-compose.yml 创建应用启动docker-compose文件。配置规则见下面第3部分docker-compose.yml。

2-3、
mkdir -p /home/nvx_hz/redis/conf   创建redis持久化bind目录以及redis.conf存档目录。
cd /home/nvx_hz/redis/conf         进入当前目录
touch  redis.conf                  创建redis启动配置文件。配置规则见下面第3部分redis.conf。
```

### 3、配置规则

#### .env文件内容

```properties
#配置达能-web服务端口
DANENG_PORT=1091

#redis配置 这里REDIS_HOST就用容器名通信
REDIS_HOST=redis
REDIS_PORT=6379

#达能调用Wegene-web地址，即wegene-web部署的地址。已下是实例，根据实际部署调整http://192.168.10.10:1092部分就好了。
QUERY_WEGENE_RUN_URL=http://192.168.10.10:1092/wegene/queryWegene

#daneng-web镜像tag
IMAGE_TAG=registry.cn-hangzhou.aliyuncs.com/dsz-docker/daneng-web:0.0.1-SNAPSHOT
```

#### redis.conf文件内容

```properties
protected-mode no
port 6379
tcp-backlog 511
timeout 0
tcp-keepalive 300
daemonize no
supervised no
pidfile /var/run/redis_6379.pid
loglevel notice
logfile ""
databases 16
always-show-logo yes
save 900 1
save 300 10
save 60 10000
stop-writes-on-bgsave-error yes
rdbcompression yes
rdbchecksum yes
dbfilename dump.rdb
rdb-del-sync-files no
dir ./
replica-serve-stale-data yes
replica-read-only yes
repl-diskless-sync no
repl-diskless-sync-delay 5
repl-diskless-load disabled
repl-disable-tcp-nodelay no
replica-priority 100
acllog-max-len 128
lazyfree-lazy-eviction no
lazyfree-lazy-expire no
lazyfree-lazy-server-del no
replica-lazy-flush no
lazyfree-lazy-user-del no
appendonly no
appendfilename "appendonly.aof"
appendfsync everysec
no-appendfsync-on-rewrite no
auto-aof-rewrite-percentage 100
auto-aof-rewrite-min-size 64mb
aof-load-truncated yes
aof-use-rdb-preamble yes
lua-time-limit 5000
slowlog-log-slower-than 10000
slowlog-max-len 128
latency-monitor-threshold 0
notify-keyspace-events ""
hash-max-ziplist-entries 512
hash-max-ziplist-value 64
list-max-ziplist-size -2
list-compress-depth 0
set-max-intset-entries 512
zset-max-ziplist-entries 128
zset-max-ziplist-value 64
hll-sparse-max-bytes 3000
stream-node-max-bytes 4096
stream-node-max-entries 100
activerehashing yes
client-output-buffer-limit normal 0 0 0
client-output-buffer-limit replica 256mb 64mb 60
client-output-buffer-limit pubsub 32mb 8mb 60
hz 10
dynamic-hz yes
aof-rewrite-incremental-fsync yes
rdb-save-incremental-fsync yes
jemalloc-bg-thread yes
```

#### docker-compose.yml

```yml
version: "3.7"

services:
  daneng-web:
    env_file:
      - .env
    image: ${IMAGE_TAG}
    container_name: daneng-web-server
    ports:
      - ${DANENG_PORT}:${DANENG_PORT}
    depends_on:
      - redis
    networks:
      - localnet
    restart: always  
    
  redis:
    image: redis
    container_name: redis
    hostname: redis #redis容器主机名
    restart: always
    ports:
      - 6379:6379
    networks:
      - localnet
    volumes:
      #挂载redis.conf配置文件
      - /home/nvx_daneng/redis/conf/redis.conf:/etc/redis/redis.conf:rw
      #挂载redis持久数据目录
      - /home/nvx_daneng/redis/data:/data:rw
    command:
      #开启redis持久化 aof rdb
      redis-server /etc/redis/redis.conf --appendonly yes 
    privileged: true  #环境变量
    environment:
      - TZ=Asia/Shanghai
      - LANG=en_US.UTF-8
    
   networks:
    localnet:
      external:
        name: localnet
```



## 二、Wegene-锘崴

