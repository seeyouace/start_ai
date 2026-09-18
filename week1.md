# 周一任务：Token 基础（15 分钟学习材料）

> 
> 文件建议保存到：`week1/materials/token_basic.md`，直接复制到仓库
> 学习目标：搞懂 Token 是什么、怎么算、上下文窗口、中文 / 英文 token 换算，工程层面的影响，**不深入 BPE 算法底层**（面试后端 Agent 岗位不需要）

## 1. Token 是什么

Token 是大模型处理文本的**最小处理单元**。
不是字，也不是单词，是模型预训练阶段用 BPE（字节对编码）切分出来的片段：

- 英文：经常是完整单词，长单词会拆成片段
- 中文：大多是**单个汉字，少数常用两字词合并**

> 
> 经验估算：**1 个中文汉字 ≈ 1~2 token；1000token ≈ 750 英文单词，≈500 中文汉字**

> 
> 例子：
> `人工智能` → 可能被切为 `人工`、`智能` 两个 token；
> 长英文单词 `unhappiness` → 拆成 `un` + `happiness`

## 2. 为什么 LLM 按 Token 计费，不是按字符

模型推理时，**每个 token 都会走一次 Transformer 计算**，KV Cache 也是按 token 数量占用显存。
token 越多：

1. 显存占用越高
2. 推理速度越慢
3. 费用越高（OpenAI 这类 API）
4. 更容易打满上下文窗口上限

## 3. 上下文窗口（Context Window）

> 
> 非常核心，Agent/RAG 工程第一大坑
> 上下文窗口 = **输入 token（prompt + 用户问题 + 历史对话 + 检索文档） + 输出 token（模型回答）的总和上限**

举例：Llama3 8B 常见版本上下文窗口 8192 tokens

- 一旦输入 + 输出总和超过窗口上限：
  - 旧信息直接截断丢弃；
  - 或者直接报错；
  - 模型会丢失前面的对话、知识库内容，产生幻觉。

> 
> 工程要点：
> 做长对话、RAG、Agent，**必须监控 token 数量，做上下文裁剪 / 摘要压缩**，否则系统不稳定。

## 4. 工具：如何计算 token 数量

OpenAI 官方工具：`tiktoken` Python 库，用来对文本做分词、统计 token。

> 
> 后面周二写代码会用到。

简单示例代码（先看懂，不用现在跑）

```
import tiktoken

# 选择编码，llama3可以近似用gpt-4编码做估算
enc = tiktoken.encoding_for_model("gpt-4")
text = "你好，什么是token？"
tokens = enc.encode(text)
print(f"token数量：{len(tokens)}")
```

⚠️ 注意：不同模型分词器不一样！tiktoken 是 OpenAI 的，Llama3 有自己的分词器，**只能做估算**。Ollama 接口返回的 usage 字段会返回真实 input/output token，优先读接口返回值。

## 5. 面试常问总结（写进你的 NOTE.md）

> 
> 可直接复制到`week1/NOTE.md`

1. Token 是模型文本处理最小单元，BPE 分词，中文 1 字≈1~2token；
2. 上下文窗口是输入 + 输出 token 总和上限，超限会截断信息；
3. Token 决定显存、推理速度、成本；
4. 工程实践：实时统计 token，长对话做上下文压缩，防止超出窗口。

## 6. 参考阅读链接（快速浏览）

1. OpenAI 官方介绍 Token：https://platform.openai.com/docs/guides/tokenization

> 
> 重点看：How tokens work 章节，不用看全部

2. OpenAI Token 计算器网页（可视化体验）：[https://platform.openai.com/tokenizer](https://platform.openai.com/tokenizer)

> 
> 强烈推荐，粘贴中文、英文，实时看文本被切分成哪些 token，直观感受。

## ✅ 任务验收（完成这一步）

1. 打开上面的 tokenizer 网页，粘贴一段中文，观察分词效果；
2. 在笔记里写下一句话总结 Token；
3. 回答 2 个自测问题：
   - Q1：为什么不能直接用字符长度代替 token 数量？
   - Q2：上下文窗口超限会发生什么？

你完成之后，我们继续下一小节：推理核心机制（Streaming、KV Cache）。
