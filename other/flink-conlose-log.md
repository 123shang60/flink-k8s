# flink 1.11 打印日志到标准输出

1. 准备日志配置文件

    需要通过 configmap 或者直接打包配置到 flink 镜像中的形式，将日志配置文件替换为以下配置：

    `log4j.properties`
    ```properties
    rootLogger.level = INFO
    rootLogger.appenderRef.console.ref = ConsoleAppender

    # Log output from org.apache.flink.yarn to the console. This is used by the
    # CliFrontend class when using a per-job YARN cluster.
    logger.yarn.name = org.apache.flink.yarn
    logger.yarn.level = INFO
    logger.yarn.appenderRef.console.ref = ConsoleAppender
    logger.yarncli.name = org.apache.flink.yarn.cli.FlinkYarnSessionCli
    logger.yarncli.level = INFO
    logger.yarncli.appenderRef.console.ref = ConsoleAppender
    logger.hadoop.name = org.apache.hadoop
    logger.hadoop.level = INFO
    logger.hadoop.appenderRef.console.ref = ConsoleAppender

    # Log output from org.apache.flink.kubernetes to the console.
    logger.kubernetes.name = org.apache.flink.kubernetes
    logger.kubernetes.level = INFO
    logger.kubernetes.appenderRef.console.ref = ConsoleAppender

    appender.console.name = ConsoleAppender
    appender.console.type = CONSOLE
    appender.console.layout.type = PatternLayout
    appender.console.layout.pattern = %d{yyyy-MM-dd HH:mm:ss,SSS} %-5p %-60c %x - %m%n

    # suppress the warning that hadoop native libraries are not loaded (irrelevant for the client)
    logger.hadoopnative.name = org.apache.hadoop.util.NativeCodeLoader
    logger.hadoopnative.level = OFF

    # Suppress the irrelevant (wrong) warnings from the Netty channel handler
    logger.netty.name = org.apache.flink.shaded.akka.org.jboss.netty.channel.DefaultChannelPipeline
    logger.netty.level = OFF
    ```

    `logback.xml`
    ```xml
    <configuration>
        <appender name="console" class="ch.qos.logback.core.ConsoleAppender">
            <encoder>
                <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{60} %X{sourceThread} - %msg%n</pattern>
            </encoder>
        </appender>

        <logger name="ch.qos.logback" level="WARN" />
        <root level="INFO">
            <appender-ref ref="console"/>
        </root>
    </configuration>
    ```

2. 在启动时增加新的配置参数：

    `-Dkubernetes.container-start-command-template='%java% %classpath% %jvmmem% %jvmopts% %logging% %class% %args%' `

这样启动的集群就能以标准输出的形式，向控制台打印日志。日志就可以通过 `kubectl logs ` 命令轻松看到