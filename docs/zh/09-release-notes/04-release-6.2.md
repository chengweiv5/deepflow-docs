---
title: DeepFlow 6.2 Release Notes
permalink: /release-notes/release-6.2
---

# 6.2.5 [2023/03/21]

## 新特性 (Alpha)

- Universal Application Topology
  - 增加`方向得分`指标，得分越高时客户端、服务端方向的准确性越高，得分为 255 时方向一定是正确的。
- Querier API
  - PromQL 查询 Prometheus 原生指标时，支持使用 DeepFlow AutoTagging 自动注入的标签

## 新特性 (GA)

- Universal Application Topology
  - **支持零插码自动展现进程粒度的全景应用拓扑** [FR-001-小米](https://github.com/deepflowio/deepflow/issues/1481)
- Integration
  - 将 OpenTelemetry Span 数据预聚合为服务和路径指标
- AutoTagging
  - 当无法按 Pod 分组时，auto\_service、auto\_instance（resource\_glX）优先按进程分组

## 优化

- Management
  - 支持配置小时粒度的数据存储时长
  - 支持统一设置公有云账号下所有托管 K8s 集群的`额外对接路由接口`
  - 提供两种 deepflow-agent 二进制包：动态链接、静态链接，前者依赖 glibc 动态链接库，后者在多线程下 malloc/free 锁竞争明显
- Querier API
  - 自定义类型的 Tag（k8s.label/cloud.tag/os.app）的 Category 统一为 map\_item

# 6.2.4 [2023/03/07]

## 新特性（Alpha）

- Integration
  - 将 OpenTelemetry Span 数据预聚合为服务和路径指标
- AutoTagging
  - 支持批量录入负载均衡器及其监听器的信息 [FR-022-小米](https://github.com/deepflowio/deepflow/issues/2406)
  - 当无法按 Pod 分组时，auto\_service、auto\_instance（resource\_glX）优先按进程分组

## 新特性 (GA)

- AutoTagging
  - 自动继承父进程上标记的元数据 [FR-024-小米](https://github.com/deepflowio/deepflow/issues/2456)
- SQL API
  - 支持 SLIMIT 参数限制返回的 Series 数量

## 优化

- AutoTagging
  - 进程粒度的应用拓扑适配端口复用的场景 [ISSUE-#2394](https://github.com/deepflowio/deepflow/issues/2394)
  - 字段改名：使用 auto\_instance 替代 resource\_gl0，使用 auto\_serivce 替代 resource\_gl2
- Management
  - 支持配置 deepflow-agent list k8s-apiserver 的时间间隔
  - 支持指定采集器所在环境的 Hostname

# 6.2.3 [2023/02/21]

## 新特性 (Alpha)

- SQL API
  - 新增 SLIMIT 参数以限制返回结果中的 Series 数量

## 新特性 (GA)

- Universal Application Topology
  - **支持利用 TOA（TCP Option Address）机制计算 NAT 前、后的真实访问关系** [FR-002-小米](https://github.com/deepflowio/deepflow/issues/1490)
- AutoTagging
  - 进程自动继承其父进程的元数据（os\_app 标签） [FR-024-小米](https://github.com/deepflowys/deepflow/issues/2456)
  - 支持同步百度云云智能网（CSN）资源信息
- Grafana
  - 增加 Grafana backend 插件模块，支持标准的 Grafana 告警策略配置

## 优化

- Management
  - 远程升级云服务器上的 deepflow-agent 可完全通过 deepflow-ctl 完成，无需为 deepflow-server 手动挂载 hostPath
- AutoTagging
  - 适配 K8s 1.18、1.20 的资源信息同步
- SQL API
  - 获取 enum 类型 Tag 字段的可选值时，返回取值对应的描述信息

# 6.2.2 [2023/02/07]

## 新特性 (GA)

- AutoTracing
  - **支持零插码的 Golang 应用分布式追踪**
- AutoTagging
  - **支持为进程、云服务器、K8s Namespace 增加自定义元数据** [FR-001-小米](https://github.com/deepflowys/deepflow/issues/1481)
  - 支持自动同步 AWS 和阿里云账号下的 K8s 集群信息
- Management
  - 支持 ClickHouse 节点数大于 deepflow-server 副本数 [FR-003-中通](https://github.com/deepflowys/deepflow/issues/1623)
  - 支持 deepflow-agent 以普通进程（而非 Pod）形态运行于 K8s Node 上 [FR-004-腾讯](https://github.com/deepflowys/deepflow/issues/1710)
  - 支持为 deepflow-agent 指定域名形式的 controller 或 ingester 地址 [FR-008-小米](https://github.com/deepflowys/deepflow/issues/1998)

## 优化

- deepflow-agent
  - 扫描进程的正则表达式列表（os-proc-regex）支持配置 action=drop 用于表达忽略语义 [FR-010-小米](https://github.com/deepflowys/deepflow/issues/2280)
  - 支持运行于低于 3.0 的 Linux Kernel 环境中 [FR-012-小米](https://github.com/deepflowys/deepflow/issues/2283)
  - 使用操作系统的 socket 信息校正流日志的方向 [FR-011-小米](https://github.com/deepflowys/deepflow/issues/2281)
  - 当 agent 运行环境的 ctrl\_ip 或 ctrl\_mac 发生变化时，支持自动更新 agent 的相应信息
- deepflow-server
  - UDP 流超时结束时，l4\_flow\_log 的 status 字段置为正常

# 6.2.1 [2023/01/17]

## 新特性 (Alpha)

- Universal Application Topology
  - **支持零插码自动展现进程粒度的全景应用拓扑** [FR-001-小米](https://github.com/deepflowio/deepflow/issues/1481)
- AutoTagging
  - **支持为进程、云服务器、K8s Namespace 增加自定义元数据** [FR-001-小米](https://github.com/deepflowio/deepflow/issues/1481)
  - 支持自动同步 AWS 和阿里云账号下的 K8s 集群信息
  - 支持同步百度云云智能网（CSN）资源信息
- Querier API
  - **支持 PromQL**
- Management
  - 支持 ClickHouse 节点数大于 deepflow-server 副本数 [FR-003-中通](https://github.com/deepflowio/deepflow/issues/1623)
  - 支持 deepflow-agent 以普通进程（而非 Pod）形态运行于 K8s Node 上 [FR-004-腾讯](https://github.com/deepflowio/deepflow/issues/1710)
  - 支持为 deepflow-agent 指定域名形式的 controller 或 ingester 地址 [FR-008-小米](https://github.com/deepflowio/deepflow/issues/1998)

## 优化

- Querier API
  - 支持返回 AS 之前的原始字段名
- Grafana
  - 优化 Enum 类型的 Variable，避免选择 All 时在 SQL 中将所有候选值全部展开

# 6.2.0 [2022/12/29]

## 新特性 (Alpha)

- AutoTracing
  - **支持零插码的 Golang 应用分布式追踪**
- Universal Application Topology
  - **支持利用 TOA（TCP Option Address）机制计算 NAT 前、后的真实访问关系** [FR-002-小米](https://github.com/deepflowio/deepflow/issues/1490)

## 优化

- AutoTracing
  - 当应用进程仅作为客户端时，避免 eBPF 将所有请求关联至同一个 Trace
  - 简化 eBPF 代码中的协议识别逻辑 [FR-005-云杉](https://github.com/deepflowio/deepflow/issues/1739)
- Management
  - 支持创建 agent-group 时指定 group ID [FR-007-小米](https://github.com/deepflowio/deepflow/issues/1864)
