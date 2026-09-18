# Agent Platform Demo 项目架构文档
Clean Architecture + DDD 设计，面向企业托管Managed Agent平台

## 系统分层
1. **接入层**
    - HTTP/SSE接口，流式输出；鉴权、租户校验、限流
2. **应用服务层**
    - Agent会话服务、任务管理服务、RAG检索服务、评测服务
    - 异步任务队列，处理长耗时Agent任务
3. **领域层（核心业务逻辑 DDD）**
    - Agent领域：Agent实例、Agent状态、Planner、Tool调用、Memory
    - RAG领域：知识库、文档、分片、向量检索
    - Eval领域：评测用例、评测结果、LLM Judge
4. **基础设施层**
    - 模型网关：统一LLM请求，多模型路由、降级
    - 向量存储、关系数据库、缓存
    - OTel埋点、日志、指标
    - 沙箱执行环境、MCP客户端
5. **Harness开发者平台层**
    - 轨迹回放服务、工具注册中心、评测数据集管理

## 数据流
用户请求 → 接入层鉴权&租户校验 → 创建Agent任务投递队列 → Agent Planner生成计划 → 按需RAG检索 / 调用工具（普通工具/沙箱代码执行/MCP工具） → 调用LLM生成思考结果 → 保存执行轨迹与token指标 → 流式返回结果

Eval Harness流程：批量灌入测试用例，自动执行Agent任务，收集输出，LLM Judge打分，输出评测报告。

## 存储设计
- PostgreSQL：租户信息、会话元数据、任务记录、评测数据集、审计日志
- Redis：会话缓存、任务状态、分布式锁、限流
- Qdrant：向量存储，RAG知识库向量

## 核心模块
1. api-gateway：Go接入网关，SSE流式输出，鉴权限流
2. agent-service：Agent编排业务服务，调用LangGraph服务
3. rag-service：文档管理、切片、Embedding、向量检索、Rerank
4. eval-harness：评测底座，批量任务执行，LLM-as-judge
5. model-gateway：模型接入层，兼容OpenAI API，多模型路由
6. sandbox-service：E2B沙箱代理服务，代码执行，资源管控
7. langgraph-agent：Python实现LangGraph状态机Agent编排逻辑

## 关键非功能设计
1. 多租户隔离：会话数据隔离，向量库租户空间隔离，token配额隔离
2. 稳定性：任务超时、重试、熔断，LLM降级策略
3. 可观测：全链路OTel Trace，每一步Agent动作埋点；token消耗指标
4. 安全：沙箱资源限制，输入输出Guardrail，审计日志

## 部署方式
Docker Compose 本地一键拉起所有依赖服务，Github Action CI。
