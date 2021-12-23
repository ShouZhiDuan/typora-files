# Medsua安全通信

## 1、Pairs

![image-20211125104620885](C:\Users\dev\AppData\Roaming\Typora\typora-user-images\image-20211125104620885.png)

![image-20211125115247915](C:\Users\dev\AppData\Roaming\Typora\typora-user-images\image-20211125115247915.png)

![image-20211125115255400](C:\Users\dev\AppData\Roaming\Typora\typora-user-images\image-20211125115255400.png)

- **decryptedPublicKey**、**decryptedPrivateKey**（前端生成）

- **encryptedPublicKey**、**encryptedPrivateKey**（后端->前端）

  - user_keys：用户秘钥对存储表

  - 示例图

    ![image-20211125111644963](C:\Users\dev\AppData\Roaming\Typora\typora-user-images\image-20211125111644963.png)

- **session_key**

  ![image-20211125141125414](C:\Users\dev\AppData\Roaming\Typora\typora-user-images\image-20211125141125414.png)



## 2、interface

- /keys/upload
- /keys/download
- /sessionKey/download
- /sessionKey/upload

```tex
io.novovivo.license.controller.KeyManagementController
```















