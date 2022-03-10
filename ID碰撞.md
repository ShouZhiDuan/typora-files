# 一、ID碰撞

## 1、创建研究参数

```json
{
    "analysisMethodId":6,
    "dataSources":[
        {
            "dataNodeId":1,
            "dataNodeName":"杭州节点13",
            "datasetIds":[
                14
            ],
            "filenames":[
                "breast_hetero_guest--1641283781381.csv"
            ],
            "matchRate":0,
            "matchedIds":0,
            "sampleSize":569,
            "sampleSizeArray":[
                null,
                null
            ]
        }
    ],
    "l1Token":"已废弃",
    "l2Token":"已废弃",
    "model":"",
    "page":1,
    "perPage":10,
    "server":"192.168.10.5",
    "studyInfo":{
        "description":"ID碰撞",
        "endDate":"01/04/2022",
        "name":"ID碰撞",
        "privacy":"PRIVATE",
        "startDate":"01/04/2022",
        "userId":2
    },
    "trainingStudyId":0,
    "userId":2
}
```

## 2、运行研究参数

```json
{
    "dataSourceAndStatusList":[
        {
            "datasetIds":[
                14
            ],
            "filenames":[
                "breast_hetero_guest--1641283781381.csv"
            ],
            "localDBIds":[
                54
            ],
            "sampleSize":569,
            "sampleSizeArray":[
                null,
                null
            ],
            "sessionId":"f59ad87073c9216047727affbe2c06255b65eb8fd14afcf8e7e439f96c9d8e6253af553b420658130504a8fc3f273934b81c03e29cd1eab867946804edd97c6a",
            "studyDataNodeStatus":{
                "primaryKey":{
                    "dataNode":{
                        "createdAt":1639469495000,
                        "description":"1",
                        "id":1,
                        "name":"杭州节点13",
                        "publicKey":"f59ad87073c9216047727affbe2c06255b65eb8fd14afcf8e7e439f96c9d8e6253af553b420658130504a8fc3f273934b81c03e29cd1eab867946804edd97c6a",
                        "publicKeyHeartBeat":"MIIBojANBgkqhkiG9w0BAQEFAAOCAY8AMIIBigKCAYEAuFwoCOgdHDjrTfhbJxFubmt2Ih7X5zJreGsocJCvsviZ60+vkXbF3VR3JkC6uXhyigjd10isWpz9cHW+RJxGUia3wf3+xXSMEH+vXmQXfVShMUe/vv0bSPHb22OVOH6EUmtqb4mbjFRJiFleD6ilo7fB5O1r+kR2WbS7tNo3cade+8UvdKqKYZ2wOV8gh6jP+1wfEgOCg8lFoUzyZE4+Ucskk1k+yceVyef00/mj6WQLjnlH1nGz5BunxZM32Ke7ALmTyfLtxgUoo7+WrafQjxwjppW/6TAguZwjS+nEJ1vCNwmojRD+aiHb/f1NiJ1OmMhn90IPggznOK9XNVH5DF6xw/hlZxS3bdxE792HwPu8DoOmlY/2cYFkaz3/q6uKCq1bfE/dpGVv7My8WZjVAR7RMFFBF8huX+oITejs4FH0AuZE72b8GDBLhnqIqZTCzEB7xiVX/ozOPUwKjC9d7zKHqKKHBZQi0469je6pQdSy0HhQyhlvS8eTf11CsYUZAgMBAAE=",
                        "status":"ALIVE",
                        "synced":false,
                        "updatedAt":1641284203000,
                        "url":"https://snode4.nvxclouds.app:11443",
                        "username":"null"
                    },
                    "study":{
                        "createdAt":1641284218000,
                        "datanodes":"[{\"dataNodeId\":1, \"dataNodeName\": \"杭州节点13\", \"matchedIds\": 0, \"filenames\": [\"breast_hetero_guest--1641283781381.csv\"], \"datasetNames\": [], \"sampleSizeArray\": [null,null], \"featuresParas\": null, \"params\": null, \"datasetIds\": [14], \"sampleSize\": 569, \"matchRate\": 0.0}]",
                        "description":"ID碰撞",
                        "endDate":"01/04/2022",
                        "id":113,
                        "method":"ID_Matching",
                        "name":"ID碰撞",
                        "parameters":"{\"mainStudyId\": null}",
                        "privacy":"PRIVATE",
                        "progress":0,
                        "server":"192.168.10.5",
                        "startDate":"1641284397022",
                        "status":"APPROVED",
                        "updatedAt":1641284397024,
                        "userId":2,
                        "username":"new_medusa"
                    }
                },
                "status":"NOT_STARTED"
            },
            "weightedArray":[

            ]
        }
    ],
    "dataSources":[
        {
            "dataNodeId":1,
            "dataNodeName":"杭州节点13",
            "datasetIds":[
                14
            ],
            "datasetNames":[

            ],
            "filenames":[
                "breast_hetero_guest--1641283781381.csv"
            ],
            "matchRate":0,
            "matchedIds":0,
            "sampleSize":569,
            "sampleSizeArray":[
                null,
                null
            ]
        }
    ],
    "encryptedSessionKey":"f1adddc6a7a62eb29e686b4e93b1bb2e8bde25e3a2f8687664f609e50585cb3962b986179820f563803e51e2e788460aca42dc644025e747102ede233e07174874d17ec1225cb53446889d9ef677c6e7105f0551737bba434dbf674e06aabe486fde6948699112912c1ddf5a5dcf48dae726c892da8e5cb2c25434bdedaad1fcc122b674a8075e51126704fafa0712cbf3f4f5430911da242a32a6c26d15d37b880fdd76f190f70ebfe0227a7c0098dcf40b597c18f4beaeaf28efd0b6b8ff931b412919dfd75da148d8bd739cc1fd3bcd9c6b316ee8a2b427a7e37ce0ff9e84e4fde897ffdb65c14f40d25743fdb738aced9dc85e7f26d2662b234b097dd91623edafc71b3df7f7af383a71d3b9454e8ce0803eaef936dbcb9822e29adfaf3944c266e7417150fa72642709aaff6b45b0dad64759fd2d9d7ab1e1228a31c23fcb42a9c8cb3fbfb5d4874018b8db41f9a60d49dfe37c5401a570895a764a79512290617b4a0b0267fb325cfd8d073b27f66fb6e38c774def254cca818397b87e",
    "l1Token":"eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJuZXdfbWVkdXNhIiwidXNlcklkIjoyLCJ0eXBlIjoiTDEiLCJpYXQiOjE2NDEyODQzOTYsImV4cCI6MTY0MTI5NTE5Nn0.hgO44k6T6-AYZMV67-Ut9BF7kAyMbZjASQTC4NlWWAs",
    "method":"ID_Matching",
    "page":1,
    "parameterDTO":{

    },
    "perPage":10,
    "port":8081,
    "server":"192.168.10.5",
    "studyId":113,
    "studyInfo":{
        "description":"ID碰撞",
        "endDate":"01/04/2022",
        "name":"ID碰撞",
        "privacy":"PRIVATE",
        "startDate":"01/04/2022",
        "userId":2
    },
    "userId":2
}
```



