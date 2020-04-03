<h1>说明</h1>
<p>使用kubeadm安装kubernetes集群</p>

```
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
  
