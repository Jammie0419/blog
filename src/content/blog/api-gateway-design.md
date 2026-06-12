---
title: '从零搭建高并发 API 网关 —— 架构设计与实践'
description: '分享使用 Go 语言从零开始搭建高性能 API 网关的经验，涵盖架构设计、限流算法、服务发现等核心模块。'
pubDate: 2026-06-01
tags: ['Go', 'API网关', '高并发', '微服务']
---

## 背景

在微服务架构中，API 网关是流量入口的核心组件。本文分享我使用 Go 语言从零搭建 API 网关的实践过程。

## 架构设计

网关的核心职责包括：

- **路由转发**：根据请求路径将流量转发到对应的后端服务
- **限流熔断**：保护后端服务不被突发流量压垮
- **鉴权认证**：统一处理身份验证与权限校验
- **负载均衡**：将请求均匀分发到多个服务实例

### 为什么选择 Go？

Go 语言在并发处理上有天然优势，goroutine + channel 的组合使得高并发场景下的性能表现优异。

## 限流算法

我们实现了三种主流限流算法：

### 令牌桶算法

```go
type TokenBucket struct {
    rate       float64
    capacity   int
    tokens     float64
    lastRefill time.Time
}

func (tb *TokenBucket) Allow() bool {
    tb.refill()
    if tb.tokens >= 1 {
        tb.tokens--
        return true
    }
    return false
}
```

### 漏桶算法

漏桶算法以固定速率处理请求，适合平滑突发流量的场景。

## 性能测试

经过压测，单节点 QPS 达到 **10w+**，P99 延迟 **< 5ms**。

> 性能调优的关键在于减少不必要的内存分配和锁竞争。

## 总结

API 网关的设计需要权衡功能丰富度与性能损耗。我们选择了插件化架构，让每个功能模块可以独立启用或关闭。
