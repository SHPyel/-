<h1>说明</h1>
<p>使用kubeadm安装kubernetes集群</p>
<p>系统初始化</p>
```
- hosts : all
  remote_user : root
  tasks :
  - name : show hostname
    shell : hostname
  - name : show ip
    command : ip a
  - hostname : name=qc-app-{{ ansible_default_ipv4.address.split('.')[-2] }}-{{ ansible_default_ipv4.address.split('.')[-1] }}


<p>docker-ce安装</p>

 step 1: 安装必要的一些系统工具
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
 Step 2: 添加软件源信息
sudo yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
Step 3: 更新并安装Docker-CE
sudo yum makecache fast
安装指定版本的Docker-CE:
Step 1: 查找Docker-CE的版本:
yum list docker-ce.x86_64 --showduplicates | sort -r
Step2: 安装指定版本的Docker-CE:
sudo yum -y install docker-ce-[VERSION]
yum install -y docker-ce-19.03.11-3.el7

#/bin/bash
url=mirrorgooglecontainers
version=v1.15.0
#images=(`kubeadm config images list --kubernetes-version=$version|awk -F '/' '{print $2}'`)
images=(kube-apiserver-arm64 kube-scheduler-arm64 kube-proxy-arm64 kube-controller-manager-arm64)
#docker pull mirrorgooglecontainers/kube-proxy-arm64:v1.15.0
for imagename in ${images[@]} ; do
  docker pull $url/$imagename:$version
  docker tag $url/$imagename:$version k8s.gcr.io/$imagename:$version
  docker rmi -f $url/$imagename:$version
done

```
docker pull gcr.azk8s.cn/google_containers/coredns:1.3.1
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/kube-apiserver-arm64:v1.15.0
  
