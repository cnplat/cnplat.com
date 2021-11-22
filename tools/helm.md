# Helm

## 介绍

Github: [https://github.com/helm/helm](https://github.com/helm/helm)

## 安装

每个Helm 版本都提供了各种操作系统的二进制版本，这些版本可以手动下载和安装。

1. 下载 [需要的版本](https://github.com/helm/helm/releases)
2. 解压(tar -zxvf helm-v3.0.0-linux-amd64.tar.gz)
3. 在解压目中找到helm程序，移动到需要的目录中(mv linux-amd64/helm /usr/local/bin/helm)

然后就可以执行客户端程序

## 基础使用

### 添加稳定的仓库

```
helm repo add bitnami https://charts.bitnami.com/bitnami # 添加仓库
helm repo update # 确定我们可以拿到最新的charts列表
```

### 安装示例

您可以通过helm install 命令安装chart。 Helm可以通过多种途径查找和安装chart， 但最简单的是安装官方的bitnami charts。

```shell
$ helm repo update              # 确定我们可以拿到最新的charts列表
$ helm install bitnami/mysql --generate-name
NAME: mysql-1612624192
LAST DEPLOYED: Sat Feb  6 16:09:56 2021
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES: ...
```
在上面的例子中，`bitnami/mysql`这个chart被发布，名字是`mysql-1612624192`

您可以通过执行`helm show chart bitnami/mysql`命令简单的了解到这个chart的基本信息。 或者您可以执行`helm show all bitnami/mysql`获取关于该chart的所有信息。

每当您执行`helm install`的时候，都会创建一个新的发布版本。 所以一个chart在同一个集群里面可以被安装多次，每一个都可以被独立的管理和升级。


`helm`是一个拥有很多能力的强大的命令，更多信息详见[使用 Helm](https://helm.sh/zh/docs/intro/using_helm/)
