# ADR - Architecture Decision Records 架构决策记录
记录项目选型理由，面试重点，可以展示技术权衡思维

## ADR001: 后端主体语言选择 Go
- 决策：主业务服务使用Go，Agent编排逻辑使用Python LangGraph独立服务
- 理由：
1. 本人10年后端经验，熟悉Go；高并发、长连接SSE、异步任务性能好
2. LangGraph生态以Python为主，直接用Python做Agent编排，避免重写状态机
3. 服务之间HTTP/gRPC通信，解耦；可以独立扩容Agent编排服务
- 权衡：增加服务间调用开销；通过缓存减少调用次数

## ADR002: 向量库选择 Qdrant
- 决策：使用Qdrant，而非Milvus / Chroma
- 理由：轻量，docker部署简单，API友好，资源占用低，适合Demo项目；支持多租户collection隔离
- 权衡：大规模生产Milvus更成熟，Demo场景Qdrant足够

## ADR003: 任务队列选择 Asynq
- 决策：Go生态Asynq，基于Redis的任务队列
- 理由：轻量，原生Go，支持延迟任务、重试、任务状态持久化，快速集成
- 权衡：不适合超大规模分布式集群，满足本项目MVP需求

## ADR004: Agent编排使用LangGraph
- 决策：LangGraph作为Agent核心编排框架
- 理由：工业界主流，状态机模型，非常适合后端工程师理解；支持断点、状态持久化，适配长任务Agent
- 权衡：Python服务，需要跨服务调用

## ADR005: 代码沙箱选用E2B
- 决策：E2B，不直接原生Firecracker/gVisor
- 理由：开箱即用，API简单，资源限制、隔离能力满足Demo；Firecracker底层太重，项目MVP阶段不重复造轮子
- 权衡：生产私有化场景可替换为自建Firecracker/gVisor

## ADR006: 可观测选用OpenTelemetry
- 决策：OTel全链路埋点
- 理由：行业标准，可对接Jaeger/Prometheus；可以追踪Agent内部每一步思考、工具调用、LLM调用，非常适合Agent系统排查问题
