# GitOps连续交付之argo-cd

## 介绍

Argo CD是用于Kubernetes的声明性GitOps连续交付工具。

Github: [https://github.com/argoproj/argo-cd](https://github.com/argoproj/argo-cd)

## 安装

```shell
~ kubectl create ns argocd
~ kubectl apply -n argocd -f https://raw.githubusercontent.com/cnplat/architect/main/argo/cd.yaml
# 查看相关服务状态，STATUS全部为Running后进行下一步
~ kubectl get pods -n=argocd
NAME                                      READY   STATUS            RESTARTS   AGE
pod/argocd-application-controller-0       1/1     Running           0          3m33s
pod/argocd-dex-server-6dcf645b6b-ph9z6    1/1     Running           0          3m34s
pod/argocd-redis-5b6967fdfc-8bthj         1/1     Running           0          3m34s
pod/argocd-repo-server-7598bf5999-l9q9v   1/1     Running           0          3m34s
pod/argocd-server-79f9bc9b44-vctx2        1/1     Running           0          3m34s
# 获取argo-cd admin密码
~ kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
nbTjEbeMQQlzcoZv
# Visit UI: https://<your server ip>:30810/
```

## 基础使用

### 登录

输入登录账号`UserName`为`admin`，`Password`使用下面命令获取
```shell
~ kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
nbTjEbeMQQlzcoZv
```

![登录页](https://raw.githubusercontent.com/cnplat/cnplat.com/main/static/image/argocd/login.jpeg)

![主页](https://raw.githubusercontent.com/cnplat/cnplat.com/main/static/image/argocd/home.jpeg)