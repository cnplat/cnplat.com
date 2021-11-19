# 使用kubeadm安装Kubernetes

本文相关命令基于`Debian 10.2`编写

## 安装Kubeadm

[官方安装文档](https://kubernetes.io/zh/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)

### 禁用交换分区

```shell
swapoff -a;
sed -ri 's/.*swap.*/#&/' /etc/fstab;
```

### 允许 iptables 检查桥接流量

```shell
modprobe br_netfilter;
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
br_netfilter
EOF

cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
EOF
sysctl --system;

echo '1' > /proc/sys/net/ipv4/ip_forward
echo '1' > /proc/sys/net/bridge/bridge-nf-call-iptables
```

### 安装 Docker

```shell
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
cat <<EOF | sudo tee /etc/docker/daemon.json
{
  "registry-mirrors": ["https://n3kgoynn.mirror.aliyuncs.com"],
  "exec-opts": ["native.cgroupdriver=systemd"]
}
EOF
systemctl daemon-reload && systemctl enable docker && systemctl restart docker
```

### 安装 ipvs、kubeadm、kubelet 和 kubectl

```shell
apt-get update;
apt-get install -y apt-transport-https ca-certificates curl gnupg2;
apt-get install -y ipvsadm ipset sysstat conntrack libseccomp2;
apt-get update && apt-get install -y apt-transport-https
curl https://mirrors.aliyun.com/kubernetes/apt/doc/apt-key.gpg | apt-key add - 
cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://mirrors.aliyun.com/kubernetes/apt/ kubernetes-xenial main
EOF
apt-get update && apt-get install -y kubelet kubeadm kubectl
apt-mark hold kubelet kubeadm kubectl
systemctl enable kubelet && systemctl start kubelet
```

## 使用 kubeadm 创建集群

```
kubeadm init --apiserver-advertise-address=0.0.0.0 \
--apiserver-cert-extra-sans=127.0.0.1 \
--image-repository=registry.aliyuncs.com/google_containers \
--ignore-preflight-errors=all \
--service-cidr=10.18.0.0/16 \
--pod-network-cidr=10.244.0.0/16

# setup kubectl
mkdir -p $HOME/.kube && cp -i /etc/kubernetes/admin.conf $HOME/.kube/config

# del master taint
kubectl taint nodes --all node-role.kubernetes.io/master-

# install Flannel
kubectl apply -f https://raw.fastgit.org/flannel-io/flannel/master/Documentation/kube-flannel.yml
```