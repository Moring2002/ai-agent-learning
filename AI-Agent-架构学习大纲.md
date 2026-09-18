# AI Agent 架构工程 · 学习大纲

> **学员画像**
>
> 已有 React / React Native / Flutter 开发经验，JS/TS 已系统强化，Python 基础语法已掌握。
>
> **培养目标**
>
> 从前端工程师进阶到 **AI Agent 应用 / 全栈 / 架构工程师**。
>
> 核心目标不是刷 Python、背 LangChain API，而是建立完整 Agent 系统的架构与机制认知。

---

## 一、学习纪律

### 1. 学习重心

```
架构 / 理论       45%
实验 / 验证       25%
项目              20%
框架 / API        10%
```

### 2. 允许 AI Coding，但不外包思考

可以让 AI：

- 写样板代码
- 补 CRUD
- 生成测试
- 改类型
- 查 API
- 重构重复代码

不能把以下事情直接交给 AI：

- 为什么需要这个组件
- State 应该放哪里
- Agent 下一步如何决定
- Context 为什么这么构造
- Tool 为什么这样设计
- 失败后如何恢复
- 如何证明 Agent 做对了
- 如何控制权限和风险

### 3. 所有“为什么”必须落到机制

禁止：

> “因为框架就是这么设计的。”

要求：

> 谁 → 在什么阶段 → 拿什么 → 做什么 → 产生什么结果 → 下一步谁接着做。

---

# 二、课程总览

| 单元 | 主题 | 定位 |
|---|---|---|
| 0 | Agent 所需的最小后端基础 | 建立服务端 / 状态 / 数据 / 异步空间感 |
| 1 | LLM 原理与 API 心智模型 | 理解模型真正做什么、不做什么 |
| 2 | Context Engineering | 理解模型每一轮到底应该看到什么 |
| 3 | Tool Calling / Skill / MCP | 理解 Agent 如何从“说”变成“做” |
| 4 | RAG 与知识系统 | 理解检索全链路 |
| 5 | Agent Loop / Planning / Workflow | 理解 Agent 的核心决策循环 |
| 6 | Memory / State | 区分任务状态与长期记忆 |
| 7 | Agent Runtime / Reliability | 从 Demo 走向生产系统 |
| 8 | Multi-Agent | 理解何时拆分、如何协作 |
| 9 | Eval / Observability / Security | 能证明 Agent 正确并控制风险 |
| 10 | 毕业项目：Production Agent | 综合验收 |

---

# 三、统一课堂模式

每课固定：

1. **讲授**：概念 + 架构图 + 类比
2. **机制**：解释“为什么”
3. **预测**：运行实验前先写答案
4. **实验**：最小可运行验证
5. **结果**：预测 / 实际 / 原因
6. **病例**：从现象反推机制
7. **复盘**：沉淀一句机制锚点

每课建议目录：

```
exercises/X.Y/
├── practice.md
└── review.md
```

---

# 四、第 0 单元：Agent 所需的最小后端基础

> 目标：不是补齐传统后端课程，而是建立 Agent 运行环境的空间感。

## 0.1 HTTP / API / Service

### 内容

- HTTP Request / Response
- REST
- JSON
- Status Code
- API Contract
- Middleware
- Authentication

### 必须回答

> React 调 FastAPI 时，中间发生了什么？

### 实验

```
React
 ↓ HTTP
FastAPI
 ↓
JSON
 ↓
React
```

---

## 0.2 State / Database / Cache

### 内容

- PostgreSQL
- Redis
- Session
- Cache
- Persistent State

### 核心问题

> 为什么 Agent 的状态不能只放 Python 变量里？

---

## 0.3 Async / Queue / SSE

### 内容

- async / await
- Worker
- Queue
- SSE
- WebSocket
- Polling
- Long-running task

### 核心问题

> 一个 Agent 跑 30 秒，为什么不能简单按普通 CRUD 请求处理？

---

## 0.4 最小后端实验

实现：

```
React
 ↓
FastAPI
 ↓
PostgreSQL
 ↓
Redis
```

验收：

> 能完整讲清一个请求从前端到数据库再返回的链路。

