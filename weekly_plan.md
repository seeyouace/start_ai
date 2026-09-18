# Week1 Plan｜LLM 推理基础 & 模型调用入门

> 
> 仓库：seeyouace/start_ai
> 目标：掌握 Token、KV Cache、Streaming、Function Calling；本地 Ollama 跑通模型，产出可运行 Demo，提交 GitHub
> 工作日每日：1.5～2h；周末约 2h；**总耗时约 9.5h**
> 产出物：4 份 Python 脚本 + requirements.txt + 学习笔记 NOTE.md，放入`week1/`目录

## 周一｜LLM 推理核心概念（90 分钟）

目标：建立工程视角心智模型，理解 LLM 调用性能、成本、限制

1. 15min：Token 基础
   - 学习内容：Token 定义、按 Token 计费逻辑、上下文窗口（输入 + 输出最大 token）
   - 理解要点：中文约 1~2token；超出上下文窗口会丢失信息或报错
   - 产出：笔记，一句话总结 Token
2. 30min：推理核心机制
   - 学习：Streaming 流式输出、KV Cache、上下文压缩、LLM 不确定性（幻觉、输出随机性）
3. 45min：阅读官方 API 文档
   - OpenAI `/v1/chat/completions`，重点看`stream`、`tool_choice`/function_call
   - 核心认知：LLM 本质是一个不稳定、长耗时的远程服务

## 周二｜本地环境搭建：Ollama + 模型部署（100 分钟）

目标：搭建本地私有大模型服务，脱离外部 API 依赖

1. 10min：安装 Ollama
   - Linux/macOS：`curl -fsSL https://ollama.com/install.sh | sh`
   - Windows：官网下载安装包
2. 20min：本地运行 Llama3:8b
```
ollama run llama3:8b
```
   - 验证：可以正常对话即为成功
3. 30min：Ollama 服务模式
```
ollama serve
```
   - 访问 `http://localhost:11434`，理解 Ollama 是兼容 OpenAI 风格的本地模型网关
4. 40min：编写基础调用脚本
   - 依赖：`pip install openai python-dotenv`
   - 代码：`week1/01_simple_chat.py`，普通一次性调用，打印输入输出 token 数量
   - 产出：可运行的单次对话脚本

## 周三｜Streaming 流式输出（后端核心技能）（100 分钟）

目标：掌握流式输出原理，为后续 SSE 接口做铺垫

1. 20min：Streaming 原理学习
   - 理解：分块 chunk 持续返回，不是一次性返回完整结果；对应后端 SSE 流式推送
2. 60min：编写流式脚本
   - 文件：`week1/02_streaming_chat.py`，开启`stream=True`，逐段打印返回内容
3. 20min：对比测试
   - 对比普通一次性调用 vs 流式调用，记录耗时、token 消耗差异
   - 产出：流式对话 Demo

## 周四｜Function Calling 原理与入门（120 分钟）

> 
> 本周核心：Agent 的基础能力

1. 30min：Function Calling 原理（工程视角）
   - 流程：LLM 输出结构化 JSON 指令 → 后端执行工具 → 工具结果回传给 LLM → LLM 生成最终回答
2. 60min：编写基础工具调用脚本
   - 实现工具：`get_current_time()` 获取当前时间
   - 文件：`week1/03_function_call_basic.py`
3. 30min：观测 token 消耗
   - 观察开启 function calling 后 token 增量开销，记录笔记
   - 产出：基础工具调用 Demo

## 周五｜强化 Function Calling + JSON 结构化输出（120 分钟）

目标：实现稳定 JSON 输出，增强程序鲁棒性，规避 LLM 乱格式问题

1. 30min：学习`response_format`强制 JSON 输出
2. 60min：编写稳定版 Function Calling 脚本
   - 强制 LLM 返回 JSON，增加后端 JSON 解析异常容错处理
   - 文件：`week1/04_function_call_json.py`
3. 30min：整理本周学习笔记
   - 记录：Token、KV Cache、Streaming、Function Calling、Ollama 服务架构
   - 文件：`week1/NOTE.md`
   - 产出：带容错的 function call 脚本 + 完整学习笔记

## 周末｜代码整理 + Github 提交（120 分钟）

目标：规范仓库目录，提交代码，形成可追溯提交记录（面试加分）

1. 30min：整理`week1`目录结构

```
start_ai/
└── week1/
    ├── 01_simple_chat.py
    ├── 02_streaming_chat.py
    ├── 03_function_call_basic.py
    ├── 04_function_call_json.py
    ├── requirements.txt
    └── NOTE.md
```

2. 30min：编写 week1 子目录 README，说明本周目标、环境、运行命令
3. 30min：git 提交
```
git add .
git commit -m "feat(week1): add basic chat & streaming & function call demos"
git commit -m "docs(week1): add llm basics note (token, kv cache, streaming)"
git push
```
4. 30min：Github 网页校验，确认代码、笔记上传成功

## 本周时间汇总表

表格

| 日期 | 任务 | 时长 |
| --- | --- | --- |
| 周一 | LLM 推理基础、Token、API 概念 | 90 分钟 |
| 周二 | Ollama 部署 + 基础对话脚本 | 100 分钟 |
| 周三 | Streaming 流式输出 Demo | 100 分钟 |
| 周四 | Function Calling 基础原理与脚本 | 120 分钟 |
| 周五 | JSON 结构化输出 + 整理笔记 | 120 分钟 |
| 周末 | 代码整理 + 提交 Github | 120 分钟 |

> 
> 总投入：约 9.5 小时

你可以直接复制全部内容，在仓库新建文件命名为 `week1/WEEK1_PLAN.md`。

要不要我继续把 week1 目录下的 4 个 Python 脚本、requirements.txt 和 NOTE.md 一次性生成好，同样做成可直接复制的 markdown 块？
