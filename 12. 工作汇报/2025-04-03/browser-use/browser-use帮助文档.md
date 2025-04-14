
**概述**

  

Agent 类是 Browser Use 中处理浏览器自动化的核心组件。通过配置不同参数，你可以定制代理的行为，使其能够根据任务需求进行网页操作、视觉处理以及日志记录等功能。

  

**基本设置**

下面是一个简单的示例，用于初始化代理：
```
from browser_use import Agent
from langchain_openai import ChatOpenAI

agent = Agent(
    task="Search for latest news about AI",
    llm=ChatOpenAI(model="gpt-4o"),
)
```
