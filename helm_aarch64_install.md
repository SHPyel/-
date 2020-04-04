# helm安装
```
使用helm管理应用
```
# 下载helm
```
https://get.helm.sh/helm-v2.16.1-linux-arm64.tar.gz
cp helm tiller /usr/local/bin
helm version
tiller -version
```
# 创建helm-service-account.yaml文件
```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: tiller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: tiller
  namespace: kube-system
```
# helm初始化
```
kubectl apply -f helm-service-account.yaml
[root@k8s-master ~]# helm init --service-account tiller
```
# 验证
```
kubectl get pod -n kube-system -l app=helm

[root@master ~]# kubectl get pod -n kube-system -l app=helm
NAME                             READY   STATUS    RESTARTS   AGE
tiller-deploy-54f7455d59-28dcg   1/1     Running   0          39m
[root@master ~]#
```
# helm安装git插件
```
将helm release放在git上托管，使用gitlab做helm仓库
https://github.com/diwakar-s-maurya/helm-git
```
# 安装git插件
```
helm plugin install https://github.com/diwakar-s-maurya/helm-git

[root@master ~]# helm plugin list
NAME    	VERSION	DESCRIPTION
helm-git	1.0.0  	Let's you use private gitlab repositories easily
```
# 修改git查看配置
 ```
 vim /root/.helm/plugins/helm-git/getfile.sh
 git remote add origin http://114.116.215.218:3000/root/service-config-k8s.git
 ```
# 添加helm仓库
```
 helm repo add jiatui gitlab://root/service-config-k8s:master/helm-jtsys
输入账户密码
```
# 查看repo
```
[root@master ~]# helm repo list
NAME  	URL
stable	https://kubernetes-charts.storage.googleapis.com
local 	http://127.0.0.1:8879/charts
jiatui	gitlab://root/service-config-k8s:master/helm-jtsys
[root@master ~]#
```
# 更新仓库
```
当更新了git版本
helm repo update
```
# 安装应用
```
helm install --name architect jiatui/jtsys --debug --dry-run -f architect-values.yaml
helm upgrade architect jiatui/jtsys --debug  -f architect-values.yaml
```
# helm 命令
```
#初始化命令
helm init
##List releases of charts
helm list
#安装chart
helm install
#删除chart
helm delete --purge 
    --dry-run

#
```
