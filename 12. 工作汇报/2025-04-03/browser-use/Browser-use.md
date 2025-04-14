
## 参考地址

- [Browser-use](https://docs.browser-use.com/customize/real-browser)
- [PlayWright](https://playwright.dev/docs/intro)

### **Playwright 简介**

#### **开发背景**

Playwright 是微软于 2020 年初推出的强大自动化工具，其核心愿景是提供一个统一、优雅的 API 来实现跨浏览器的自动化测试和网页操作。它汲取了 Puppeteer 等前代工具的精华，同时创新性地解决了自动化测试中的不稳定性（flaky tests）问题，为开发者提供更可靠的自动化体验。

#### **主要特点**

- **全面的跨浏览器支持**：无缝支持 Chromium（Chrome/Edge）、Firefox 和 WebKit（Safari）三大引擎，确保应用在不同浏览器环境中的一致性表现。
    
- **强大的跨平台与多语言生态**：完美运行于 Windows、macOS 和 Linux 各主流操作系统，并为 JavaScript/TypeScript、Python、C# 和 Java 开发者提供原生 API，满足多样化的技术栈需求。
    
- **智能的自动等待机制**：内置智能自动等待功能，精确识别页面元素的可交互状态，有效消除了因页面动态加载导致的测试不稳定性，大幅提升自动化脚本的可靠性。
    
- **高效的多浏览器上下文管理**：支持在单一浏览器实例中创建多个完全隔离的上下文环境，实现真正的并行测试和精确的会话隔离，显著提升测试效率。


#### **使用场景**

Playwright 广泛应用于端到端测试、网页自动化操作、数据抓取与分析，以及精确模拟真实用户交互行为等场景，为现代 Web 应用的质量保障和自动化流程提供了强有力的技术支持。

### **browser-use 简介**

#### **定义与定位**

browser-use 是一个基于 Python 开发的开源库，旨在将 AI 智能代理与真实浏览器自动化操作无缝结合。它在 Playwright 强大功能的基础上进行了进一步封装，提供更友好的接口，使开发者能够通过自然语言描述任务，由 AI 自动生成并执行相应的浏览器操作脚本。

#### **主要特点**

- **自然语言交互**：用户只需提供简单的任务描述（如"打开某网站、抓取信息、点击按钮"等），内置的大型语言模型（如 GPT-4、DeepSeek 等）便会智能解析并生成自动化操作流程。
- **无头浏览器支持**：完全支持无头模式运行，使其能够在无图形界面的服务器环境中高效部署和运行，特别适合批量数据抓取和后台自动化任务。
- **增强的浏览器管理**：提供多标签页操作、持久化会话以及与本地浏览器的直接集成能力，免除重复登录和复杂环境配置的困扰。
- **智能错误处理与日志追踪**：内置自动重试机制和详细的操作日志记录功能，显著提升任务执行的稳定性和可调试性。
- **多模型支持**：不仅支持 OpenAI 的模型，还兼容其他 LLM，如 Anthropic、Ollama、DeepSeek 等，灵活满足不同需求和成本控制策略。
#### **使用场景**

browser-use 特别适合构建能够自主执行网页操作的 AI 代理，广泛应用于网页数据爬取、自动化测试、信息抽取、任务流程自动化等领域。通过无头浏览器模式，它可以在服务器环境中高效运行大规模自动化任务，无需图形界面支持，显著降低部署难度、人工操作成本和技术门槛。

![[file-20250403095144688.png]]
## 实战

环境配置
```
# 创建并激活专用 Python 3.11 虚拟环境
conda create --name python3.11 python=3.11
conda activate python3.11

# 安装必要依赖库
pip install langchain    # AI 大语言模型交互框架
pip install playwright   # 跨浏览器自动化工具
pip install browser-use  # AI 驱动的浏览器自动化库
```
除了通过 pip 安装 Playwright Python 包之外，还需要运行 `playwright install`

**Playwright** 需要两部分组件才能正常工作：
1. Python API 包（通过 `pip install playwright` 安装）
2. 浏览器二进制文件（通过 `playwright install` 命令安装）
![[file-20250401134823047.png]]
```
playwright install
```

# Browser-use 使用指南

根据您提供的代码示例，以下是 browser-use 的基本使用步骤：

1. **环境配置**：
    
    - 安装必要依赖（playwright、browser-use）
    - 设置环境变量（如API密钥）
    - 使用 dotenv 加载环境变量
2. **浏览器配置**：
    
    ```python
    browser = Browser(
        config=BrowserConfig(
            headless=False,  # 设置为True启用无头模式
        )
    )
    ```

无头浏览器(Headless Browser)是指可以在没有图形用户界面(GUI)的环境中运行的网页浏览器。
- "无头"意味着这种浏览器没有可视化的界面，您看不到它渲染的页面
- 它在后台运行，通过编程方式控制
- 它可以完成普通浏览器的大部分功能，如加载网页、执行JavaScript、处理cookies、填写表单等
- 它可以访问DOM(文档对象模型)并进行交互
无头浏览器特别适用于：
- 网页抓取和数据采集
- 自动化测试
- 性能监控
- 在服务器等无GUI环境中运行的自动化任务

1. **初始化AI-Agent**：
    ```python
    agent = Agent(
        task="详细的任务描述...",  # 用自然语言描述爬取任务
        llm=ChatOpenAI(...),     # 指定使用的语言模型
        use_vision=True,         # 启用视觉功能帮助识别网页元素
        browser=browser,         # 传入配置好的浏览器实例
    )
    ```
browser-use基于langchain工作，所以它支持langchain支持的大多数LLM提供商。

| LLM提供方    | 基础URL                                             | 模型名称                                           |
| --------- | ------------------------------------------------- | ---------------------------------------------- |
| OpenAI    | https://api.openai.com/v1                         | gpt-4, gpt-4o                                  |
| Anthropic | https://api.anthropic.com                         | claude-3-opus, claude-3-sonnet, claude-3-haiku |
| Qwen (阿里) | https://dashscope.aliyuncs.com/compatible-mode/v1 | qwen-plus, qwen-max, qwen-turbo                |
| Google    | https://generativelanguage.googleapis.com         | gemini-1.0-pro, gemini-1.5-pro                 |
| DeepSeek  | https://api.deepseek.com                          | deepseek-chat, deepseek-coder                  |
1. **执行爬取任务**：
    ```python
    result = await agent.run()  # 异步执行
    ```    
2. **错误处理**：
    
    - 添加超时机制防止无限执行
    - 捕获可能的异常
    - 验证返回结果
3. **任务描述技巧**：
    
    - 使用清晰的步骤编号
    - 明确指定要访问的URL
    - 详细描述需要提取的数据
    - 指定返回数据的格式（如JSON）
    - 说明如何处理特殊情况或错误



![[file-20250402111716385.png]]
![[file-20250402112914138.png]]

[^1]: 
