# 一、服务部署资源分配

## 1、192.168.10.28

- medusa
- license

## 2、192.168.10.100（Local）

| 容器名称            | 所属应用（研究方法） | 备注     | 部署目录                           |
| ------------------- | -------------------- | -------- | ---------------------------------- |
| client-torch-depseg | DepSegTraining       | 子豪计算 | /home/shouzhi/deploy-depseg/local  |
| license-local       | ImageClassification  | 资源上报 | /home/shouzhi/deploy/license-local |
| skadi-datanode      | ImageClassification  | datanode | /home/shouzhi/deploy/datanode      |



## 3、192.168.10.31（Global）

| 容器名称      | 所属应用（研究方法） | 备注 | 部署目录                                 |
| ------------- | -------------------- | ---- | ---------------------------------------- |
| global_server | ImageClassification  |      | -                                        |
| global_2.7.1  | 原来SGX              |      | /usr/local/docker-work/sgx-deploy        |
| global-depseg | DepSegTraining       |      | /usr/local/docker-work/sgx-deploy-depseg |

## 4、192.168.10.3

- skadi-local（计算端   192.168.10.3）后续会迁移到192.168.10.100





# 二、登录账号

## 1、Medusa系统

- 登录地址：https://192.168.10.28:8443/
- 账号：medusa_admin11
- 密码：-2Openhua

## 2、Skadi系统

### 2.1 图像分类研究

| 数据源    | 登录地址                      | 账户           | 密码      |
| --------- | ----------------------------- | -------------- | --------- |
| DataNode1 | https://192.168.10.100:18443/ | medusa_admin11 | -2Openhua |
| DataNode2 | https://192.168.10.100:18442/ | medusa_admin12 | -2Openhua |

### 2.2 DepSeg研究

| 数据源    | 登录地址                      | 账户           | 密码      |
| --------- | ----------------------------- | -------------- | --------- |
| DataNode1 | https://192.168.10.100:28443/ | medusa_admin21 | -2Openhua |
| DataNode2 | https://192.168.10.100:28442/ | medusa_admin22 | -2Openhua |

### 2.3 原系统研究

| 数据源     | 登录地址                      | 账户          | 密码      |
| ---------- | ----------------------------- | ------------- | --------- |
| DataNode15 | https://192.168.10.100:28444/ | medusa_admin5 | -2Openhua |
| DataNode16 | https://192.168.10.100:28446/ | medusa_admin6 | -2Openhua |



# 三、端口分配

## 1、图像分析

| 数据源-1                  | 容器名                    | 数据源-2              | 容器名                     | Global             | 备注          |
| ------------------------- | ------------------------- | --------------------- | -------------------------- | ------------------ | ------------- |
| datanode12443             | skadi-datanode            | datanode12442         | skadi-datanode66           |                    | skadi         |
| license-local13443        | skadi-license             | license-local13442    | skadi-license-image2       |                    | license-local |
| web18443                  | skadi-web                 | web18442              | skadi-web66                |                    | skadi-web     |
| datanode-deeplearning-cpp | datanode-deeplearning-cpp | datanode-deeplearning | datanode-deeplearning-cpp2 | 192.168.10.31:2080 | 本地计算      |



## 2、DepSeg	

brain-mri-2-0--1642125263158

| 数据源-1                           | 容器名              | 数据源-2                           | 容器名               | Global              | 备注          |
| ---------------------------------- | ------------------- | ---------------------------------- | -------------------- | ------------------- | ------------- |
| datanode52443                      | skadi-datanode2     | datanode52442                      | skadi-datanode3      |                     | skadi         |
| license-local14443                 | skadi-license2      | license-local14442                 | skadi-license3       |                     | license-local |
| web28080:s28443                    | skadi-web2          | web28081:s28442                    | skadi-web3           |                     | skadi-web     |
| client-torch-depseg（11000:s8080） | client-torch-depseg | client-torch-depseg（11001:s8081） | client-torch-depseg2 | 192.168.10.31:18080 | 本地计算      |
| tensorboard-nginx(80:s8200)        |                     |                                    |                      |                     |               |



# 四、模块部署

