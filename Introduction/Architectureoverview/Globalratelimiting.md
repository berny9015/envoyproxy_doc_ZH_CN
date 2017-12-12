### 全局限速

尽管分布式熔断在控制分布式系统中的吞吐量方面通常是非常有效的，但是有时并不是非常有效，所以需要全局速率限制。最常见的情况是，大量主机转发请求到少量主机，并且平均请求延迟较低（例如连接到数据库服务器的请求）。如果目标主机被备份，则下游主机将压倒上游集群。在这种情况下，要在每个下游主机上配置足够严格的熔断限制是非常困难的，这样系统将在典型的请求模式期间正常运行，但仍然可以防止系统因发生故障而引发的级联故障。全球限速是这种情况的一个很好的解决方案。

Envoy直接与全球gRPC限速服务集成。尽管可以使用任何实现定义的RPC/IDL协议的服务，但Lyft提供了使用Redis后端的Go编写的[参考实现](https://github.com/lyft/ratelimit)。Envoy的限速具有以下特点：

- **网络级别限制过滤器**：Envoy将为安装过滤器的侦听器上的每个新连接调用速率限制服务。配置指定一个特定的域和描述符设置为速率限制。这对速率限制每秒传送收听者的连接的最终效果。[配置参考](../../Configurationreference/Networkfilters/Ratelimit.md)。
- **HTTP级别限制过滤器**：Envoy将为安装过滤器的侦听器上的每个新请求调用速率限制服务，并且路由表指定应调用全局速率限制服务。对目标上游群集的所有请求以及从始发群集到目标群集的所有请求都可能受到速率限制。[配置参考](../../Configurationreference/HTTPfilters/Ratelimit.md)


限速服务[配置](../../Configurationreference/Ratelimitservice.md)


### 返回
- [架构介绍](../Architectureoverview.md)
- [简介](../../Introduction.md)
- [首页目录](../../README.md)