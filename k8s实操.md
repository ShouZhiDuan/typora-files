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

## 四、k8s常用组件

### 1、ReplicationController(RC)

> ReplicationController定义了一个期望的场景，即声明某种Pod的副本数量在任意时刻都符合某个预期值，所以RC的定义包含以下几个部分：
>
> - Pod期待的副本数（replicas）
> - 用于筛选目标Pod的Label Selector
> - 当Pod的副本数量小于预期数量时，用于创建新Pod的Pod模板（template）
>
> 也就是说通过RC实现了集群中Pod的高可用，减少了传统IT环境中手工运维的工作。

- 创建dsz-replication-controller.yaml

```yaml
apiVersion: v1
kind: ReplicationController
metadata:
  name: rc-nginx
spec:
  replicas: 3 #指定高可用副本nginx数量
  selector:
    app: rc-nginx
  template:
    metadata:
      name: nginx
      labels:
        app: rc-nginx #标签用于扩缩容标记
    spec:
      containers:
      - name: rc-nginx
        image: nginx #通过改变RC里Pod模板中的镜像版本，可以实现Pod的升级功能
        ports:
        - containerPort: 80
```

- 启动 会启动3个nginx

  > Controller Manager（见本文最前面的架构图）能够及时发现，然后根据RC的定义，创建一个新的Pod。

```
kubectl apply -f dsz-replication-controller.yaml
```

- 手动扩缩容

```shell
kubectl scale rc rc-nginx --replicas=2
```

- 模拟挂掉一个，会重新帮我们重启一个

```shell
kubectl delete pods rc-nginx-qp7pf
kubectl get pods 
发现还是原来定义replicas的数量，只是上面删除的pod不在了重新启动了一个
```

### 2、ReplicaSet(RS)

> 在Kubernetes v1.2时，RC就升级成了另外一个概念：Replica Set，官方解释为“下一代RC”
>
> ReplicaSet和RC没有本质的区别，kubectl中绝大部分作用于RC的命令同样适用于RS
>
> RS与RC唯一的区别是：RS支持基于集合的Label Selector（Set-based selector），而RC只支持基于等式的Label Selector（equality-based selector），这使得Replica Set的功能更强

`注意`

> 一般情况下，我们很少单独使用Replica Set，它主要是被Deployment这个更高的资源对象所使用，从而形成一整套Pod创建、删除、更新的编排机制。当我们使用Deployment时，无须关心它是如何创建和维护Replica Set的，这一切都是自动发生的。同时，无需担心跟其他机制的不兼容问题（比如ReplicaSet不支持rolling-update但Deployment支持）。

```yaml
apiVersion: extensions/v1beta1
kind: ReplicaSet
metadata:
  name: frontend
spec:
  matchLabels: 
    tier: frontend
  matchExpressions: 
    - {key:tier,operator: In,values: [frontend]}
  template:
  ...
```







