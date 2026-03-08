# AI Agent 智能体

> 从对话到行动：AI Agent 架构与实践

---

## 🤖 什么是 AI Agent？

### 定义

AI Agent 是能够**感知环境、自主决策、执行动作**的智能系统。

### 与传统 LLM 的区别

| 特性 | 传统 LLM | AI Agent |
|------|----------|----------|
| **交互方式** | 对话 | 任务闭环 |
| **工具使用** | 无 | 可调用工具 |
| **记忆能力** | 短期 | 长期记忆 |
| **规划能力** | 无 | 多步规划 |
| **自主性** | 被动响应 | 主动执行 |

---

## 🏗️ Agent 架构

```
┌─────────────────────────────────────────┐
│           感知 (Perception)              │
│  接收用户输入、环境信息                  │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│           规划 (Planning)                │
│  - 任务分解                             │
│  - 策略制定                             │
│  - 反思与调整                           │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│           记忆 (Memory)                  │
│  - 短期记忆 (上下文)                     │
│  - 长期记忆 (向量数据库)                 │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│           工具使用 (Tool Use)            │
│  - API 调用                             │
│  - 代码执行                             │
│  - 外部系统交互                         │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│           行动 (Action)                  │
│  执行具体操作                           │
└─────────────────────────────────────────┘
```

---

## 🛠️ 核心组件

### 1. ReAct 框架

**Reasoning + Acting**

```python
# ReAct 循环
while not task_completed:
    # 思考
    thought = llm.predict(
        f"Task: {task}\\n"
        f"Observation: {observation}\\n"
        f"Thought:"
    )
    
    # 行动
    action = parse_action(thought)
    observation = execute_action(action)
```

**示例**:
```
Question: 北京今天天气如何？

Thought 1: 我需要查询北京的天气信息。
Action 1: search_weather(location="北京")
Observation 1: 北京今天晴，25°C

Thought 2: 我已经获得了天气信息，可以回答用户。
Action 2: finish(answer="北京今天晴天，气温25°C")
```

### 2. 工具定义

```python
from langchain.tools import tool

@tool
def search_weather(location: str) -> str:
    """查询指定地区的天气"""
    # 调用天气 API
    return f"{location}今天晴，25°C"

@tool
def calculator(expression: str) -> str:
    """计算数学表达式"""
    return str(eval(expression))

tools = [search_weather, calculator]
```

### 3. 记忆系统

```python
from langchain.memory import VectorStoreRetrieverMemory

# 长期记忆
memory = VectorStoreRetrieverMemory(
    retriever=vectorstore.as_retriever()
)

# 保存交互
memory.save_context(
    {"input": "我叫张三"},
    {"output": "你好张三！"}
)

# 检索相关记忆
relevant_memories = memory.load_memory_variables(
    {"input": "张三喜欢什么？"}
)
```

---

## 📋 Agent 类型

### 1. 单 Agent 架构

```python
from langchain.agents import initialize_agent

agent = initialize_agent(
    tools,
    llm,
    agent="zero-shot-react-description",
    verbose=True
)

agent.run("查询北京天气并计算25度华氏度是多少")
```

### 2. 多 Agent 协作

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Planner   │────→│  Executor   │────→│   Critic    │
│   Agent     │     │   Agent     │     │   Agent     │
└─────────────┘     └─────────────┘     └─────────────┘
       ↑                                      │
       └──────────────────────────────────────┘
```

### 3. Agent 工作流

```python
from langchain import hub
from langchain.agents import create_openai_functions_agent

# 获取预定义 Prompt
prompt = hub.pull("hwchase17/openai-functions-agent")

# 创建 Agent
agent = create_openai_functions_agent(
    llm, tools, prompt
)
```

---

## 🏢 企业级 Agent 应用

### 智能客服 Agent

```
用户: 我的订单什么时候到？

Agent 思考:
1. 需要查询订单信息
2. 需要调用物流接口
3. 需要生成回复

Agent 行动:
1. query_order(order_id="12345")
2. query_logistics(tracking_id="ABC123")
3. generate_response()

回复: 您的订单12345已发货，预计明天送达。
```

### 代码助手 Agent

```python
# 能力
- 理解需求
- 编写代码
- 执行测试
- 修复 Bug
- 部署上线

# 工具
- code_writer: 生成代码
- code_executor: 执行代码  
- test_runner: 运行测试
- git_tool: 版本控制
```

---

## 🔥 热门 Agent 框架

| 框架 | 特点 | 适用场景 |
|------|------|----------|
| **LangChain** | 生态完善 | 通用开发 |
| **AutoGPT** | 自主性强 | 自动化任务 |
| **MetaGPT** | 多 Agent 协作 | 软件开发 |
| **CrewAI** | 角色扮演 | 团队协作 |
| **Dify** | 可视化 | 快速搭建 |

---

## 💡 最佳实践

### 1. 工具设计原则

- **原子性**: 每个工具只做一件事
- **明确性**: 描述清晰，LLM 知道何时使用
- **容错性**: 处理异常情况

### 2. 提示工程

```python
SYSTEM_PROMPT = """你是一个智能助手，可以使用以下工具：

{tools}

请按照以下格式思考：
Thought: 我需要做什么
Action: 工具名称
Action Input: 工具参数

重要原则：
1. 如果不确定，先询问用户
2. 如果工具调用失败，尝试其他方法
3. 始终保护用户隐私
"""
```

### 3. 安全与限制

- **权限控制**: 限制工具访问范围
- **人工审核**: 关键操作需要确认
- **审计日志**: 记录所有操作

---

## 🔗 相关资源

- [LangChain Agents](https://python.langchain.com/docs/modules/agents/)
- [AutoGPT GitHub](https://github.com/Significant-Gravitas/AutoGPT)
- [ReAct Paper](https://arxiv.org/abs/2210.03629)

---

*最后更新: 2026-03-08*
