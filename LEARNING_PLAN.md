# 3个月学习路线 & 每周任务计划
背景：10年大型后台开发，擅长微服务、高可用、分布式、容器、CI/CD；重点补齐LLM/Agent工程能力。
> 工作日：1.5~2h；周末4~6h；每周产出代码/笔记，提交到本仓库。

## 阶段1｜第1～4周：LLM应用基础 + RAG + Context工程（岗位硬性门槛）
目标：理解LLM推理、Token、KV Cache、Function Calling、RAG完整链路、Context Engineering；独立跑通基础Agent Demo

### 第1周｜LLM推理基础 & LLM应用核心概念
学习内容
1. LLM推理侧原理（不学习预训练/微调，只看推理）
    - Token机制、token计数、上下文窗口、上下文压缩（context compaction）
    - KV Cache、Prefix Cache、流式输出Streaming原理；推理链路瓶颈分析
    - LLM能力边界：幻觉、输出随机性、Function Calling底层逻辑
2. 模型网关基础：OpenAI兼容API，多模型路由、模型降级、限流
3. 阅读材料：OpenAI Function Calling文档、Anthropic关于context window最佳实践

实操任务
1. Python脚本调用Ollama本地模型，实现stream流式返回
2. 对比普通调用 vs function calling调用；记录token消耗
3. 本地部署Ollama，跑Llama3，搭建本地模型服务

输出物：调用Demo，笔记记录token消耗对比

### 第2周｜Prompt工程 + Context Engineering
学习内容
1. Prompt基础：Few-shot、CoT、Self-Consistency、结构化输出（JSON Schema约束）
2. Context Engineering
    - 会话短期上下文、长记忆区分；上下文裁剪、摘要压缩、动态组装
    - 上下文优先级、去重、过滤策略；compact取舍
3. Guardrail基础：输入校验、输出过滤、内容安全拦截思路

实操任务
1. Schema约束LLM强制输出JSON，处理容错
2. 实现简单上下文压缩：长对话自动摘要，减少token

输出物：上下文管理模块 + 技术笔记：上下文压缩的取舍

### 第3周｜RAG完整体系
学习内容
1. RAG全链路：文档解析 → chunk分块 → Embedding → 向量库存储 → 召回 → Rerank → 拼接prompt
2. 向量库原理：向量索引、相似度检索；Milvus/Qdrant/Chroma对比
3. RAG常见问题：chunk大小、召回噪声、幻觉抑制、混合检索（关键词+向量）
4. RAG评测指标：召回率、精准率

实操任务
1. 本地搭建RAG：文档解析、分块、存入Qdrant；检索+Rerank
2. 测试不同chunk大小，对比回答质量和token消耗

输出物：RAG基础模块，README记录分块策略对比

### 第4周｜Agent基础范式 + 开源框架入门
学习内容
1. Agent范式：ReAct、Plan&Execute、Self-critique；核心组件Planner、Tool、Memory、State
2. Multi-agent基础：主Agent + Sub-agent分工，简单多智能体通信
3. LangGraph（重点），了解LangChain、LlamaIndex

实操任务
1. LangGraph搭建基础Agent：工具调用（计算器/文件读取）+ RAG检索工具
2. 会话短期记忆，多轮对话

输出物：RAG-Agent Demo，仓库雏形完成

> 阶段1验收：可以口述完整Agent链路；Demo完成：用户提问→检索知识库→调用工具→LLM回答

## 阶段2｜第5～8周：Agent服务化、可观测、Eval评测，Demo升级生产级服务
目标：单进程Demo改造为微服务；链路追踪、评测体系，体现后端工程能力

### 第5周｜Agent服务化架构 + 异步任务编排
学习内容
1. Agent系统和传统微服务差异：LLM不稳定、长耗时任务、输出不确定、token为资源成本
2. Agent会话状态管理：状态持久化、快照、断点续跑
3. 异步任务队列：长任务处理、超时、重试、熔断、限流
4. 多租户基础设计：会话隔离、基础数据隔离