---

# 五、第 1 单元：LLM 原理与 API 心智模型

## 1.1 LLM 到底在做什么

### 内容

```
Token
 ↓
Embedding
 ↓
Transformer
 ↓
Next Token Prediction
 ↓
Output
```

重点理解：

- Token
- Context Window
- Attention
- Sampling
- Structured Output

### 必须回答

> 模型为什么不知道数据库里的实时数据？

---

## 1.2 Temperature / Sampling / Context

实验：

同一问题使用不同 temperature，对比稳定性。

再测试短、长、超长 Context。

必须回答：

> Context Window 为什么是架构约束？

---

## 1.3 Structured Output

```
自然语言
 ↓
Structured Output
 ↓
结构化对象
```

必须回答：

> Structured Output 与 Tool Calling 的关系是什么？

---

## 1.4 Model Routing

```
简单任务 → 便宜模型
复杂推理 → 强模型
Embedding → Embedding Model
```

问题：

> 为什么一个 Agent 系统通常不应该只依赖一个模型？

---

# 六、第 2 单元：Context Engineering

> ⭐ 核心单元

## 2.1 Context 的组成

```
System Prompt
+
User Input
+
Conversation
+
Memory
+
Retrieved Documents
+
Tool Result
+
Current State
+
Available Tools
```

必须理解每一项从哪里来。

---

## 2.2 Context Selection

实验：

比较：

- 全量历史
- 最近 N 条
- Summary
- Relevant Retrieval

观察结果变化。

---

## 2.3 Context Compression

```
10000 tokens
 ↓
Summary
 ↓
1500 tokens
```

必须回答：

> 什么可以丢？谁决定丢什么？

---

## 2.4 Context Poisoning

实验：

给 Agent 错误历史、错误 Tool Result、恶意检索内容。

分析：

> 错误信息进入 Context 后，Agent 的行为链如何被污染？

---

# 七、第 3 单元：Tool Calling / Skill / MCP

> ⭐ 核心单元

## 3.1 Tool Calling

完整链路：

```
User
 ↓
LLM
 ↓
Tool Selection
 ↓
Tool Schema
 ↓
Arguments
 ↓
Permission
 ↓
Execution
 ↓
Result
 ↓
LLM
```

---

## 3.2 Tool Schema

理解：

- name
- description
- parameters
- return schema
- error schema

核心问题：

> Tool Description 为什么会直接影响 Agent 决策？

---

## 3.3 Tool Registry

从：

```
if tool == "search"
if tool == "weather"
if tool == "sql"
```

升级到：

```
Tool Registry
├── Search
├── Weather
├── SQL
├── GitHub
└── Browser
```

必须回答：

> 为什么 Tool 必须从 Agent Core 解耦？

---

## 3.4 Skill

理解：

> Tool = 原子动作

> Skill = 完成某类任务所需的一组能力

例：

```
Code Review Skill
├── GitHub Search
├── Read File
├── Run Test
└── Create Comment
```

---

## 3.5 MCP

重点：

```
Agent
 ↓
MCP Client
 ↓
MCP Server
 ↓
Tools / Resources / Prompts
 ↓
External System
```

必须回答：

> MCP 主要解决的工程问题是什么？

---

# 八、第 4 单元：RAG 与知识系统

> ⭐ 核心单元

## 4.1 Indexing

```
Document
 ↓
Parsing
 ↓
Chunking
 ↓
Metadata
 ↓
Embedding
 ↓
Index
```

实验：

比较不同 Chunk 策略对召回的影响。

---

## 4.2 Retrieval

```
Query
 ↓
Recall
 ↓
Filter
 ↓
Rerank
 ↓
Top K
```

必须理解：

- Vector Search
- Keyword Search
- Hybrid Search
- Metadata Filter
- Reranker

---

## 4.3 RAG Failure Analysis

制造：

- 召回不到
- 召回错误
- 召回太多
- 召回正确但模型没有使用

每种情况回答：

> 错误发生在哪一环？

---

## 4.4 Agentic RAG

比较：

```
普通 RAG：
Question → Retrieve → Answer
```

与：

