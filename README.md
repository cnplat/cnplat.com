# 中小型企业云原生入坑指南

Github: [https://github.com/cnplat/cnplat.com](https://github.com/cnplat/cnplat.com)

Gitbook: [https://docs.cnplat.com/](https://docs.cnplat.com/)

## 目录

- [x] [前言](preface/README.md)
  - [ ] [云原生是什么](preface/why-cloud-native.md)
  - [x] [适合的读者](preface/suitable-readers.md)
  - [ ] [致谢](preface/thanks.md)
- [x] [准备](ready/README.md)
  - [ ] [公有云的Kubernetes服务](ready/public-cloud-kubernetes.md)
  - [x] [安装轻量级Kubernetes之K3S](ready/install-k3s.md)
  - [x] [使用kubeadm安装Kubernetes](ready/install-kubernetes-for-kubeadm.md)
- [x] [Kubernetes的工作负载资源](workloads/README.md)
  - [x] [Pods](https://kubernetes.io/zh/docs/concepts/workloads/pods/)
  - [x] [Deployments](https://kubernetes.io/zh/docs/concepts/workloads/controllers/deployment/)
  - [x] [ReplicaSet](https://kubernetes.io/zh/docs/concepts/workloads/controllers/replicaset/)
  - [x] [StatefulSets](https://kubernetes.io/zh/docs/concepts/workloads/controllers/statefulset/)
  - [x] [DaemonSet](https://kubernetes.io/zh/docs/concepts/workloads/controllers/daemonset/)
  - [x] [Jobs](https://kubernetes.io/zh/docs/concepts/workloads/controllers/job/)
  - [x] [CronJob](https://kubernetes.io/zh/docs/concepts/workloads/controllers/cron-jobs/)
  - [x] [定制资源](https://kubernetes.io/zh/docs/concepts/extend-kubernetes/api-extension/custom-resources/)
- [ ] [Kubernetes生态工具](tools/README.md)
  - [ ] [Kubernetes IDE之Lens](tools/lens.md)
  - [x] [Kubernetes包管理器之Helm](tools/helm.md)
  - [x] [可视化运维管理之Kubernetes Dashboard](tools/kubernetes-dashboard.md)
  - [x] [GitOps连续交付之argo-cd](tools/argo-cd.md)
  - [ ] [云原生监控报警系统之Kube-Prometheus](tools/kube-prometheus.md)
  - [ ] [云原生全链路追踪系统之Jaeger](tools/jaeger.md)
- [ ] [基础设施选型](base/README.md)
  - [ ] [HTTP反向代理、服务发现、负载均衡之Traefik](base/traefik.md)
  - [ ] [云原生开源企业级分布式存储之longhorn](base/longhorn.md)
  - [ ] [MySQL水平扩展集群系统之Vitess](base/vitess.md)
  - [ ] [真正的列式数据库管理系统之ClickHouse](base/clickhouse.md)
  - [ ] [实时分布式消息传递平台之NSQ](base/nsq.md)
- [ ] [框架选型](frame/README.md)
  - [ ] [分布式多语言多平台微服务构建框架之dapr](frame/dapr.md)
- [ ] [附录](appendix.md)