# 二、数据分箱

## 1、创建研究参数

```json
{
    "analysisMethodId":25,
    "dataSources":[
        {
            "dataNodeId":1,
            "dataNodeName":"杭州节点13",
            "datasetIds":[
                16
            ],
            "filenames":[
                "breast_hetero_guest--1641284791256.csv"
            ],
            "matchRate":0,
            "matchedIds":0,
            "sampleSize":569,
            "sampleSizeArray":[
                null,
                null
            ]
        }
    ],
    "l1Token":"已废弃",
    "l2Token":"已废弃",
    "model":"",
    "page":1,
    "perPage":10,
    "server":"192.168.10.5",
    "studyInfo":{
        "description":"数据分箱",
        "endDate":"01/04/2022",
        "name":"数据分箱",
        "privacy":"PRIVATE",
        "startDate":"01/04/2022",
        "userId":2
    },
    "trainingStudyId":0,
    "userId":2
}
```

## 2、运行研究参数

```json
{
    "dataSourceAndStatusList":[
        {
            "datasetIds":[
                16
            ],
            "filenames":[
                "breast_hetero_guest--1641284791256.csv"
            ],
            "localDBIds":[
                56
            ],
            "sampleSize":569,
            "sampleSizeArray":[
                null,
                null
            ],
            "sessionId":"f59ad87073c9216047727affbe2c06255b65eb8fd14afcf8e7e439f96c9d8e6253af553b420658130504a8fc3f273934b81c03e29cd1eab867946804edd97c6a",
            "studyDataNodeStatus":{
                "primaryKey":{
                    "dataNode":{
                        "createdAt":1639469495000,
                        "description":"1",
                        "id":1,
                        "name":"杭州节点13",
                        "publicKey":"f59ad87073c9216047727affbe2c06255b65eb8fd14afcf8e7e439f96c9d8e6253af553b420658130504a8fc3f273934b81c03e29cd1eab867946804edd97c6a",
                        "publicKeyHeartBeat":"MIIBojANBgkqhkiG9w0BAQEFAAOCAY8AMIIBigKCAYEAuFwoCOgdHDjrTfhbJxFubmt2Ih7X5zJreGsocJCvsviZ60+vkXbF3VR3JkC6uXhyigjd10isWpz9cHW+RJxGUia3wf3+xXSMEH+vXmQXfVShMUe/vv0bSPHb22OVOH6EUmtqb4mbjFRJiFleD6ilo7fB5O1r+kR2WbS7tNo3cade+8UvdKqKYZ2wOV8gh6jP+1wfEgOCg8lFoUzyZE4+Ucskk1k+yceVyef00/mj6WQLjnlH1nGz5BunxZM32Ke7ALmTyfLtxgUoo7+WrafQjxwjppW/6TAguZwjS+nEJ1vCNwmojRD+aiHb/f1NiJ1OmMhn90IPggznOK9XNVH5DF6xw/hlZxS3bdxE792HwPu8DoOmlY/2cYFkaz3/q6uKCq1bfE/dpGVv7My8WZjVAR7RMFFBF8huX+oITejs4FH0AuZE72b8GDBLhnqIqZTCzEB7xiVX/ozOPUwKjC9d7zKHqKKHBZQi0469je6pQdSy0HhQyhlvS8eTf11CsYUZAgMBAAE=",
                        "status":"ALIVE",
                        "synced":false,
                        "updatedAt":1641284203000,
                        "url":"https://snode4.nvxclouds.app:11443",
                        "username":"null"
                    },
                    "study":{
                        "createdAt":1641286119000,
                        "datanodes":"[{\"dataNodeId\":1, \"dataNodeName\": \"杭州节点13\", \"matchedIds\": 0, \"filenames\": [\"breast_hetero_guest--1641284791256.csv\"], \"datasetNames\": [], \"sampleSizeArray\": [null,null], \"featuresParas\": null, \"params\": null, \"datasetIds\": [16], \"sampleSize\": 569, \"matchRate\": 0.0}]",
                        "description":"数据分箱",
                        "endDate":"01/04/2022",
                        "id":115,
                        "method":"ID_Matching",
                        "name":"数据分箱",
                        "nextStudyId":114,
                        "parameters":"{\"mainStudyId\": 114}",
                        "privacy":"PRIVATE",
                        "progress":0,
                        "server":"192.168.10.5",
                        "startDate":"1641286119092",
                        "status":"APPROVED",
                        "updatedAt":1641286119094,
                        "userId":2,
                        "username":"new_medusa"
                    }
                },
                "status":"NOT_STARTED"
            },
            "weightedArray":[

            ]
        }
    ],
    "dataSources":[
        {
            "dataNodeId":1,
            "dataNodeName":"杭州节点13",
            "datasetIds":[
                16
            ],
            "datasetNames":[

            ],
            "filenames":[
                "breast_hetero_guest--1641284791256.csv"
            ],
            "matchRate":0,
            "matchedIds":0,
            "sampleSize":569,
            "sampleSizeArray":[
                null,
                null
            ]
        }
    ],
    "encryptedSessionKey":"39bc708636e18a7b0c3cf6ee9939757715f305f5329e4f6a5793f19847168e103e8587707165d6b524313c774a2f0e2ce68c8533f635b05730788c2c559eadfaae440b4186e1eddeca3281f174f65b04aaf1dd0311525db16b463192ebe2b4fbc102b722b9bc9e688b8b79ac9849f2ce94eddaf2d9a2521e46633780525c775964f03eacde14d8fc6d2f215acdd1d22ff3cf10b13251dc942ff58e8ec62f2124bf7affd740bbef299603e4f451c74736b24c2557a50dfcc6f23a8c2b43b61f044dc5a57a680980949c446a0588310e873340282e036abd1df0515239ca04363b56b466a0b57334a65d1cbf5a945820b7364b511c88d2568089aff887b61b66e36d791bdc5473ddb45da4f44d6c4cd425e1081fce597b25d0b123ebc5131a48eb871095aefa08ae7d5274fe54c3613ddbcddaba300f691a00208e66a655f6f098f1c4a9301253fa26c962dd3b752e01a5e50a0f0f12244392ec1109d0acbcfa83d58aaa9f46340140779c2819bb77cc9034c768a31c0913ca69fefced6e812bdb",
    "l1Token":"eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJuZXdfbWVkdXNhIiwidXNlcklkIjoyLCJ0eXBlIjoiTDEiLCJpYXQiOjE2NDEyODYxMTgsImV4cCI6MTY0MTI5NjkxOH0.ZRbFRWmKxrbOyPvbpycmgRsXrrg_WsIvpKpXcLIQgsc",
    "method":"ID_Matching",
    "page":1,
    "parameterDTO":{
        "mainStudyId":114
    },
    "perPage":10,
    "port":8081,
    "server":"192.168.10.5",
    "studyId":115,
    "studyInfo":{
        "description":"数据分箱",
        "endDate":"01/04/2022",
        "name":"数据分箱",
        "privacy":"PRIVATE",
        "startDate":"01/04/2022",
        "userId":2
    },
    "userId":2
}
```

