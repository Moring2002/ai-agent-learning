# AI Agent 架构工程 · 学习仓库

面向已经具备 React / React Native / Flutter / JS / TS 基础，希望转向 AI Agent 应用与平台工程的系统学习仓库。

## 学习目标

不是把框架 API 背熟，而是建立完整的 Agent 系统心智模型：

```
LLM
 ↓
Context
 ↓
Planning
 ↓
Tool / Skill / MCP
 ↓
RAG / Memory
 ↓
Workflow / Agent Loop
 ↓
Runtime
 ↓
Eval / Observability
 ↓
Security / Reliability
 ↓
Production System
```

## 学习方式

沿用 `react-native-learning` 的方式：

**讲概念 → 解释机制 → 先预测 → 做实验 → 验证 → 病例诊断 → 复盘 → 周测**

编码允许使用 AI Coding 辅助；架构判断、机制解释、故障定位必须自己掌握。

## 仓库结构

- `AI-Agent-架构学习大纲.md`：课程主线
- `ENVIRONMENT.md`：开发环境与自检要求
- `COURSE-STATE.md`：当前进度
- `STUDY-LOG.md`：学习打卡
- `exercises/`：每课实验与作业
- `projects/`：综合项目
- `reviews/`：周测与复盘

## 当前主线

**第 0 单元：Agent 所需的最小后端基础**

先建立 HTTP / Service / State / Async 的空间感，然后进入 LLM、Context、Tool、RAG、Agent Core、Runtime、Eval 和 Security。

## 最终项目

**AI Codebase Agent**

以 GitHub Repository 作为真实对象，逐步加入：

React → FastAPI → LLM → RAG → GitHub Tools → Planning → Memory → Runtime → Eval

## 环境

正式开始前请先查看 **[ENVIRONMENT.md](./ENVIRONMENT.md)**。

课程环境原则：能跑实验即可，不提前安装大量 Agent 框架；学习重点放在系统架构与机制。