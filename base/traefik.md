# HTTP 反向代理、服务发现、负载均衡之Traefik

## 介绍

Github: [https://github.com/traefik/traefik/](https://github.com/traefik/traefik/)

## 安装

### Github

```
kubectl create ns traefik
kubectl apply -n traefik -f https://raw.githubusercontent.com/cnplat/yaml/main/traefik/traefik-crd.yaml
kubectl apply -n traefik -f https://raw.githubusercontent.com/cnplat/yaml/main/traefik/traefik-rbac.yaml
kubectl apply -n traefik -f https://raw.githubusercontent.com/cnplat/yaml/main/traefik/traefik-ingress-controller.yml
```

### Gitee

```shell
kubectl create ns traefik
kubectl apply -n traefik -f https://gitee.com/cnplat/yaml/raw/main/traefik/traefik-crd.yaml
kubectl apply -n traefik -f https://gitee.com/cnplat/yaml/raw/main/traefik/traefik-rbac.yaml
kubectl apply -n traefik -f https://gitee.com/cnplat/yaml/raw/main/traefik/traefik-ingress-controller.yml
```


## 基础使用