# 三、异质逻辑回归

## 1、创建研究参数

```json
{
    "analysisMethodId":32,
    "dataSources":[
        {
            "dataNodeId":1,
            "dataNodeName":"杭州节点13",
            "datasetIds":[
                16
            ],
            "filenames":[
                "breast_hetero_guest--1641284791256.csv"
            ],
            "matchRate":0,
            "matchedIds":0,
            "sampleSize":569,
            "sampleSizeArray":[
                null,
                null
            ]
        }
    ],
    "l1Token":"已废弃",
    "l2Token":"已废弃",
    "model":"",
    "page":1,
    "perPage":10,
    "server":"192.168.10.5",
    "studyInfo":{
        "description":"异质逻辑回归",
        "endDate":"01/04/2022",
        "name":"异质逻辑回归",
        "privacy":"PRIVATE",
        "startDate":"01/04/2022",
        "userId":2
    },
    "trainingStudyId":0,
    "userId":2
}
```

## 2、运行研究参数

```json
{
    "dataSourceAndStatusList":[
        {
            "datasetIds":[
                16
            ],
            "filenames":[
                "breast_hetero_guest--1641284791256.csv"
            ],
            "localDBIds":[
                56
            ],
            "sampleSize":569,
            "sampleSizeArray":[
                null,
                null
            ],
            "sessionId":"f59ad87073c9216047727affbe2c06255b65eb8fd14afcf8e7e439f96c9d8e6253af553b420658130504a8fc3f273934b81c03e29cd1eab867946804edd97c6a",
            "studyDataNodeStatus":{
                "primaryKey":{
                    "dataNode":{
                        "createdAt":1639469495000,
                        "description":"1",
                        "id":1,
                        "name":"杭州节点13",
                        "publicKey":"f59ad87073c9216047727affbe2c06255b65eb8fd14afcf8e7e439f96c9d8e6253af553b420658130504a8fc3f273934b81c03e29cd1eab867946804edd97c6a",
                        "publicKeyHeartBeat":"MIIBojANBgkqhkiG9w0BAQEFAAOCAY8AMIIBigKCAYEAuFwoCOgdHDjrTfhbJxFubmt2Ih7X5zJreGsocJCvsviZ60+vkXbF3VR3JkC6uXhyigjd10isWpz9cHW+RJxGUia3wf3+xXSMEH+vXmQXfVShMUe/vv0bSPHb22OVOH6EUmtqb4mbjFRJiFleD6ilo7fB5O1r+kR2WbS7tNo3cade+8UvdKqKYZ2wOV8gh6jP+1wfEgOCg8lFoUzyZE4+Ucskk1k+yceVyef00/mj6WQLjnlH1nGz5BunxZM32Ke7ALmTyfLtxgUoo7+WrafQjxwjppW/6TAguZwjS+nEJ1vCNwmojRD+aiHb/f1NiJ1OmMhn90IPggznOK9XNVH5DF6xw/hlZxS3bdxE792HwPu8DoOmlY/2cYFkaz3/q6uKCq1bfE/dpGVv7My8WZjVAR7RMFFBF8huX+oITejs4FH0AuZE72b8GDBLhnqIqZTCzEB7xiVX/ozOPUwKjC9d7zKHqKKHBZQi0469je6pQdSy0HhQyhlvS8eTf11CsYUZAgMBAAE=",
                        "status":"ALIVE",
                        "synced":false,
                        "updatedAt":1641284203000,
                        "url":"https://snode4.nvxclouds.app:11443",
                        "username":"null"
                    },
                    "study":{
                        "createdAt":1641286665000,
                        "datanodes":"[{\"dataNodeId\":1, \"dataNodeName\": \"杭州节点13\", \"matchedIds\": 0, \"filenames\": [\"breast_hetero_guest--1641284791256.csv\"], \"datasetNames\": [], \"sampleSizeArray\": [null,null], \"featuresParas\": null, \"params\": null, \"datasetIds\": [16], \"sampleSize\": 569, \"matchRate\": 0.0}]",
                        "description":"异质逻辑回归",
                        "endDate":"01/04/2022",
                        "id":118,
                        "method":"ID_Matching",
                        "name":"异质逻辑回归",
                        "nextStudyId":116,
                        "parameters":"{\"mainStudyId\": 116}",
                        "privacy":"PRIVATE",
                        "progress":0,
                        "server":"192.168.10.5",
                        "startDate":"1641286665346",
                        "status":"APPROVED",
                        "updatedAt":1641286665356,
                        "userId":2,
                        "username":"new_medusa"
                    }
                },
                "status":"NOT_STARTED"
            },
            "weightedArray":[

            ]
        }
    ],
    "dataSources":[
        {
            "dataNodeId":1,
            "dataNodeName":"杭州节点13",
            "datasetIds":[
                16
            ],
            "datasetNames":[

            ],
            "filenames":[
                "breast_hetero_guest--1641284791256.csv"
            ],
            "matchRate":0,
            "matchedIds":0,
            "sampleSize":569,
            "sampleSizeArray":[
                null,
                null
            ]
        }
    ],
  "encryptedSessionKey":"6ea0e4a24b785e64fb456e80089152c977ac191448ed2dc4e7e6daa54dddfbbfed48711bb0be25212d2b57328a0b1706922a89c6007c783b143d83ef6c3e816061f4fe5e4f56b4568ee3d4d1395c6d0d233c0e73978c8e976bc0a408305daea89223a2cc462bcfa9f54cf036db9defddce6a739be1c6d62d74da54cd5304ef303ed77f30ebbff0aa4e57c285ffe83255aca966a0dce31aab13ba32e4aa669e76d52e9b06dd7f6710c570695389f36a8cfa5cd080057a4bd7669a7260e2cc8395ab8be3d316176f29b1e6d999a0448b23b58784aed358ff166a61cacdf8c7b5e302529a731bca12d218f02d7c9279a926067a8cb88238f0eb059bade237bc51ab1826289c1a69057c592fe9dbbaf012828890dbd28861dc08f4494581245989f476802344396c55028e4574f21a75a73c00b6f1dc51df4596f9ea99ca157dc467e772814089f2eeebba499e3a77b7bd61a4a16b56ffeb304520d5637f57dbca85205891201bcd60695b787998d99967bea29bbdb91a3c139490f6ec35905e0262",
    "l1Token":"已废弃",
    "method":"ID_Matching",
    "page":1,
    "parameterDTO":{
        "mainStudyId":116
    },
    "perPage":10,
    "port":8081,
    "server":"192.168.10.5",
    "studyId":118,
    "studyInfo":{
        "description":"异质逻辑回归",
        "endDate":"01/04/2022",
        "name":"异质逻辑回归",
        "privacy":"PRIVATE",
        "startDate":"01/04/2022",
        "userId":2
    },
    "userId":2
}
```









































