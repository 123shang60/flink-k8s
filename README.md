# 使用 k8s 部署 flink

## 基本思路

1. 新建 flink 的namespaces
2. 创建相关权限
3. init-job

最简单的 demo 版本可以参考 [demoflink](./demo-flink/README.md)

## ha

flink 官方支持两种模式的 ha

- [ZooKeeper HA Services](https://ci.apache.org/projects/flink/flink-docs-release-1.13/docs/deployment/ha/zookeeper_ha/)
- [Kubernetes HA Services](https://ci.apache.org/projects/flink/flink-docs-release-1.13/docs/deployment/ha/kubernetes_ha/) 
  
两种部署示例可以参考 [zookeeper ha flink](./zookeeper-ha-flink/README.md) 以及 [k8s ha flink](./k8s-ha-flink/README.md)

## 日志打印问题 (flink 1.11)

flink 1.11 版本下，默认部署的 flink 会将日志打印到文件中，不利于问题排查。为了解决此问题，可以参考 [flink日志打印到标准输出](./other/flink-conlose-log.md)

## 私库拉取问题

企业中部署可能需要使用私库镜像，可以参考 [flink 私库镜像](./other/private.md) 处理。