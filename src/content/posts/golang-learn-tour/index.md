---
title: golang随笔备忘录
published: 2026-03-09
description: ""
image: "./harbor.jpg"
tags: [后端, Web, Go]
draft: false

lang: ""
---

## 背景

为什么不做前端了？well...这个问题涉及的个人原因有些多，纯技术角度解释也很复杂，可以参考[杭电某 wiki](https://hdu-cs.wiki/s/751b2a65)，特指其中评价后端的某句话：

> 这些概念在不同的语言和框架之间是通用的。而核心的业务逻辑甚至基本可以脱离特定的 Web 框架而存在。你会学习到通用的 “道”，而不是仅仅某一个工具的 “术”。

总之对如今/后来的我，前端已经：不适合，不够擅长，使用范围重合低

为什么是 Go？很多人吐槽其控制流面条，不爽，OOP 弱，ORM 奇怪，巴拉巴拉，xxx

少关注太多别人的见解，每个人有自己的需求立场，不同的立场和角度得到的结论往往差别较大，乃至难以理解。关注自己的需求就好。就这么简单。

web 后端需求+朋友项目+杭州找饭吃+语言生态对我需求便利

完

看什么：

1. [go.dev 选看](https://go.dev/doc/tutorial/) + [module ref](pkg.go.dev) (ref) + 适当 llm + Google

2. [八股 1](https://xiaolincoding.com/) + [八股 2](https://golangguide.top/) + [CN 校准参考](https://goclub.space/docs/companion) + [CN-basic-synatx](https://gopl-zh.github.io/index.html) +

为什么 P1 不是直接给固定的各种 老 blog/教程/视频？

太老/太旧/八股 or 面试题为主/缺乏引导/中文过量/卖课过量/中文圈 go 匮乏/go 特殊性，原因很多

本篇目的？

没有目的，也不是给其它人看的。我的记性很差，故备忘录随笔糊个几下。老实说我已经越来越懒于写 blog 了，意义不足

## 大纲

- `Go`(basic syntax)
- `MySQL`/`PostgreSQL`
- `Gin`
- `Redis`
- `RabbitMQ`/`Kafka`
- `Docker compose`
- `K3s`/`K8s`
- gRPC
- Prometheus
- Grafana
- OpenTelemetry
- GORM

接下来部分跳着看，自取所需即可。想找工作就 web 框架+mysql+中间件+八股+面经+容器化，有远大追求另谈。

我层次比较低，以效率目的为先，填补构建在后。以及要找饭吃（）

## 阶段 A — 语言基础（语法、模块、并发）

- [ ] 安装 Go 并运行第一个程序
  - 文档：https://go.dev/doc/install
  - 练习：hello world、`go run`、`go build`、`go env`
- [ ] 完成 **A Tour of Go**（交互式）
  - 文档：https://go.dev/tour/
  - 重点：slices、maps、methods、interfaces、defer、panic/recover
  - 练习：跑完所有章节并做题
- [ ] 阅读 **Effective Go**（风格）
  - 文档：https://go.dev/doc/effective_go
  - 练习：把常见 idiom 写成小样例（命名、接口、错误处理）
- [ ] 学习 Go Modules (`go mod`)
  - 文档：https://go.dev/ref/mod
  - 练习：`go mod init`、添加依赖、查看 `go.sum`、配置 proxy
- [ ] 并发基础（goroutine / channel / context）
  - 参考文章：https://go.dev/blog/pipelines
  - 练习：实现 worker pool、并发爬虫（限制并发）、用 `context` 管理取消/超时

---

## 阶段 B - Web 后端必备（目标：能独立实现 REST API + 数据持久化 + 测试）

- [ ] 学习标准库 HTTP（优先）
  - 文档：https://pkg.go.dev/net/http
  - 练习：实现最小 HTTP server（路由、JSON I/O、日志 middleware、graceful shutdown、请求超时与 context）
- [ ] JSON 编码/解码与请求处理
  - 文档：https://pkg.go.dev/encoding/json
  - 练习：实现 `POST /users`、`GET /users/{id}`、输入校验、错误返回
- [ ] 可选：用轻量框架重写（对比实现）
  - Gin 文档：https://gin-gonic.com/en/docs/
  - 练习：用 Gin 改写并比较两种写法的优劣
- [ ] 数据库基础（以 PostgreSQL 为推荐）
  - PostgreSQL docs：https://www.postgresql.org/docs/
  - Go `database/sql`：https://pkg.go.dev/database/sql
  - 推荐驱动 / 示例库：`pgx` 或 `lib/pq`（在 github 搜索最新驱动）
  - 练习：设计 `users` 表，写 CRUD、事务、索引、分页
- [ ] 可选 ORM（理解其代价）
  - GORM docs：https://gorm.io/docs/index.html
  - 练习：用 GORM 实现 CRUD，并查看生成的 SQL
- [ ] 并发模式与后台任务
  - 练习：实现异步任务（邮件/图片处理）、保证安全关闭与没有 goroutine 泄露
- [ ] 测试（单元/集成）与基准
  - 文档：https://pkg.go.dev/testing
  - 练习：表驱动测试、集成测试（用 Docker Compose 启动 DB）、`go test -bench`
- [ ] 性能剖析（pprof）
  - 文档：https://pkg.go.dev/runtime/pprof
  - 练习：在服务暴露 `/debug/pprof/`，采集 CPU/heap 分析热点

---

## 阶段 C — 工程化与生产能力（目的：把服务做成可运维的系统）

- [ ] Redis 缓存与缓存策略
  - 文档：https://redis.io/docs/latest/
  - Go client（示例）：https://github.com/redis/go-redis
  - 练习：为 `GET /users/{id}` 添加缓存，设计缓存失效与一致性方案
- [ ] 消息与事件（Kafka / NATS 等）
  - Kafka docs：https://kafka.apache.org/documentation/
  - 练习：实现 `user_created` 事件生产/消费，处理幂等与重试
- [ ] RPC 与接口定义（gRPC + Protobuf）
  - gRPC Go：https://grpc.io/docs/languages/go/
  - 练习：用 protobuf 定义接口，搭建简单的 gRPC 服务并互相调用
- [ ] 容器化（Docker）
  - 文档：https://docs.docker.com/
  - 练习：编写高质量 Dockerfile（multi-stage）、构建并在本地运行镜像
- [ ] 编排与部署（Kubernetes）
  - 文档：https://kubernetes.io/docs/
  - 练习：用 minikube/kind 或云集群部署应用，熟悉 Deployment/Service/ConfigMap/Secret、滚动更新与探针
- [ ] 监控、可视化与追踪
  - Prometheus：https://prometheus.io/docs/introduction/overview/
  - Grafana：https://grafana.com/docs/
  - OpenTelemetry（Go）：https://opentelemetry.io/docs/languages/go/
  - 练习：导出指标，搭建 Grafana 面板，实践分布式追踪分析 P99
- [ ] CI/CD 与自动化流水线
  - GitHub Actions docs：https://docs.github.com/actions
  - 练习：实现 build → test → image → deploy 的流水线
- [ ] 安全与运维要点
  - 练习：Secrets 管理、TLS/HTTPS、依赖漏洞扫描、速率限制、日志审计、认证（JWT/OAuth）

### 随笔

Go 的 gc 和内存自动管理，导致 runtime 停机时间不可测，使得其无法做硬实时平台，与 java 和 py 类似

每个 go 项目为一个 module，每个 module 下，通常按惯例，每个 directory=package=library，package(except main)/lib can be imported by others who can access the module.others can use the func of the pack, like `fmt.Println()`.

btw, in "import" process, the statement like "math/fmt",it means the pack u import is "fmt",like:

`math/rand` -> rand

`net/http` -> http

and,module is unique. eg: github.com/gin-gonic/gin . it represent "module path + directory"

```
module
  ├─ directory
  │     └─ package
  ├─ directory
  │     └─ package
  └─ directory
        └─ package
```

the pack/dirc is a kind of "namespace".

蚌，发现自己 en 讲比 cn 貌似还顺畅些。

:::tip
Go Tour and the other similar, many exercises of them are CS-style learning, not production backend.

Don't get stuck thinking:

"If I can't solve this, I can't be a backend dev."

That is false.
:::
