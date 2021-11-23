# GitOps连续交付之argo-cd

## 介绍

Argo CD是用于Kubernetes的声明性GitOps连续交付工具。

Github: [https://github.com/argoproj/argo-cd](https://github.com/argoproj/argo-cd)

## 安装

### Github

```shell
kubectl create ns argocd
# 普通安装
kubectl apply -n argocd -f https://raw.githubusercontent.com/cnplat/yaml/main/argo-cd/install.yaml
# 高可用安装
# kubectl apply -n argocd -f https://raw.githubusercontent.com/cnplat/yaml/main/argo-cd/ha-application-crd.yaml
# kubectl apply -n argocd -f https://raw.githubusercontent.com/cnplat/yaml/main/argo-cd/ha-appproject-crd.yaml
# kubectl apply -n argocd -f https://raw.githubusercontent.com/cnplat/yaml/main/argo-cd/ha-install.yaml
# 获取argo-cd admin密码
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
nbTjEbeMQQlzcoZv
# Visit UI: https://<your server ip>:30810/
```

### Gitee

```shell
kubectl create ns argocd
# 普通安装
kubectl apply -n argocd -f https://gitee.com/cnplat/yaml/raw/main/argo-cd/install.yaml
# 高可用安装
# kubectl apply -n argocd -f https://gitee.com/cnplat/yaml/raw/main/argo-cd/ha-application-crd.yaml
# kubectl apply -n argocd -f https://gitee.com/cnplat/yaml/raw/main/argo-cd/ha-appproject-crd.yaml
# kubectl apply -n argocd -f https://gitee.com/cnplat/yaml/raw/main/argo-cd/ha-install.yaml
# 获取argo-cd admin密码
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
nbTjEbeMQQlzcoZv
# Visit UI: https://<your server ip>:30810/
```

## 基础使用

### 登录

输入登录账号`UserName`为`admin`，`Password`使用下面命令获取
```shell
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
nbTjEbeMQQlzcoZv
```

![登录页](https://raw.githubusercontent.com/cnplat/cnplat.com/main/static/image/argocd/login.jpeg)

![主页](https://raw.githubusercontent.com/cnplat/cnplat.com/main/static/image/argocd/home.jpeg)

### 添加GIT配置仓库

创建一个GIT仓库，后面Kubenetes相关的应用，公司开发的软件的部署yaml文件都会放在这里，argocd将基于这个仓库自动维护软件部署更新等操作。

现在，我们使用github作为演示，地址为`https://github.com/cnplat/yaml`

1. 点击`设置`页面。

![主页](https://raw.githubusercontent.com/cnplat/cnplat.com/main/static/image/argocd/addrepo/1.png)

2. 点击`Repositories`。

![主页](https://raw.githubusercontent.com/cnplat/cnplat.com/main/static/image/argocd/addrepo/2.png)

3. 点击`Connect Repo using HTTPS`，添加git仓库。

![主页](https://raw.githubusercontent.com/cnplat/cnplat.com/main/static/image/argocd/addrepo/3.png)

4. 输入git仓库地址，私有仓库需要输入`Username`和`Password`，最后点击左上角`Connect`按钮，完成添加。

![主页](https://raw.githubusercontent.com/cnplat/cnplat.com/main/static/image/argocd/addrepo/4.png)

5. 添加完成。

![主页](https://raw.githubusercontent.com/cnplat/cnplat.com/main/static/image/argocd/addrepo/5.png)