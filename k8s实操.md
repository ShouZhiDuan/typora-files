# k8s实操

## 一、k8s架构预览图

![image-20210811102903898](C:\Users\dev\AppData\Roaming\Typora\typora-user-images\image-20210811102903898.png)

![image-20210811103011624](C:\Users\dev\AppData\Roaming\Typora\typora-user-images\image-20210811103011624.png)

##  二、基础配置文件

### 1、Maps

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
```

`类比JSON`

```json
{
"apiVersion": "apps/v1",
"kind": "Deployment",
"metadata": {
         "name": "nginx-deployment",
         "labels": {
                    "app": "nginx"
                   }
        }
}
```

> metadata表示key，下面的内容表示value，该value中包含两个直接的key：name和labels
>
> name表示key，nginx-deployment表示value
>
> labels表示key，下面的表示value，这个值又是一个map
>
> app表示key，nginx表示value
>
> 相同层级的记得使用空间缩进，左对齐

### 2、Lists

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp
spec:
  containers:
  - name: myapp-container01
    image: busybox:1.28
  - name: myapp-container02
    image: busybox:1.28
```

`类比JSON`

```json
{
"apiVersion": "v1",
"kind": "Pod",
"metadata": {
           "name": "myapp",
           "labels": {
                       "app": "myapp"
                     }
         },
"spec": {
 "containers": [{
                 "name": "myapp-container01",
                 "image": "busybox:1.28",
                }, 
                {
                 "name": "myapp-container02",
                 "image": "busybox:1.28",
                }]
      }
}
```

### 3、YAML模板

`官网`：<https://kubernetes.io/docs/reference/>

```yaml
# yaml格式对于Pod的定义：
apiVersion: v1          #必写，版本号，比如v1
kind: Pod               #必写，类型，比如Pod
metadata:               #必写，元数据
  name: nginx           #必写，表示pod名称
  namespace: default    #表示pod名称属于的命名空间
  labels:
    app: nginx                  #自定义标签名字
spec:                           #必写，pod中容器的详细定义
  containers:                   #必写，pod中容器列表
  - name: nginx                 #必写，容器名称
    image: nginx                #必写，容器的镜像名称
    ports:
    - containerPort: 80         #表示容器的端口
```



## 三、Pod部署演示DEMO

### 1、定义dsz-nginx-pod.yaml

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
spec:
  containers:
  - name: nginx-container
    image: nginx
    ports:
    - containerPort: 80
```

### 2、启动nginx

```shell
kubectl apply -f dsz-nginx-pod.yaml
```

### 3、查看pod部署列表

```shell
kubectl get pod -o wide
```

显示如下
NAME                                    READY   STATUS    RESTARTS   AGE    IP            NODE        NOMINATED NODE   
dsz-nginx-pod                           1/1     Running   0          11m    **10.233.81.9**   k8s-node1   <none>           <none>

集群节点中的任意一个服务器：

```shell
curl 10.233.81.9:80
```

### 4、查看pod详情

```shell
kubectl describe pod dsz-nginx-pod
```

```powershell
Name:         dsz-nginx-pod
Namespace:    default
Priority:     0
Node:         k8s-node1/192.168.10.36
Start Time:   Wed, 11 Aug 2021 03:33:01 +0000
Labels:       app=dsz-nginx
Annotations:  cni.projectcalico.org/podIP: 10.233.81.9/32
              cni.projectcalico.org/podIPs: 10.233.81.9/32
Status:       Running
IP:           10.233.81.9
IPs:
  IP:  10.233.81.9
Containers:
  dsz-nginx-container:
    Container ID:   containerd://96debba8385182bb0dc77e28287d5db5870e4c65dcf23a56eefcd27bfbb3977b
    Image:          nginx
    Image ID:       docker.io/library/nginx@sha256:8f335768880da6baf72b70c701002b45f4932acae8d574dedfddaf967fc3ac90
    Port:           80/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Wed, 11 Aug 2021 03:33:27 +0000
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-5l86s (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  default-token-5l86s:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-5l86s
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                 node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  16m   default-scheduler  Successfully assigned default/dsz-nginx-pod to k8s-node1
  Normal  Pulling    16m   kubelet            Pulling image "nginx"
  Normal  Pulled     15m   kubelet            Successfully pulled image "nginx" in 24.596785434s
  Normal  Created    15m   kubelet            Created container dsz-nginx-container
  Normal  Started    15m   kubelet            Started container dsz-nginx-container
```

### 5、移除pod

```shell
kubectl delete -f dsz-nginx-pod.yaml
pod "dsz-nginx-pod" deleted 移除成功
```