```
Agentic RAG：
Question
 ↓
Agent
 ↓
决定搜索什么
 ↓
Search
 ↓
发现不足
 ↓
再次 Search / SQL / Tool
 ↓
Answer
```

核心问题：

> 什么时候普通 RAG 就够？什么时候应该引入 Agentic RAG？

---

# 九、第 5 单元：Agent Loop / Planning / Workflow

> ⭐⭐⭐⭐⭐ 整套体系的核心

## 5.1 Agent Loop

```
Goal
 ↓
Observe
 ↓
Reason
 ↓
Plan
 ↓
Act
 ↓
Observe Result
 ↓
Replan
 ↓
Act
 ↓
...
 ↓
Finish
```

必须回答：

> Agent 与 Chatbot 的本质区别是什么？

---

## 5.2 ReAct

```
Thought
 ↓
Action
 ↓
Observation
 ↓
Thought
```

必须回答：

> ReAct 解决什么问题？

---

## 5.3 Planning

学习：

- Task Decomposition
- Plan-and-Execute
- Reflection
- Replanning
- Long-horizon Planning

实验：

让 Agent 执行一个 5~8 步任务，并人为让中间 Tool 失败。

观察：

> 停止？重试？换 Tool？还是重新规划？

---

## 5.4 Workflow vs Agent

确定路径：

```
A → B → C → D
```

适合 Workflow。

不确定下一步：

```
Goal
 ↓
Agent 决策
 ↓
Tool
 ↓
Observation
 ↓
继续决策
```

适合 Agent。

必须回答：

> 为什么很多所谓 Agent 实际上更适合 Workflow？

---

# 十、第 6 单元：Memory / State

> ⭐ 核心单元

## 6.1 State ≠ Memory

### State

当前任务正在发生什么。

### Memory

系统过去知道什么。

---

## 6.2 Short-term Memory

- Conversation
- Current Task
- Recent Tool Results

---

## 6.3 Long-term Memory

- User Profile
- Semantic Memory
- Episodic Memory
- Procedural Memory

---

## 6.4 Memory Lifecycle

```
Conversation
 ↓
Memory Extraction
 ↓
Memory Write
 ↓
Memory Index
 ↓
Future Retrieval
 ↓
Context
```

核心问题：

> 什么值得存？谁决定存？存错了怎么处理？

---

# 十一、第 7 单元：Agent Runtime / Reliability

> ⭐ 从 Demo 到生产系统的分界点

## 7.1 Session / State

```
User
 ↓
Session
 ↓
Agent Run
 ↓
Task State
```

---

## 7.2 Checkpoint / Resume

设计实验：

任务：

```
1 → 2 → 3 → 4 → 5
```

执行到 3 时故障。

恢复后：

> 应该从 3 重新执行，还是从 4 继续？

---

## 7.3 Durable Execution

理解：

- Queue
- Worker
- Checkpoint
- Persistence
- Resume
- Event

---

## 7.4 Retry / Timeout / Idempotency

制造：

1. Tool 偶发失败
2. Tool 超时
3. Tool 已成功但 Response 丢失

重点回答：

> 为什么第 3 种情况 Retry 最危险？

---

## 7.5 Sandbox

当 Agent 能：

- 执行代码
- 访问文件
- 操作浏览器
- 调用外部 API

必须理解：

- Sandbox
- Permission
- Resource Limit
- Credential Isolation

---

# 十二、第 8 单元：Multi-Agent

## 8.1 Supervisor

```
Supervisor
├── Researcher
├── Analyst
└── Writer
```

---

## 8.2 Handoff

理解：

> Agent A 为什么把任务交给 Agent B？

---

## 8.3 Shared State vs Independent Context

实验：

比较：

- 全共享 Context
- 最小共享 State
- 独立 Context

分析信息污染和成本。

---

## 8.4 Parallel Agent

```
Supervisor
├── Search A
├── Search B
└── Search C
       ↓
      Merge
```

必须回答：

> 哪些任务可以并行？

---

# 十三、第 9 单元：Eval / Observability / Security

> ⭐⭐⭐⭐⭐

## 9.1 Eval

建立：

