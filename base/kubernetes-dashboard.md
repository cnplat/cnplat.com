# Kubernetes运维管管理之Kubernetes Dashboard

## 介绍

Github: [https://github.com/kubernetes/dashboard](https://github.com/kubernetes/dashboard)

## 安装

```shell
kubectl apply -f https://raw.githubusercontent.com/cnplat/argo-apps/main/kubernetes-dashboard/kubernetes-dashboard.yaml
kubectl apply -f https://raw.githubusercontent.com/cnplat/argo-apps/main/kubernetes-dashboard/dashboard-adminuser.yaml
# 获取登录token
kubectl describe secret admin-user --namespace=kube-system
# Visit: https://<your server ip>:30801/
```