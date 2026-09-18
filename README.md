# start_ai
> Author: seeyouace
> Background: 10年大型互联网后端开发工程师，本仓库用于记录转型AI Agent后端开发的学习笔记 + 企业级Managed Agent平台Demo项目迭代。
> Target Position: Agent后端 / Managed Agents / Agent Harness 研发（企业多租户智能体平台方向）

## 仓库说明
本仓库包含两大块内容：
1. 【学习计划】3个月系统学习路线，从LLM应用栈、RAG、Context Engineering，到Agent Runtime、沙箱、Harness基础设施，按周安排学习+实操任务。
2. 【工程项目】agent-platform-demo：企业托管Agent平台MVP，对标Managed Agents岗位技术要求，Clean Architecture + DDD，具备多租户、RAG、Agent编排、可观测、评测Harness、沙箱执行能力。

> 项目定位：不是简单LangChain Demo，而是面向企业私有化交付的智能体平台后端底座，突出后端工程能力、稳定性、可运维性。

## 目录结构
start_ai/
├── LEARNING_PLAN.md         # 3 个月完整学习路线（按周）
├── PROJECT_ARCH.md          # Agent 平台项目整体架构
├── README.md
├── .gitignore
└── docs/
├── ADR.md               # 架构决策记录
├── evals.md             # Agent 评测 Harness 设计
└── deployment.md         # docker-compose 本地部署文档

## 学习目标
补齐AI Agent后端岗位必备知识：
- LLM推理基础、KV Cache、Streaming、Token机制
- Prompt & Context Engineering
- RAG全链路工程化
- Agent范式：ReAct / Plan&Execute、LangGraph、Function Calling
- Agent服务化、异步任务、状态持久化
- OpenTelemetry全链路可观测
- Agent Eval自动化评测体系
- Agent Runtime、多智能体编排、MCP协议
- 代码沙箱 E2B，安全执行环境
- 企业场景：多租户隔离、私有化部署、合规审计、资源配额

## 项目能力清单
- 多租户会话管理，租户数据隔离
- LangGraph实现Agent编排，支持工具调用 + RAG检索
- 多轮对话记忆、上下文压缩优化token消耗
- 长任务异步队列，任务持久化、断点续跑
- OpenTelemetry全链路追踪，记录Agent每一步思考、LLM调用、工具调用
- Agent自动化评测Harness，LLM-as-judge自动打分，支持回归测试
- E2B代码沙箱，安全执行Agent生成代码，资源限制
- MCP协议工具调用演示
- SSE流式输出
- Docker Compose一键本地部署，CI/CD Github Action

## 技术栈
- 后端主语言：Go
- Agent编排层：LangGraph(Python)
- 存储：PostgreSQL(元数据)，Redis(缓存/任务状态)，Qdrant(向量库RAG)
- 任务队列：Asynq（Go异步任务）
- 可观测：OpenTelemetry + Jaeger + Prometheus
- 沙箱：E2B
- LLM：Ollama本地开源模型 + OpenAI兼容模型网关

## 学习周期
总周期：12周（3个月）
- 阶段1(1-4周)：LLM应用基础 + RAG + Context工程 + Agent基础Demo
- 阶段2(5-8周)：服务化改造 + 可观测 + Eval评测，产出MVP
- 阶段3(9-12周)：Runtime、沙箱、MCP、Harness、企业多租户私有化能力

## 面试亮点
> 区别普通AI Demo：用传统大型后端架构思维解决LLM不确定性、长任务、token成本、企业安全隔离等工程问题。