## 1、DepSeg-Global（192.168.10.31）

### 1.1 部署目录

```tex
/usr/local/docker-work/sgx-deploy-depseg
```

### 1.2 docker-compose.yml（硬件模式）

```yml
version: "3.7"
  
networks:
  localnet:
    external:
      name: localnet

services:
    global-server-new-pre:
      image: "nvxhub.nvxclouds.net:9443/sgx/img/nvxcloudstechsgxserverhw:11dcc382473f9e3cb99328c8f115d0aa12ab3d20"
      devices:
        - /dev/isgx:/dev/isgx
      container_name: global-depseg
      ports:
        - 18080-18081:18080-18081
      environment:
        - LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/opt/sgxsdk/sdk_libs
        - HTTPS_PORT=18080 #提供medusa通信端口
        - ANALYSIS_DEFAULT_PORT=18081 #提供本地计算通信端口
        - CENTRAL_API_URL=https://192.168.10.28:10443/results/complete #medusa接口
        - CENTRAL_API_UPDATES_URL=https://192.168.10.28:10443/results/updates #medusa接口
        - CENTRAL_ACCESS_TOKEN=NOVOVIVO-token-M1LG1xLV8*c4MqzQH+SVJtvy4KoLObJt
        - CENTRAL_VERIFY_SKIP=1
        - NODE_NAME=CNN计算节点
      networks:
         - localnet
      command: ["./start.sh"]
      restart: always

```

### 1.3 docker-compose.yml（软件模式）

```yml

version: "3.7"
  
networks:
  localnet:
    external:
      name: localnet

services:
    global-server-new-pre:
      image: "nvxhub.nvxclouds.net:9443/sgx/img/nvxcloudstechsgxserversw:11dcc382473f9e3cb99328c8f115d0aa12ab3d20"
      container_name: global-depseg
      ports:
        - 18080-18081:18080-18081
      environment:
        - LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/opt/sgxsdk/sdk_libs
        - HTTPS_PORT=18080 #提供medusa通信端口
        - ANALYSIS_DEFAULT_PORT=18081 #提供本地计算通信端口
        - CENTRAL_API_URL=https://192.168.10.28:10443/results/complete #medusa接口
        - CENTRAL_API_UPDATES_URL=https://192.168.10.28:10443/results/updates #medusa接口
        - CENTRAL_ACCESS_TOKEN=NOVOVIVO-token-M1LG1xLV8*c4MqzQH+SVJtvy4KoLObJt
        - CENTRAL_VERIFY_SKIP=1
        - NODE_NAME=CNN计算节点
      networks:
         - localnet
      command: ["./start.sh"]
      restart: always


```



## 2、DepSeg-Local（192.168.10.100）

### 2.1 部署目录

```tex
/home/shouzhi/deploy-depseg/local
```

### 2.2 第一套 startLocal.sh 

```powershell
#!/bin/bash
docker container stop client-torch-depseg
docker container rm client-torch-depseg
docker run --gpus device=0 \ #device对于相同的global不要相同
        -v /home/shouzhi/deploy/datanode52443/uploads:/home/uploads \ #skadi部署目录
        -v /home/shouzhi/deploy/datanode52443/outputs:/home/outputs \ #skadi部署目录
        -v /home/shouzhi/deploy/datanode52443/logs:/home/logs \ #skadi部署目录
        -e DIMAGE_HTTP_PORT=11000 \ #local本服务http端口
        -e DIMAGE_HTTPS_PORT=8080 \ #local本服务https端口
        -e DIMAGE_HOSTNAME=0.0.0.0 \
        -e DSGX_REMOTE_ATTESTATION_HOST=192.168.10.31 \ #global地址
        -e DSGX_REMOTE_ATTESTATION_PORT=18081 \ #global(ANALYSIS_DEFAULT_PORT)对应端口
        -e DIMAGE_FLAG_BYPASS_IAS_VERIFY=ON \
        --restart always \
        -it -d --name client-torch-depseg \
        --network localnet \
        nvxhub.nvxclouds.net:9443/skadi/migrate/nvxcloudstechdatanodeai:58-upcert \
        bash -c "./start.sh"
```

