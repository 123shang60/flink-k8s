# zookeeper ha flink

- 使用 zookeeper 作为 flink 服务的高可用配置
- 使用 s3 作为高可用数据存储后端

## why

自 flink 1.12 版本起，native k8s flink 就已经支持使用 k8s configmap 作为高可用存储。但是，在以下几种情况下，受限于条件限制，可能还是需要使用基于 zookeeper 的 ha 方案：

- 使用的是 flink 1.11 版本
- 集群 k8s 版本过低，不支持 flink 高可用

## 与普通部署区别

1. 需要在启动参数中增加高可用选项的配置：
   - `high-availability: zookeeper`
   - `high-availability.zookeeper.quorum: zookeeper:2181`
   - `high-availability.zookeeper.path.root: /flink`
   - `high-availability.cluster-id: /cluster_one`
   - `high-availability.storageDir: s3://flink/recovery`

    > 配置放在配置文件中与放在启动参数中没有本质区别，此部分也可以放入配置文件中

2. 需要在配置文件中新增 s3 相关的配置：
   - `s3.access-key: xxxxxx`
   - `s3.secret-key: xxxxxx`
   - `s3.endpoint: http://s3`
   - `s3.path.style.access: true`
   
   > 配置放在配置文件中与放在启动参数中没有本质区别，此部分也可以放入启动参数中

3. 构建镜像时，需要执行以下命令，将 `flink-s3-fs-hadoop` 依赖加入 flink 依赖中
   - `mkdir -p /opt/flink/plugins/s3-fs-hadoop`
   - `cp /opt/flink/opt/flink-s3-fs*.jar /opt/flink/plugins/s3-fs-hadoop/`


## demo

启动 demo 可以参考本目录下 [zookeeper-ha.yaml](./zoookeeper-ha.yaml) 文件，需要自行准备 `zookeeper` 及 `s3` 环境