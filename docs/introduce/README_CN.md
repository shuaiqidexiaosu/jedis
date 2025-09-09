# Jedis

[![发布](https://img.shields.io/github/release/redis/jedis.svg?sort=semver)](https://github.com/redis/jedis/releases/latest)
[![maven中心仓库地址](https://img.shields.io/maven-central/v/redis.clients/jedis.svg)](https://search.maven.org/artifact/redis.clients/jedis)
[![Java文档](https://www.javadoc.io/badge/redis.clients/jedis.svg)](https://www.javadoc.io/doc/redis.clients/jedis)
[![MIT已获得许可](https://img.shields.io/badge/license-MIT-blue.svg)](./LICENSE.txt)
[![集成](https://github.com/redis/jedis/actions/workflows/integration.yml/badge.svg?branch=master)](https://github.com/redis/jedis/actions/workflows/integration.yml)
[![codecov](https://codecov.io/gh/redis/jedis/branch/master/graph/badge.svg?token=pAstxAAjYo)](https://codecov.io/gh/redis/jedis)
[![聊天群](https://img.shields.io/discord/697882427875393627?style=flat-square)](https://discord.gg/redis)

<p align="center">
[ <a href="../../README.md">En</a> |
<b>中</b> ]
<b>Jedis 文档</b>
</p>

## 什么Jedis 是什么？

Jedis 是 [Redis](https://github.com/redis/redis "Redis") 的 Java 客户端，专为性能和易用性而设计。

您是否正在寻找一个用于处理对象映射的高级库？请参阅 [redis-om-spring](https://github.com/redis/redis-om-spring)！

## 如何使用 Redis？

[在 Redis 大学免费学习](https://university.redis.io/academy/)

[试用 Redis Cloud](https://redis.io/try-free/)

[深入了解开发者教程](https://redis.io/learn/)

[加入 Redis 社区](https://redis.io/community/)

[在 Redis 工作](https://redis.io/careers/jobs/)

## 支持的 Redis 版本

此库的最新版本支持以下 Redis 版本：
[5.0](https://github.com/redis/redis/blob/5.0/00-RELEASENOTES)、
[6.0](https://github.com/redis/redis/blob/6.0/00-RELEASENOTES)、
[6.2](https://github.com/redis/redis/blob/6.2/00-RELEASENOTES)、
[7.0](https://github.com/redis/redis/blob/7.0/00-RELEASENOTES)、
[7.2](https://github.com/redis/redis/blob/7.2/00-RELEASENOTES)、
[7.4](https://github.com/redis/redis/blob/7.4/00-RELEASENOTES) 和
[8.0](https://github.com/redis/redis/blob/8.0/00-RELEASENOTES)。

下表重点介绍了最新库版本与 Redis 和 JDK 版本的兼容性。兼容性包括通信功能和 Redis 命令功能。

| Jedis 版本 | 支持的 Redis 版本 | JDK 兼容性 |
|---------------|---------------------------------------|-------------------|
| 3.9+ | 5.0 到 6.2 版本系列 | 8, 11 |
| >= 4.0 | 5.0 到 7.2 版本系列 | 8, 11, 17 |
| >= 5.0 | 6.0 到当前版本 | 8, 11, 17, 21 |
| >= 5.2 | 7.2 到当前版本 | 8, 11, 17, 21 |
| >= 6.0 | 7.2 到当前版本 | 8, 11, 17, 21 |

## 入门

要开始使用 Jedis，首先将其作为依赖项添加到您的 Java 项目中。如果您使用的是 Maven，则如下所示：

```xml
<dependency>
<groupId>redis.clients</groupId>
<artifactId>jedis</artifactId>
<version>6.0.0</version>
</dependency>
```

要使用最新的 Jedis，请查看[此处](/docs/jedis-maven.md)。

接下来，您需要连接到 Redis。建议您安装一个 redis-stack docker：

```bash
docker run -p 6379:6379 -it redis/redis-stack:latest
```

对于许多应用程序来说，最好的实践是使用连接池。您可以像这样实例化一个 Jedis 连接池：

```java
JedisPool pool = new JedisPool("localhost", 6379);
```

有了 `JedisPool` 实例，您可以使用
[try-with-resources](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html)
块来获取连接并运行 Redis 命令。

以下是在 *try-with-resources* 块中运行单个 [SET](https://redis.io/commands/set) 命令的方法：

```java
try (Jedis jedis = pool.getResource()) {
jedis.set("clientName", "Jedis");
}
```

`Jedis` 实例实现了大多数 Redis 命令。请参阅
[Jedis Javadocs](https://www.javadoc.io/doc/redis.clients/jedis/latest/redis/clients/jedis/Jedis.html)
获取受支持命令的完整列表。

### 更简单的使用连接池的方法

每个命令都使用 *try-with-resources* 块可能比较繁琐，因此您可以考虑使用 JedisPooled。

```java
JedisPooled jedis = new JedisPooled("localhost", 6379);
```

现在您可以像从 Jedis 发送命令一样发送命令了。

```java
jedis.sadd("planets", "Venus");
```

## 连接 Redis 集群

Jedis 允许您连接到 Redis 集群，并支持 [Redis 集群规范](https://redis.io/topics/cluster-spec)。
为此，您需要使用 `JedisCluster` 进行连接。请参阅以下示例：

```java
Set<HostAndPort> jedisClusterNodes = new HashSet<HostAndPort>();
jedisClusterNodes.add(new HostAndPort("127.0.0.1", 7379));
jedisClusterNodes.add(new HostAndPort("127.0.0.1", 7380));
JedisCluster jedis = new JedisCluster(jedisClusterNodes);
```

现在，您可以使用 `JedisCluster` 实例并像使用标准池连接一样发送命令：

```java
jedis.sadd("planets", "Mars");
```

## 使用 Redis 模块

Jedis 支持 [Redis 模块](https://redis.io/docs/modules/)，例如 [RedisJSON](https://redis.io/json/) 和 [RediSearch](https://redis.io/search/)。

详情请参阅 [RedisJSON Jedis](docs/redisjson.md) 或 [RediSearch Jedis](docs/redisearch.md)。

## 故障转移

Jedis 支持 Redis 部署的重试和故障转移。这在以下情况下非常有用：

1. 您有多个 Redis 部署。这可能包含两个独立的 Redis 服务器，或者两个或多个跨多个 [双活 Redis Enterprise](https://redis.io/docs/latest/operate/rs/databases/active-active/) 集群复制的 Redis 数据库。
2. 您希望您的应用程序一次只连接到一个部署，并在第一个部署不可用时故障转移到下一个可用的部署。

有关完整的故障转移配置选项和示例，请参阅 [Jedis 故障转移文档](docs/failover.md)。

## 基于令牌的身份验证

Jedis 从 5.3.0 GA 版本开始支持基于令牌的身份验证 (TBA)。此功能由一个扩展库补充，该扩展库增强了开发者体验，并提供了 TBA 功能所需的大部分组件。

值得注意的是，该扩展库内置了对 **Microsoft EntraID** 的支持，可作为通用解决方案的一部分提供无缝集成。

有关更多详细信息和示例，请参阅 [高级用法](docs/advanced-usage.md) 文档。

## 文档

[Jedis wiki](http://github.com/redis/jedis/wiki) 包含几篇关于使用 Jedis 的有用文章。

您还可以查看[最新的 Jedis Javadocs](https://www.javadoc.io/doc/redis.clients/jedis/latest/index.html)。

一些具体的用例示例可以在测试源代码的[`redis.clients.jedis.examples`
包](src/test/java/redis/clients/jedis/examples/)中找到。

## 故障排除

如果您遇到问题或有任何疑问，我们随时为您提供帮助！
​​
请在[Redis Discord 服务器](http://discord.gg/redis)或
[Jedis GitHub 讨论区](https://github.com/redis/jedis/discussions)或
[Jedis 邮件列表](http://groups.google.com/group/jedis_redis)上联系我们。

## 贡献

我们期待您的贡献！

欢迎您随时报告错误！ [您可以在 GitHub 上提交错误报告](https://github.com/redis/jedis/issues/new)。

您也可以贡献文档——或任何有助于改进 Jedis 的内容。更多详情，请参阅
[贡献指南](https://github.com/redis/jedis/blob/master/.github/CONTRIBUTING.md)。

## 许可证

Jedis 采用 [MIT 许可证](https://github.com/redis/jedis/blob/master/LICENSE)。

## 赞助

[![Redis Logo](../../redis-logo-full-color-rgb.png)](https://redis.io/)