### 2.3 第二套 startLocal2.sh

```powershell
#!/bin/bash
docker container stop client-torch-depseg2
docker container rm client-torch-depseg2
docker run --gpus device=1 \ #device对于相同的global不要相同
        -v /home/shouzhi/deploy/datanode52442/uploads:/home/uploads \ #skadi部署目录
        -v /home/shouzhi/deploy/datanode52442/outputs:/home/outputs \ #skadi部署目录
        -v /home/shouzhi/deploy/datanode52442/logs:/home/logs \ #skadi部署目录
        -e DIMAGE_HTTP_PORT=11001 \ #local本服务http端口
        -e DIMAGE_HTTPS_PORT=8081 \ #local本服务https端口
        -e DIMAGE_HOSTNAME=0.0.0.0 \
        -e DSGX_REMOTE_ATTESTATION_HOST=192.168.10.31 \ #global地址
        -e DSGX_REMOTE_ATTESTATION_PORT=18081 \ #global(ANALYSIS_DEFAULT_PORT)对应端口
        -e DIMAGE_FLAG_BYPASS_IAS_VERIFY=ON \
        --restart always \
        -it -d --name client-torch-depseg \
        --network localnet \
        nvxhub.nvxclouds.net:9443/skadi/migrate/nvxcloudstechdatanodeai:58-upcert \
        bash -c "./start.sh"
```



## 3、Image-Local（192.168.10.100）

### 3.1 部署目录

```tex
/home/shouzhi/deploy-image/local
```

### 3.2 第一套 

#### docker-compose.yml 

```yml
version: "3.7"

services:
    datanode-deeplearning-cpp:
      image: "nvxhub.nvxclouds.net:9443/ai/datanode-deeplearning-cpp-stable:0.1.1"
      container_name: datanode-deeplearning-cpp
      environment:
        - DIMAGE_HTTP_PORT=12000
        - DIMAGE_HTTPS_PORT=2080
        - DIMAGE_HOSTNAME=0.0.0.0
        - DSGX_REMOTE_ATTESTATION_PORT=2081
        - DSGX_REMOTE_ATTESTATION_HOST=192.168.10.31
        - DIMAGE_FLAG_BYPASS_IAS_VERIFY=ON
        - DeepLearningHost=192.168.10.100:50156
        - DeepLearningDevice=cuda:0
      command: ["./start.sh"]
      tty: true
      volumes:
        - ./data/outputs:/home/outputs
        - ./data/logs:/home/logs
        - /home/ltguo/nvxclouds/DataNode_AI/:/home/DataNode/
      networks:
        - localnet
      restart: always
      ports:
        - 12000:12000
        - 2080:2080
networks:
  localnet:
    external:
      name: localnet

```

#### docker-py-run.sh

```powershell
#!/bin/bash

CONTAINER_NAME=datanode-deeplearning-cpp
DOCKER_IMAGE=nvxhub.nvxclouds.net:9443/ai/datanode-deeplearning-cpp:0.1

docker container stop ${CONTAINER_NAME}
docker container rm ${CONTAINER_NAME}

docker run \
        -v ${PWD}/data/outputs:/home/outputs \
        -v ${PWD}/data/logs:/home/logs \
        -v ${PWD}:/home/DataNode \
        -e DIMAGE_HTTP_PORT=11000 \
        -e DeepLearningHost=192.168.10.3:50055 \
        -e DeepLearningDevice="cuda:0" \
        -e DIMAGE_HTTPS_PORT=8086 \
        -e DIMAGE_HOSTNAME=0.0.0.0 \
        -e DSGX_REMOTE_ATTESTATION_HOST=192.168.10.31 \
        -e DSGX_REMOTE_ATTESTATION_PORT=2081 \
        -e DIMAGE_FLAG_BYPASS_IAS_VERIFY=ON \
        --gpus '"device=0"' \
        --restart always \
        -it -d --name ${CONTAINER_NAME} \
        -p 11000:11000 \
        -p 8086:8086 \
        ${DOCKER_IMAGE} \
        bash -c "./start.sh"
```

### 3.3 第二套 

