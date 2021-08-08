# flink 私有镜像拉取

k8s 支持通过 secret 配置加载私库镜像。flink 在依赖私库镜像时，也可以通过增加启动参数，支持从私有仓库拉取镜像

需要在 k8s 同命名空间中增加对应的 secret ，同时在 flink 启动参数中增加 

```
-Dkubernetes.container.image.pull-secrets=xxxxxx
```

即可实现 flink 拉取私有镜像


更多 k8s 配置参数可以参考 [flink 配置参数表](https://ci.apache.org/projects/flink/flink-docs-release-1.11/ops/config.html#kubernetes)