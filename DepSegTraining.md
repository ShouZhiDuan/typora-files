# DepSegTraining

## 一、创建研究

### 1、获取研究方法（/v2/methods/18）

```json
{
    "code":0,
    "message":"",
    "data":{
        "id":18,
        "blocked":false,
        "description":"Convolutional neural network is a deep learning method specially developed for image classification and recognition developed on the basis of multi-layer neural network",
        "descriptionZh":"卷积神经网络是在多层神经网络的基础上发展起来的针对图像分类和识别而特别设计的一种深度学习方法",
        "name":"Federated CNN Framework Training Study",
        "nameDB":"DeepSegTraining",
        "nameZh":"分布式卷积神经网络框架训练",
        "createdTime":"2021-09-16",
        "type":2
    }
}
```

### 2、获取计算节点列表（/v2/sgx/get）

```json
{
    "code":0,
    "message":"",
    "data":{
        "page":1,
        "perPage":10,
        "pages":1,
        "total":1,
        "list":[
            {
                "id":1,
                "ip":"192.168.10.30",
                "createdAt":"2021-09-01 16:30:31",
                "updatedAt":"2021-09-24 20:13:13",
                "name":"杭州计算节点",
                "enclaveId":"a8196a010cd13aa2e42415d2651676a112798c606df155227d46b3dfcb1acb01",
                "status":"Service Available, Verification Succeed"
            }
        ],
        "end":true
    }
}
```

### 3、模型（前端维护）

- UNet
- CNNMnist

### 4、数据集列表（/dataset/list/method）

```json
[
    {
        "dataSetId":54,
        "filename":"brain-mri-2-0--1629881955776",
        "dataNodeName":"杭州节点13",
        "dataSetName":"cnntest1",
        "description":"cnntest1",
        "status":"ALIVE",
        "methods":"DeepSegTraining",
        "dataSetStatus":"ALIVE",
        "institution":null,
        "dataNodeId":3,
        "attributeNames":[
            "null"
        ],
        "valueCategorical":[
            "null"
        ],
        "isCategorical":[
            ""
        ],
        "numCategorical":[
            "null"
        ],
        "sampleSize":54,
        "modelList":"CNNMnist,UNet",
        "imageDimension":null,
        "sampleSizeTraining":45,
        "sampleSizeEvaluation":9,
        "databaseKey":"brain-mri-kaggle"
    }
]
```

### 5、提交研究

- 获取L2 Token（token/generateL2Token）

```json
{
    "username":"medusa_admin",
    "l2Token":"eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJtZWR1c2FfYWRtaW4iLCJ1c2VySWQiOjEsInR5cGUiOiJMMiIsIm1ldGhvZCI6IkRlZXBTZWdUcmFpbmluZyIsImlhdCI6MTYzMjcwODY5NCwiZXhwIjoxNjMyNzA4OTk0fQ.rcgS_z0zw9bmpDzl-Z5VcNu9jK1IIR7kIozVFFhDQJ0",
    "l1Token":"eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJtZWR1c2FfYWRtaW4iLCJ1c2VySWQiOjEsInR5cGUiOiJMMSIsImlhdCI6MTYzMjcwODY5NCwiZXhwIjoxNjMyNzE5NDk0fQ.KPHP6ke3pGxf-nNe5BliRQqT07OCBf52BOjvzmWGAAk"
}
```

- 保存研究（/studies/inject）