#### docker-compose.yml 

```Yml
version: "3.7"

services:
    datanode-deeplearning-cpp-2:
      image: "nvxhub.nvxclouds.net:9443/ai/datanode-deeplearning-cpp-stable:0.1.1"
      container_name: datanode-deeplearning-cpp-2
      environment:
        - DIMAGE_HTTP_PORT=13000
        - DIMAGE_HTTPS_PORT=3080
        - DIMAGE_HOSTNAME=0.0.0.0
        - DSGX_REMOTE_ATTESTATION_PORT=2081
        - DSGX_REMOTE_ATTESTATION_HOST=192.168.10.31
        - DIMAGE_FLAG_BYPASS_IAS_VERIFY=ON
        - DeepLearningHost=192.168.10.100:50157
        - DeepLearningDevice=cuda:0
      command: ["./start.sh"]
      tty: true
      volumes:
        - ./data/outputs:/home/outputs
        - ./data/logs:/home/logs
        - /home/ltguo/nvxclouds/DataNode_AI/:/home/DataNode/
      networks:
        - localnet
      restart: always
      ports:
        - 13000:13000
        - 3080:3080
networks:
  localnet:
    external:
      name: localnet
```

#### docker-py-run.sh

```powershell
#!/bin/bash

CONTAINER_NAME=datanode-deeplearning-python-2
DOCKER_IMAGE=nvxhub.nvxclouds.net:9443/ai/datanode-deeplearning-python-stable:0.1.1
GRPC_SERVER_PORT=50157

docker container stop ${CONTAINER_NAME}
docker container rm ${CONTAINER_NAME}

docker run \
        -v /home/ltguo/.cache/torch/:/root/.cache/torch/ \
        -v /home/shouzhi/deploy/datanode12442/uploads:/data \
        -p ${GRPC_SERVER_PORT}:50055 \
        --gpus '"device=1"' \
        --restart always \
        -it -d --name ${CONTAINER_NAME} \
        ${DOCKER_IMAGE} \
        bash -c "./start.sh"
```

# 五、数据库操作

## 1、删除注册节点

- 第一步创建存储过程

  ```sql
  -- dNodeId需要删除的数据节点ID
  DROP PROCEDURE IF EXISTS rmDNodeById;
  delimiter $
  create procedure rmDNodeById(in dNodeId LONG)
  begin
     DELETE FROM analysis_method WHERE dataset_id in (SELECT id from datasets WHERE datanode_id = dNodeId);
     DELETE FROM dataset_usage_statistics WHERE data_set_id in (SELECT id from datasets WHERE datanode_id = dNodeId);
     DELETE FROM studies_datanodes_status WHERE datanode_id = dNodeId;
     DELETE FROM datasets WHERE datanode_id = dNodeId;
     DELETE FROM datanodes WHERE id = dNodeId; 
  end$
  ```

- 第二步执行存储过程

  ```sql
  -- 9表示要删除的数据节点ID
  CALL rmDNodeById(9);
  ```

- [存储过程使用实例教程](https://segmentfault.com/a/1190000039248897)



# 六、相关部署文档

## 1、[锘崴隐私计算平台SKADI部署](https://docs.google.com/document/d/1F8SL4K-KwyGebnMDiv-vt-J21uBheWe7UDnfKcy1xMU/edit#heading=h.d01ia694n9i3)

## 2、[图像研究部署文档 - Google 文档](https://docs.google.com/document/d/1Ylr8LUxb14S4G7jio6545fwBuBALqP6Xf_Ub6oito-g/edit#heading=h.ohkmvwaarrx6)

## 3、[(机密)锘崴平台版本控制文档 - Google 表格](https://docs.google.com/spreadsheets/d/1oI_iNMWThp9uyURwbPZ16OnDYKasoFuJHxQP2e-EXio/edit#gid=1670105393)



# 七、各个环境镜像版本

## 1、蚂蚁TEE

- local
  - nvxhub.nvxclouds.net:9443/sgx/img/nvxcloudstechdatanodestatis:jacktemp
- global
  - nvxhub.nvxclouds.net:9443/skadi/migrate/sgxserverbase:latest









