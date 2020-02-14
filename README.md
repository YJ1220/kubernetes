# kubernetes

dockerfile国内可访问的镜像仓库
Docker Hub镜像仓库: https://hub.docker.com/
阿里云镜像仓库：https://cr.console.aliyun.com
RedHat镜像仓库：https://access.redhat.com/containers

国内无法访问的镜像仓库
google镜像仓库：https://console.cloud.google.com/gcr/images/google-containers/GLOBAL
coreos镜像仓库：https://quay.io/repository/

临时解决方法：
	• 在部署kubernetes集群时，需要从google镜像仓库获取kubernetes组件相关镜像，以及从coreos仓库获取flannel网络插件等镜像，但dockerhub或阿里云仓库基本能够搜索到他人上传的包含这2个仓库中的镜像，我们只需要拉取到本地以后改回默认的镜像tag即可。
	• 另外dockerhub对google仓库做了镜像mirror，因此可以在google镜像名称前加mirrorgooglecontainers，即可直接在dockerhub拉取google镜像，拉取到本地后同样改回google仓库默认tag即可。

例如拉取kube-apiserver-amd64:v1.13.2镜像使用如下格式即可：

# google镜像默认格式k8s.gcr.io/kube-apiserver:v1.13.2
# dockerhub拉取镜像
$ docker pull mirrorgooglecontainers/kube-apiserver-amd64:v1.13.2
# 修改tag
$ docker tag mirrorgooglecontainers/kube-apiserver-amd64:v1.13.2 k8s.gcr.io/kube-apiserver-amd64:v1.13.2
# 成功拉取的镜像
$ docker images | grep kube-apiservermirrorgooglecontainers/kube-apiserver-amd64   v1.13.2             177db4b8e93a        2 months ago        181MBk8s.gcr.io/kube-apiserver-amd64               v1.13.2             177db4b8e93a        2 months ago        181MB

从阿里云镜像仓库搜索并拉取

# 从阿里云镜像仓库拉取镜像
$ docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/kube-apiserver-amd64:v1.13.0
# 修改镜像tag
$ docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/kube-apiserver-amd64:v1.13.0  k8s.gcr.io/kube-apiserver:v1.13.0

# 通过阿里云构建
  FROM k8s.gcr.io/kube-proxy-amd64:v1.11.1
  FROM k8s.gcr.io/coredns:1.6.2