```json
{
    "analysisMethodId":18,
    "compositeStudyId":null,
    "parameters":null,
    "server":"192.168.10.30",
    "model":"CNNMnist",
    "dataSources":[
        {
            "dataNodeId":3,
            "datasetIds":[
                54
            ],
            "filenames":[
                "brain-mri-2-0--1629881955776"
            ],
            "dataNodeName":"杭州节点13",
            "sampleSize":54,
            "sampleSizeArray":[
                45,
                9
            ]
        }
    ],
    "userId":1,
    "studyInfo":{
        "name":"dep测试名称2",
        "privacy":"Public",
        "startDate":"09/27/2021",
        "endDate":"09/27/2021",
        "description":"dep测试描述",
        "userId":1
    },
    "trainingStudyId":0,
    "l1Token":"eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJtZWR1c2FfYWRtaW4iLCJ1c2VySWQiOjEsInR5cGUiOiJMMSIsImlhdCI6MTYzMjcwOTAxMCwiZXhwIjoxNjMyNzE5ODEwfQ.LK8KGf8zeisyPAVGw4lRiUCPdIXTAbchmUZga6E7GxU",
    "l2Token":"eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJtZWR1c2FfYWRtaW4iLCJ1c2VySWQiOjEsInR5cGUiOiJMMiIsIm1ldGhvZCI6IkRlZXBTZWdUcmFpbmluZyIsImlhdCI6MTYzMjcwOTAxMCwiZXhwIjoxNjMyNzA5MzEwfQ.e2ujL0vvTEDaeRQZrs43EVfzopiZOWXvdb2pqoiIdq4"
}
```

- 下载秘钥（/keys/download）需要前端一起看下这个key是什么作用？

```json
{
    "userId":1,
    "encryptedPublicKey":"634c1d535bdbed796feff43010f2346a44b7ac94fd40c8996b4e4fccf34d223b9284d4593d78992205dfd919132c6748b85db82eb2e4c44781ee1b0d70ccc3d058079a07b1a79b7c135c5c89410",
    "encryptedPrivateKey":"d48f4a7596c1a4b4f982feb1495a9a6a1bc4a886c6e8270d2700e8b03e086bfc3a0d2db70354ab6df900ad66ac645d4b5b7fe6c3ec7447808e3542822f6ff501553393a3cd937e0286cb55f1747c12c77df74c5442b9d4f2553638990d6c61df43103e40cf0108224972833b09bb92645c4871c0e"
}
```

- 获取研究详情（/studies/getInfo）

```json
{
    "userId":1,
    "studyId":60581,
    "status":"Approved",
    "isPipelineStudy":false,
    "studyRequestInfo":{
        "studyInfo":{
            "userId":1,
            "name":"dep测试名称2",
            "startDate":"09/27/2021",
            "endDate":"09/27/2021",
            "description":"dep测试描述",
            "privacy":"Public"
        },
        "dataSources":[
            {
                "dataNodeId":3,
                "dataNodeName":"杭州节点13",
                "filenames":[
                    "brain-mri-2-0--1629881955776"
                ],
                "datasetNames":[

                ],
                "datasetIds":[
                    54
                ],
                "sampleSize":54,
                "sampleSizeArray":[
                    45,
                    9
                ],
                "matchedIds":0,
                "matchRate":0,
                "featuresParas":null,
                "params":null
            }
        ],
        "server":"192.168.10.30",
        "model":"CNNMnist",
        "analysisMethodId":18,
        "trainingStudyId":null,
        "idMatchingStudyId":null,
        "dataBinningStudyId":null,
        "featureXs":null,
        "trainingModel":null,
        "models":null
    }
}
```

- 获取计算列表（/v2/sgx/get）

```json
{
    "code":0,
    "message":"",
    "data":{
        "page":1,
        "perPage":10,
        "pages":1,
        "total":1,
        "list":[
            {
                "id":1,
                "ip":"192.168.10.30",
                "createdAt":"2021-09-01 16:30:31",
                "updatedAt":"2021-09-24 20:13:13",
                "name":"杭州计算节点",
                "enclaveId":"a8196a010cd13aa2e42415d2651676a112798c606df155227d46b3dfcb1acb01",
                "status":"Service Available, Verification Succeed"
            }
        ],
        "end":true
    }
}
```

- /deepseg/getMetrics 需要前端一起看下什么作用？

```tex
loss,accuracy
```











