## 概念

### Pod
- pod 是可以在k8s中创建和管理的最小可部署的计算单元
- pod中包含一组容器
- Pod 天生地为其成员容器提供了两种共享资源：网络和存储
- pod由工作负载创建,工作负载通过控制器使用pod Template来创建和管理.
```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: hello
spec:
  template:
    # 这里是 Pod 模板
    spec:
      containers:
      - name: hello
        image: busybox:1.28
        command: ['sh', '-c', 'echo "Hello, Kubernetes!" && sleep 3600']
      restartPolicy: OnFailure
    # 以上为 Pod 模板
```
#### Pod模版



### 安装minikube

```shell
// windows
winget install Kubernetes.minikube

// linux
curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64

```

### 启动cluster
```
minikube start
```

### 查询 集群信息
```
kubectl get po -A
或者
minikube kubectl -- get po -A


minikube dashboard

```

