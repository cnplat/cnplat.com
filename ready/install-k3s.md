# 安装轻量级K8S之K3S

本文相关命令基于`Debian 10.2`编写

## 准备离线文件

1. 访问 [https://get.k3s.io/](https://get.k3s.io/)，保存为 `install.sh`，放入 `shell` 所在目录的 `offline` 文件件

2. 前往[https://github.com/k3s-io/k3s/releases](https://github.com/k3s-io/k3s/releases)，下载 `您要安装版本` 的离线文件 `k3s` 和 `k3s-airgap-images-amd64.tar`，放入 `shell` 所在目录的 `offline` 文件件


```
├── master.sh
├── masterha.sh
├── node.sh
└── offline
    ├── install.sh                   # K3S官方安装shell
    ├── k3s                          # k3s二进制文件
    └── k3s-airgap-images-amd64.tar  # k3s离线镜像包
```

## Master节点

[master.sh](https://raw.githubusercontent.com/cnplat/architect/main/k3s/master.sh)

```shell
#!/bin/bash
set -e

if ! type curl >/dev/null 2>&1; then
    apt update -y && apt install curl -y && apt autoremove -y
fi

# 安装 docker
curl -fsSL https://get.docker.com | bash
cat <<EOF | tee /etc/docker/daemon.json
{
  "registry-mirrors": ["https://n3kgoynn.mirror.aliyuncs.com"]
}
EOF
systemctl daemon-reload && systemctl enable docker && systemctl restart docker

# 准备k3s离线数据
chmod -R 777 ./offline && chmod +x ./offline/k3s
mkdir -p /var/lib/rancher/k3s/agent/images/
cp ./offline/k3s-airgap-images-amd64.tar /var/lib/rancher/k3s/agent/images/
cp ./offline/k3s /usr/local/bin

# 准备k3s相关环境变量配置
localip=`ifconfig eth0 | awk '/inet/ {print $2}' | cut -f2 -d ":" |awk 'NR==1 {print $1}'`
nowip=`curl -s www.123cha.com |grep -o "[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}" |head -n 1`
export INSTALL_K3S_SKIP_DOWNLOAD=true
export K3S_NODE_IP=${localip}
export K3S_EXTERNAL_IP=${nowip}
# 这里禁用traefik, 我们要自己安装
export INSTALL_K3S_EXEC="server --docker --cluster-init --write-kubeconfig ~/.kube/config --write-kubeconfig-mode 666 --tls-san $K3S_NODE_IP --node-ip $K3S_NODE_IP --node-external-ip $K3S_EXTERNAL_IP --kube-apiserver-arg service-node-port-range=1-65000 --kube-apiserver-arg advertise-address=$K3S_NODE_IP --kube-apiserver-arg external-hostname=$K3S_NODE_IP --disable traefik"

# 安装k3s
bash ./offline/install.sh

# 输出node
echo "First execute the following commands on other nodes."
echo -e "\nexport K3S_TOKEN=$(cat /var/lib/rancher/k3s/server/node-token)\nexport K3S_URL=https://$K3S_NODE_IP:6443\n"
echo -e "HA Master Node run 'bash masterha.sh'."
echo -e "Worker Node run 'bash node.sh'."
```

## HA Mater节点

先执行master节点安装完成输出的 `K3S_TOKEN` `K3S_URL` 信息

```shell
export K3S_TOKEN=xxx
export K3S_URL=https://10.0.0.10:6443
```

[masterha.sh](https://raw.githubusercontent.com/cnplat/architect/main/k3s/masterha.sh)

```shell
set -e

if ! type curl >/dev/null 2>&1; then
    apt update -y && apt install curl -y && apt autoremove -y
fi

# 安装 docker
curl -fsSL https://get.docker.com | bash
cat <<EOF | tee /etc/docker/daemon.json
{
  "registry-mirrors": ["https://n3kgoynn.mirror.aliyuncs.com"]
}
EOF
systemctl daemon-reload && systemctl enable docker && systemctl restart docker

# 准备k3s离线数据
chmod -R 777 ./offline && chmod +x ./offline/k3s
mkdir -p /var/lib/rancher/k3s/agent/images/
cp ./offline/k3s-airgap-images-amd64.tar /var/lib/rancher/k3s/agent/images/
cp ./offline/k3s /usr/local/bin

# 准备k3s相关环境变量配置
localip=`ifconfig eth0 | awk '/inet/ {print $2}' | cut -f2 -d ":" |awk 'NR==1 {print $1}'`
nowip=`curl -s www.123cha.com |grep -o "[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}" |head -n 1`
export INSTALL_K3S_SKIP_DOWNLOAD=true
export K3S_NODE_IP=${localip}
export K3S_EXTERNAL_IP=${nowip}
export INSTALL_K3S_EXEC="server --docker --token $K3S_TOKEN --server $K3S_URL --node-ip $K3S_NODE_IP --node-external-ip $K3S_EXTERNAL_IP"

# 安装k3s
bash ./offline/install.sh
```

## Node节点

先执行master节点安装完成输出的 `K3S_TOKEN` `K3S_URL` 信息

```shell
export K3S_TOKEN=xxx
export K3S_URL=https://10.0.0.10:6443
```

[node.sh](https://raw.githubusercontent.com/cnplat/architect/main/k3s/node.sh)

```shell
#!/bin/bash
set -e

if ! type curl >/dev/null 2>&1; then
    apt update -y && apt install curl -y && apt autoremove -y
fi

# 安装 docker
curl -fsSL https://get.docker.com | bash
cat <<EOF | tee /etc/docker/daemon.json
{
  "registry-mirrors": ["https://n3kgoynn.mirror.aliyuncs.com"]
}
EOF
systemctl daemon-reload && systemctl enable docker && systemctl restart docker

# 准备k3s离线数据
chmod -R 777 ./offline && chmod +x ./offline/k3s
mkdir -p /var/lib/rancher/k3s/agent/images/
cp ./offline/k3s-airgap-images-amd64.tar /var/lib/rancher/k3s/agent/images/
cp ./offline/k3s /usr/local/bin

# 准备k3s相关环境变量配置
localip=`ifconfig eth0 | awk '/inet/ {print $2}' | cut -f2 -d ":" |awk 'NR==1 {print $1}'`
nowip=`curl -s www.123cha.com |grep -o "[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}" |head -n 1`
export INSTALL_K3S_SKIP_DOWNLOAD=true
export K3S_NODE_IP=${localip}
export K3S_EXTERNAL_IP=${nowip}
export INSTALL_K3S_EXEC="agent --docker --token $K3S_TOKEN --server $K3S_URL --node-ip $K3S_NODE_IP --node-external-ip $K3S_EXTERNAL_IP"

# 安装k3s
bash ./offline/install.sh
```

