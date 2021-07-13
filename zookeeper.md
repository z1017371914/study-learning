# Zookeeper

[toc]

## 节点类型

* PERSISTENT­持久化目录节点 

*  PERSISTENT_SEQUENTIAL­持久化顺序编号目录节点 

* EPHEMERAL­临时目录节点 

* EPHEMERAL_SEQUENTIAL­临时顺序编号目录节点 

*  Container 节点（3.5.3 版本新增，如果Container节点下面没有子节点，则Container节点 

  在未来会被Zookeeper自动清除,定时任务默认60s 检查一次） 

* TTL 节点( 默认禁用，只能通过系统配置 *zookeeper.extendedTypesEnabled=true* 开启，不稳 

  定)

## 命令

```shell
create -e /ep      xxx      # 创建临时节点

```