```
Input
 ↓
Expected Behavior
 ↓
Actual Trace
 ↓
Evaluation
```

指标：

- Task Success Rate
- Tool Accuracy
- RAG Recall
- Groundedness
- Hallucination
- Latency
- Cost

---

## 9.2 Trace

一次 Agent Run 应能看到：

```
Run
├── LLM Call
├── Retrieval
├── Memory
├── Tool Call
├── Tool Result
├── Retry
└── Final Answer
```

验收：

> 一个问题失败后，能否顺着 Trace 找到死在哪一步？

---

## 9.3 Regression Dataset

建立固定数据集：

```
Question
Expected Tool
Expected Behavior
Expected Result
```

修改 Agent 后自动回归。

---

## 9.4 Security

重点：

- Prompt Injection
- Tool Abuse
- Data Leakage
- Privilege Escalation
- Credential Leakage
- Sandbox Escape

实验：

恶意文档 → RAG → Agent

观察：

> 文档中的指令是否能改变 Agent 行为？

---

## 9.5 Human-in-the-loop

```
低风险
 ↓
自动执行

高风险
 ↓
人工确认
 ↓
执行
```

必须能设计风险边界。

---

# 十四、第 10 单元：毕业项目 · AI Codebase Agent

> 最终项目使用 GitHub Codebase 作为真实对象。

## 第一阶段

```
React
 ↓
FastAPI
 ↓
LLM
```

用户可以询问：

> “这个项目是干什么的？”

---

## 第二阶段：RAG

```
Repository
 ↓
Code / README
 ↓
Chunk
 ↓
Embedding
 ↓
Vector DB
 ↓
Retrieval
```

用户可以问：

> “7.1 的 Fiber 为什么能保存 State？”

---

## 第三阶段：GitHub Tools

```
List Files
Read File
Search Code
Read Commit
Read PR
```

Agent 可以动态查仓库。

---

## 第四阶段：Planning

例如：

> “分析这个项目为什么越来越难维护。”

Agent 自动拆解：

```
1. 阅读目录
2. 分析模块
3. 检查依赖
4. 查看重复逻辑
5. 查看提交历史
6. 汇总结论
```

---

## 第五阶段：Memory

记住：

- 用户技术栈
- 用户学习目标
- 用户项目偏好

同时提供：

- Memory Retrieval
- Memory Update
- Memory Delete

---

## 第六阶段：Runtime

增加：

- Session
- State
- Checkpoint
- Retry
- Timeout
- Queue

实现长任务恢复。

---

## 第七阶段：Eval

建立 100 个问题的回归集，统计：

- Task Success
- Tool Accuracy
- RAG Accuracy
- Latency
- Cost

---

# 十五、最终系统架构

```
                         React
                           │
                           ▼
                    API / SSE Gateway
                           │
                           ▼
                    Agent Controller
                           │
        ┌──────────────────┼─────────────────┐
        ▼                  ▼                 ▼
     Context            Memory            State
        │                  │                 │
        └──────────────────┼─────────────────┘
                           ▼
                        Planner
                           │
                  ┌────────┼────────┐
                  ▼        ▼        ▼
                 RAG      Tool     MCP
                  │        │        │
                  └────────┼────────┘
                           ▼
                    Agent Runtime
                           │
             ┌─────────────┼─────────────┐
             ▼             ▼             ▼
          Queue         Checkpoint     Sandbox
             │
             ▼
          Execution
             │
             ▼
       Trace / Evaluation
             │
             ▼
      Feedback / Regression
```

---

# 十六、毕业验收标准

不是：

> “会 LangGraph。”

而是能够拿到一个 Agent JD 后，看到：

```
Memory
RAG
Tool Calling
MCP
Workflow
Runtime
Eval
Sandbox
Multi-Agent
```

然后自动解释：

```
为什么存在
↓
解决什么问题
↓
和谁连接
↓
状态在哪里
↓
决策在哪里
↓
执行在哪里
↓
失败在哪里
↓
如何恢复
↓
如何评估
↓
如何控制权限
```

达到这个程度，才算进入 AI Agent 工程 / 架构方向。
