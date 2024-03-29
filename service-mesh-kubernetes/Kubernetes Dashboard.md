# Kubernetes Dashboard

## 概述

Kubernetes Dashboard 是 Kubernetes 集群的 Web UI，用于管理集群。

## 安装

GitHub 地址：[Kubernetes Dashboard](https://github.com/kubernetes/dashboard)

下载配置文件

```bash
wget https://raw.githubusercontent.com/kubernetes/dashboard/v1.10.1/src/deploy/recommended/kubernetes-dashboard.yaml
```

修改配置如下

```yaml
# 省略部分代码...

# ------------------- Dashboard Deployment ------------------- #

kind: Deployment
apiVersion: apps/v1
metadata:
  labels:
    k8s-app: kubernetes-dashboard
  name: kubernetes-dashboard
  namespace: kube-system
spec:
  replicas: 1
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      k8s-app: kubernetes-dashboard
  template:
    metadata:
      labels:
        k8s-app: kubernetes-dashboard
    spec:
      containers:
      - name: kubernetes-dashboard
        # 修改镜像地址为阿里云
        image: registry.aliyuncs.com/google_containers/kubernetes-dashboard-amd64:v1.10.1
        ports:
        - containerPort: 8443
          protocol: TCP
        args:
          - --auto-generate-certificates
        volumeMounts:
        - name: kubernetes-dashboard-certs
          mountPath: /certs
        - mountPath: /tmp
          name: tmp-volume
        livenessProbe:
          httpGet:
            scheme: HTTPS
            path: /
            port: 8443
          initialDelaySeconds: 30
          timeoutSeconds: 30
      volumes:
      - name: kubernetes-dashboard-certs
        secret:
          secretName: kubernetes-dashboard-certs
      - name: tmp-volume
        emptyDir: {}
      serviceAccountName: kubernetes-dashboard
      tolerations:
      - key: node-role.kubernetes.io/master
        effect: NoSchedule

---
# ------------------- Dashboard Service ------------------- #

kind: Service
apiVersion: v1
metadata:
  labels:
    k8s-app: kubernetes-dashboard
  name: kubernetes-dashboard
  namespace: kube-system
spec:
  # 修改类型为 NodePort 访问
  type: NodePort
  ports:
    - port: 443
      targetPort: 8443
      # 设置端口号为 30001
      nodePort: 30001
  selector:
    k8s-app: kubernetes-dashboard
```

部署到集群

```bash
# 部署
kubectl create -f kubernetes-dashboard.yaml

# 查看
kubectl -n kube-system get pods
kubectl -n kube-system get service kubernetes-dashboard
kubectl -n kube-system describe service kubernetes-dashboard
```

## 访问

需要使用 NodeIP:30001 访问 Dashboard，因为证书原因除火狐浏览器外其它浏览器无法直接打开页面

Chrome 浏览器显示如下

![img](./img/20190610071425.png)

Firefox 浏览器显示如下

![img](./img/20190610071443.png)

点击 **接受风险并继续** 即可显示欢迎界面

![img](./img/20190610072653.png)

## 登录

我们采用 Token 方式登录

- 创建登录账号，创建一个名为 `dashboard-adminuser.yaml` 的配置文件

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kube-system
```

```bash
kubectl create -f dashboard-adminuser.yaml
```

- 打印 Token 信息

```bash
kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep admin-user | awk '{print $1}')

# 输出如下
Name:         admin-user-token-86cz9
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name: admin-user
              kubernetes.io/service-account.uid: 3902d3d4-8b13-11e9-8089-000c29d49c77

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1025 bytes
namespace:  11 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJhZG1pbi11c2VyLXRva2VuLTg2Y3o5Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQubmFtZSI6ImFkbWluLXVzZXIiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC51aWQiOiIzOTAyZDNkNC04YjEzLTExZTktODA4OS0wMDBjMjlkNDljNzciLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6a3ViZS1zeXN0ZW06YWRtaW4tdXNlciJ9.pA44wyarsahOwqH7X7RVlcdB1k3_j-L3gwOYlTQ4_Lu5ZmfXDFlhqN-Q1tdryJes_V1Nj_utocnXBAxsGzOGaVR4Te4oli3htSepI9MrggQAyeC3C0_QANXGCE6V5L6B5tGZ6tDsY92VDnlvz2N6OrHaH2IJJd2DlxzYvAPvfAFuPeHWuPeVxUisMfXeW42S7US6skZwbZ06JrPYAFxHjqv3zoxRxI8-bmekltvOamsrL0pAXvIUzaowgbjiQb2NgeLAw9O6qfYcz5DAi2C-7G_yAcve6pgnWcIGhVpKoim9DfJUhe1SVx4H4X5Na6GVaaD6FdUIb7UOgsO1FVpTPw
```

- 将 Token 输入浏览器，成功登陆后效果如下

![img](./img/20190610081109.png)