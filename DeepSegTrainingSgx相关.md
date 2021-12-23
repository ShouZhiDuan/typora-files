# DeepSegTraining

## 一、start 研究

### 1、参数列表

![image-20210927105708494](C:\Users\dev\AppData\Roaming\Typora\typora-user-images\image-20210927105708494.png)

#### 1.1、评价标准（/deepseg/getMetrics）

```json
--request param
{"model":"CNNMnist","userId":1,"l1Token":"eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJtZWR1c2FfYWRtaW4iLCJ1c2VySWQiOjEsInR5cGUiOiJMMSIsImlhdCI6MTYzMjcwOTAxMCwiZXhwIjoxNjMyNzE5ODEwfQ.LK8KGf8zeisyPAVGw4lRiUCPdIXTAbchmUZga6E7GxU"}
--response body
loss,accuracy
```

#### 1.2、模型 （/studies/getInfo）

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
        "model":"CNNMnist",//以上第一个参数用到
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

#### 1.3、训练模型、训练和评估数据

- 应该是由前端维护





















