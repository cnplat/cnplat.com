# GitOps连续交付之argo-cd

## 介绍

Github: [https://github.com/argoproj/argo-cd](https://github.com/argoproj/argo-cd)

## 安装

```shell
kubectl create ns argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/cnplat/architect/main/argo/cd.yaml
# 获取argo-cd admin密码
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
# Visit cd: https://<your server ip>:30810/
```

## 基础使用