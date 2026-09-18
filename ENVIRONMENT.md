# AI Agent 学习环境要求

> 本文档是环境配置的唯一参考。目标是：**能启动课程、能运行实验即可**，不要为了学习 Agent 提前安装大量框架和基础设施。

## 一、基础开发环境

### 必需

| 工具 | 要求 | 用途 |
|---|---|---|
| Git | 最新稳定版 | 版本管理、分支、PR |
| Cursor / VS Code | 最新稳定版 | 开发与实验 |
| Chrome | 最新稳定版 | DevTools、Network、SSE 调试 |
| Node.js | 22 LTS | React / Web 前端 |
| Python | 3.13.x | Agent / FastAPI 后端 |
| uv | 最新稳定版 | Python 版本、虚拟环境、依赖管理 |
| Docker Desktop | 最新稳定版 | PostgreSQL、Redis 及后续基础设施 |

### Windows 推荐

如果使用 Windows：

```text
Windows
 ├── Cursor / VS Code
 ├── Chrome
 ├── Node 22 LTS
 ├── Git
 ├── Python 3.13
 └── WSL2
      └── Ubuntu
           └── Docker
```

Docker Desktop 使用 WSL 2 后端。

## 二、第一阶段需要的运行组件

课程开始时只需要：

```text
React
  ↓
FastAPI
  ↓
PostgreSQL
  ↓
Redis
```

### Python

推荐使用 uv：

```bash
uv python install 3.13
uv python pin 3.13
```

创建课程实验项目：

```bash
uv init
uv add fastapi uvicorn
```

启动：

```bash
uv run uvicorn main:app --reload
```

### Node

检查：

```bash
node -v
npm -v
```

Node 22 LTS 即可。

### Docker

检查：

```bash
docker --version
docker compose version
```

数据库与 Redis 优先使用 Docker Compose，不要求直接安装到宿主机。

## 三、模型 API

需要准备一个可以调用的 **OpenAI-compatible LLM API**。

建议通过环境变量配置：

```env
LLM_API_KEY=...
LLM_BASE_URL=...
LLM_MODEL=...
```

项目中的密钥只放在本地 `.env`，**禁止提交到 GitHub**。

推荐后续通过一个 Model Gateway / Provider Adapter 隔离模型供应商：

```text
Agent
 ↓
Model Gateway
 ↓
LLM Provider
```

这样更换模型时不用修改 Agent 核心逻辑。

## 四、调试工具

### Chrome DevTools

重点使用：

- Console
- Network
- Request / Response
- Headers
- Timing
- SSE / Streaming
- Application / Storage

### API 调试

推荐 Apifox 或 Postman。

不过不是硬依赖；curl、浏览器 DevTools 也可以完成大部分实验。

### 数据库

推荐 DBeaver，用于观察 PostgreSQL。

后续 Redis 可使用 Redis Insight。

## 五、暂时不要提前安装

在进入对应课程之前，不需要主动安装：

```text
LangChain
LangGraph
CrewAI
AutoGen
Milvus
Elasticsearch
Kafka
Kubernetes
vLLM
PyTorch
```

原因：

> 本课程首先学习 **为什么需要这些组件**，再学习具体实现。

避免先被某一个框架的 API 绑住。

## 六、环境自检

在正式上课前运行：

```bash
git --version
node -v
npm -v
python --version
uv --version
docker --version
docker compose version
```

理想结果：

```text
Git       ✅
Node      22.x
Python    3.13.x
uv        ✅
Docker    ✅
Compose   ✅
```

## 七、课程开始时的最小验收

先不做复杂 Agent。

只需要跑通：

```text
React
  ↓ HTTP
FastAPI
  ↓ JSON
React
```

然后再依次加入：

```text
LLM
 ↓
Context
 ↓
Tool
 ↓
RAG
 ↓
Agent
 ↓
Memory
 ↓
Runtime
 ↓
Eval / Security
```

## 八、环境原则

**环境够用即可，学习重点放在架构。**

本课程建议的学习时间分配：

```text
架构 / 理论       45%
实验 / 验证       25%
项目              20%
框架 / API        10%
```