实操任务
1. 重构Demo为Go后端服务；LangGraph内部服务，Go HTTP网关，SSE流式返回
2. 会话持久化PostgreSQL+Redis；保存会话状态
3. Asynq任务队列处理>30s长任务

输出物：可HTTP访问Agent微服务，简易系统设计文档

### 第6周｜Agent可观测体系 OpenTelemetry Trace
学习内容
1. OpenTelemetry：Trace、Span、Metrics、Log
2. Agent埋点设计：用户输入、LLM调用、检索、工具调用、子Agent调用
3. 指标：延迟、LLM调用次数、token消耗、任务成功率、失败分类

实操任务
1. Agent全链路OTel埋点，追踪思考、工具调用、LLM请求
2. 采集输入输出token、耗时指标

输出物：Jaeger查看完整Agent链路，指标原型

### 第7周｜Agent Eval评测体系 + Guardrail
学习内容
1. Agent评测和单元测试区别；自动评测+人工评测
2. 指标：任务完成率、幻觉检测、工具调用正确性、回答相关性
3. 评测数据集；LLM-as-judge自动评测
4. Guardrail落地：输入输出校验、敏感信息拦截

实操任务
1. 搭建评测Harness：批量跑测试用例，统计成功率、token消耗
2. LLM-as-judge自动评估输出质量

输出物：自动化评测脚本，批量执行输出评测报告

### 第8周｜阶段2整合 + 架构文档撰写
学习内容：回顾，梳理瓶颈，优化方向

实操任务
1. 整合模块：RAG+Agent+异步任务+OTel+Eval Harness
2. 编写DDD/Clean Architecture风格系统设计文档，包含：域划分、数据流、架构图、风险策略

输出物：MVP完整项目 + 正式架构文档

> 阶段2验收：可部署企业级Agent平台MVP，链路追踪+自动化评测

## 阶段3｜第9～12周：Runtime、沙箱、MCP、Harness底座、企业私有化（加分项）
目标：补齐Managed Agent岗位加分技能，项目升级适配ToB企业场景

### 第9周｜Agent Runtime、多智能体编排
学习内容
1. Agent Runtime：任务生命周期、状态快照、错误恢复
2. Multi-Agent编排：主Agent调度Sub-Agent、角色分工、消息传递
3. MCP（Model Context Protocol）、A2A协议基础

实操任务
1. 项目增加多子Agent编排
2. MCP客户端演示，调用外部工具

输出物：多智能体Demo，MCP调用示例

### 第10周｜沙箱隔离 E2B / gVisor / Firecracker
学习内容
1. 代码沙箱原理；CPU/内存/网络资源限制、逃逸防护
2. 沙箱生命周期：创建、销毁、超时回收；优先E2B上手，Firecracker/gVisor理解原理

实操任务
1. 接入E2B沙箱，Agent安全执行Python代码
2. 增加资源配额，防止无限执行

输出物：沙箱模块，Agent可安全运行代码

### 第11周｜Harness基础设施、spec-driven AI Coding范式
学习内容
1. Harness模块：执行轨迹回放、工具注册中心、评测数据管理
2. spec-driven范式：AI作为委派单元，spec定义任务，harness验证闭环

实操任务
1. 轨迹回放：存储Agent每一步思考、工具入参出参，接口查询回放记录
2. spec样例，harness验证Agent输出

输出物：简易Agent开发者Harness底座

### 第12周｜企业多租户、私有化交付 + 项目完善 + 面试材料打磨
学习内容
1. AI场景多租户隔离：向量库隔离、会话隔离、token资源配额
2. 私有化部署、VPC、合规审计、审计日志、权限模型

实操任务
1. 增加租户隔离层，审计日志
2. 完善README、架构图、部署文档；准备项目讲解材料

输出物：最终版Github项目，全套面试项目材料

## 提交规范
每周完成学习与代码，commit格式示例：
- feat: week1 add ollama streaming demo & token analysis
- docs: week2 context compression note
- feat: week5 go http gateway + async task queue

> 不一次性大量合并代码，保持迭代提交记录，便于面试查看开发过程。
