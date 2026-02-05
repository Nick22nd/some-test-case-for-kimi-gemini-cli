# Kimi CLI 任务指令用例

本文档包含针对 Kimi CLI 仓库 (https://github.com/MoonshotAI/kimi-cli) 的任务指令用例。每个任务按照指令分解，方便用户复制粘贴逐步执行。

---

## Task 1: 添加 /analyze 指令

**需求**：分析当前项目的代码结构并生成报告。

### 指令1：
```
我需要为当前的 Kimi CLI 项目添加一个新功能 /analyze。请先执行以下操作：

扫描代码库，找到斜杠命令注册的位置（应该在 src/kimi_cli/soul/slash.py）。

查看现有的斜杠命令实现，了解命令注册和执行的流程。

找到文件操作相关的工具函数，特别是文件读取和目录遍历的功能。

确认项目使用的代码分析或 AST 解析库（如果有的话）。

向我汇报斜杠命令的注册方式、文件操作工具的位置以及 /analyze 命令应该插入的代码位置。
```

### 指令2：
```
现在请创建代码分析功能的辅助函数 analyze_project_structure()。要求如下：

函数参数：接受项目根目录路径、分析深度（默认全部分析）、包含/排除的文件模式等参数。

分析逻辑：
- 遍历项目目录，收集所有源代码文件（按扩展名过滤）
- 分析项目结构（目录层级、模块组织）
- 统计代码行数、函数数量、类数量等指标
- 识别项目类型（如 Python、TypeScript、React 等）
- 分析依赖关系（如果可能）

输出格式：返回一个结构化的报告，包含项目概览、文件结构统计、代码质量指标等信息。
```

### 指令3：
```
接下来请在 src/kimi_cli/soul/slash.py 中使用 @registry.command 装饰器注册 /analyze 斜杠命令：

使用 @registry.command 装饰器定义命令，支持别名如 /analyze 或 /a。

在命令处理函数中接收 soul 和 args 参数。

解析用户传入的参数（如路径、深度、输出格式等）。

调用 analyze_project_structure() 函数执行分析。

将分析结果格式化为易读的文本，使用表格、列表等方式展示。

考虑添加 --save 选项，将分析报告保存为文件。
```

### 指令4：
```
最后，测试新添加的 /analyze 命令：

在 Kimi CLI 的交互模式中输入 /analyze 验证命令是否正常注册。

使用不同参数测试命令，如 /analyze --depth=2、/analyze --save=report.md 等。

检查分析报告的内容是否准确、格式是否清晰。

确认错误处理是否完善（如目录不存在、权限不足等情况）。

如果有任何问题，请修复并重新测试。
```

---

## Task 2: 添加 /refactor 指令

**需求**：对指定文件进行代码重构建议。

### 指令1：
```
我需要为 Kimi CLI 添加一个 /refactor 指令，用于提供代码重构建议。请先执行以下操作：

扫描代码库，找到文件操作相关的工具（位于 src/kimi_cli/tools/file/）。

查看现有的文件读取和解析工具（如 read.py、utils.py）。

确认项目是否有代码分析或 AST 解析的功能。

找到斜杠命令注册的位置（src/kimi_cli/soul/slash.py）。

查看其他斜杠命令的实现作为参考。

向我汇报文件操作工具的使用方式、代码分析能力以及 /refactor 命令应该插入的位置。
```

### 指令2：
```
现在请创建重构建议功能的辅助函数 generate_refactor_suggestions()。要求如下：

函数参数：接受文件路径、重构类型（如性能优化、可读性提升、安全性改进等）等参数。

分析逻辑：
- 读取目标文件内容
- 使用 AST 解析或其他方式分析代码结构
- 根据重构类型检查代码问题
- 识别代码异味（Code Smells）和改进机会

建议类型：
- 性能优化：识别低效的循环、重复计算等
- 可读性提升：建议改进命名、减少嵌套等
- 安全性改进：检查常见的安全漏洞模式
- 最佳实践：推荐符合 Python 风格指南的改进

输出格式：返回结构化的重构建议列表，每条建议包括问题描述、位置、建议方案等。
```

### 指令3：
```
接下来请在 src/kimi_cli/soul/slash.py 中注册 /refactor 斜杠命令：

使用 @registry.command 装饰器定义命令，支持别名如 /refactor 或 /r。

在命令处理函数中接收 soul 和 args 参数。

解析用户参数，包括目标文件路径和重构类型。

调用文件读取工具获取文件内容。

调用 generate_refactor_suggestions() 函数生成建议。

将建议格式化为易读的文本，使用代码块展示原始代码和改进建议。

考虑添加 --auto 选项，让 AI 自动应用某些重构建议。
```

### 指令4：
```
最后，测试新添加的 /refactor 命令：

在 Kimi CLI 的交互模式中输入 /refactor <文件路径> 验证命令功能。

使用不同重构类型测试命令，如 /refactor --type=performance、/refactor --type=readability 等。

检查重构建议的质量和准确性。

确认错误处理是否完善（如文件不存在、格式不支持等情况）。

如果有 --auto 选项，测试自动应用功能是否正确。

如果有任何问题，请修复并重新测试。
```

---

## Task 3: 添加 /test 指令

**需求**：为指定的函数或类生成单元测试。

### 指令1：
```
我需要为 Kimi CLI 添加一个 /test 指令，用于生成单元测试代码。请先执行以下操作：

扫描代码库，找到代码解析和分析相关的工具。

查看文件操作工具（src/kimi_cli/tools/file/）的使用方式。

确认项目使用的测试框架（如 pytest、unittest 等）。

找到斜杠命令注册的位置（src/kimi_cli/soul/slash.py）。

查看项目中已有的测试文件，了解测试编写风格。

向我汇报代码解析能力、测试框架、现有测试风格以及 /test 命令应该插入的位置。
```

### 指令2：
```
现在请创建测试生成功能的辅助函数 generate_unit_tests()。要求如下：

函数参数：接受文件路径、函数名/类名、测试框架（默认 pytest）等参数。

分析逻辑：
- 读取目标文件内容
- 解析指定的函数或类（使用 AST 解析）
- 提取函数签名、参数、返回类型、逻辑结构
- 识别边界条件、异常情况等需要测试的场景

测试生成策略：
- 为每个函数生成基本测试用例（正常输入）
- 生成边界值测试用例
- 生成异常情况测试用例
- 为类生成单元测试，包括方法测试
- 添加必要的 mock 和 fixture

输出格式：返回完整的测试代码字符串，符合项目的测试风格。
```

### 指令3：
```
接下来请在 src/kimi_cli/soul/slash.py 中注册 /test 斜杠命令：

使用 @registry.command 装饰器定义命令，支持别名如 /test 或 /t。

在命令处理函数中接收 soul 和 args 参数。

解析用户参数，包括目标文件路径、函数名/类名、测试框架等。

调用代码解析工具获取函数或类的详细信息。

调用 generate_unit_tests() 函数生成测试代码。

将生成的测试代码展示给用户，使用代码块格式。

考虑添加 --save 选项，将测试代码保存到文件（如 tests/test_<filename>.py）。
```

### 指令4：
```
最后，测试新添加的 /test 命令：

在 Kimi CLI 的交互模式中输入 /test <文件路径> --func=<函数名> 验证命令功能。

测试生成不同类型函数的测试用例（如普通函数、类方法、异步函数等）。

使用 --save 选项测试保存功能，检查生成的测试文件位置和内容。

运行生成的测试代码，确保测试能够通过。

检查测试覆盖率和测试质量。

确认错误处理是否完善（如函数不存在、无法解析等情况）。

如果有任何问题，请修复并重新测试。
```

---

## Task 4: 添加 /history 指令

**需求**：查看和搜索历史会话记录。

### 指令1：
```
我需要为 Kimi CLI 添加一个 /history 指令，用于查看和搜索历史会话记录。请先执行以下操作：

扫描代码库，找到会话管理相关的文件（应该在 src/kimi_cli/session.py）。

查看会话数据的存储位置和格式（文件或数据库）。

找到会话列表、会话详情、会话搜索等相关函数。

找到斜杠命令注册的位置（src/kimi_cli/soul/slash.py）。

查看其他斜杠命令的实现作为参考。

向我汇报会话数据结构、会话管理功能以及 /history 命令应该插入的位置。
```

### 指令2：
```
现在请创建历史记录查询功能的辅助函数 search_session_history()。要求如下：

函数参数：接受搜索关键词、时间范围、会话状态等参数。

查询逻辑：
- 读取所有历史会话记录
- 根据搜索关键词过滤会话（匹配会话标题、消息内容等）
- 根据时间范围过滤会话（如最近7天、本月等）
- 根据会话状态过滤（如已归档、活跃等）

排序选项：
- 按时间排序（最新/最旧）
- 按消息数量排序
- 按会话长度排序

输出格式：返回会话列表，每条记录包括会话 ID、标题、时间、消息摘要等信息。
```

### 指令3：
```
接下来请在 src/kimi_cli/soul/slash.py 中注册 /history 斜杠命令：

使用 @registry.command 装饰器定义命令，支持别名如 /history 或 /h。

在命令处理函数中接收 soul 和 args 参数。

解析用户参数，包括搜索关键词、时间范围、排序方式等。

调用会话管理函数获取历史记录。

调用 search_session_history() 函数进行搜索和过滤。

将结果格式化为易读的列表，每条记录显示关键信息。

考虑添加子命令：
- /history list：列出所有历史会话
- /history show <id>：显示指定会话的详细内容
- /history search <keyword>：搜索包含关键词的会话
- /history delete <id>：删除指定会话
```

### 指令4：
```
最后，测试新添加的 /history 命令：

在 Kimi CLI 的交互模式中输入 /history 验证命令功能。

测试列出所有历史会话：/history list

测试搜索功能：/history search "关键词"

测试查看指定会话：/history show <会话ID>

测试时间范围过滤：/history --days=7

测试不同排序方式：/history --sort=time

测试删除功能：/history delete <会话ID>

检查命令输出是否清晰、格式是否美观。

确认错误处理是否完善（如会话不存在、权限不足等情况）。

如果有任何问题，请修复并重新测试。
```

---

---

## Task 5: 添加 OpenAI GPT 模型支持

**需求**：配置 Kimi CLI 使用 OpenAI GPT 模型（GPT-4、GPT-4o 等）作为替代模型。

### 指令1：
```
我需要为当前的 Kimi CLI 项目配置 OpenAI GPT 模型支持。请先执行以下操作：

扫描代码库，找到 Provider 类型和模型配置相关的代码（src/kimi_cli/config.py 或类似位置）。

查看现有的 Provider 类型定义，了解支持的模型提供商（kimi、anthropic、gemini 等）。

找到模型配置的存储位置（~/.kimi/config.toml 或类似配置文件）。

查看 Provider 的实现方式，了解如何添加新的模型提供商。

向我汇报 Provider 类型、配置格式以及如何添加 OpenAI 支持的方法。
```

### 指令2：
```
现在请在配置文件中添加 OpenAI Provider 配置：

编辑 ~/.kimi/config.toml 文件（或创建新的配置文件）：

[providers.openai]
type = "openai_responses"
base_url = "https://api.openai.com/v1"
api_key = "sk-your-openai-key"

[mcp.client]
# 确保客户端支持 OpenAI API 格式

设置环境变量作为备选方案：
export OPENAI_API_KEY="sk-your-openai-key"
export OPENAI_BASE_URL="https://api.openai.com/v1"
```

### 指令3：
```
接下来添加 OpenAI 模型定义：

在配置文件中添加 GPT 模型配置：

[models.gpt-4o]
provider = "openai"
model = "gpt-4o"
max_context_size = 128000

[models.gpt-4-turbo]
provider = "openai"
model = "gpt-4-turbo"
max_context_size = 128000

[models.gpt-4]
provider = "openai"
model = "gpt-4"
max_context_size = 8192

设置默认模型：
default_model = "gpt-4o"
```

### 指令4：
```
最后，测试 OpenAI 模型集成：

重启 Kimi CLI，验证配置是否正确加载。

使用 kimi info 命令查看可用的模型列表。

测试使用 GPT 模型进行对话：
kimi --model gpt-4o

验证 API 调用是否正常，响应是否符合预期。

测试不同 GPT 模型的切换和功能。

更新文档，说明如何配置和使用 OpenAI 模型。
```

---

## Task 6: 添加 Anthropic Claude 模型支持

**需求**：配置 Kimi CLI 使用 Anthropic Claude 模型（Claude 3.5 Sonnet 等）。

### 指令1：
```
我需要为当前的 Kimi CLI 项目配置 Anthropic Claude 模型支持。请先执行以下操作：

扫描代码库，查看 anthropic Provider 的实现。

确认 Provider 类型定义中是否已包含 "anthropic" 类型。

查看 Claude 模型的 API 调用方式，了解请求和响应格式。

找到配置文件的位置和格式（~/.kimi/config.toml）。

向我汇报 Anthropic Provider 的配置方式、Claude 模型参数以及如何正确配置。
```

### 指令2：
```
现在请在配置文件中添加 Anthropic Provider 配置：

编辑 ~/.kimi/config.toml 文件：

[providers.anthropic]
type = "anthropic"
base_url = "https://api.anthropic.com"
api_key = "sk-ant-your-claude-key"

设置环境变量作为备选方案：
export ANTHROPIC_API_KEY="sk-ant-your-claude-key"
```

### 指令3：
```
接下来添加 Claude 模型定义：

在配置文件中添加 Claude 模型配置：

[models.claude-3-5-sonnet-20241022]
provider = "anthropic"
model = "claude-3-5-sonnet-20241022"
max_context_size = 200000

[models.claude-3-opus-20240229]
provider = "anthropic"
model = "claude-3-opus-20240229"
max_context_size = 200000

[models.claude-3-haiku-20240307]
provider = "anthropic"
model = "claude-3-haiku-20240307"
max_context_size = 200000

设置默认模型（可选）：
default_model = "claude-3-5-sonnet-20241022"
```

### 指令4：
```
最后，测试 Claude 模型集成：

重启 Kimi CLI，验证配置是否正确加载。

使用 kimi info 命令查看可用的 Claude 模型列表。

测试使用 Claude 模型进行对话：
kimi --model claude-3-5-sonnet-20241022

验证 API 调用是否正常，响应质量和速度是否符合预期。

测试不同 Claude 模型的切换，比较性能和效果。

更新文档，说明如何配置和使用 Claude 模型。
```

---

## Task 7: 创建自定义 Python 开发 Agent

**需求**：创建一个专门用于 Python 开发的自定义 Agent 规范。

### 指令1：
```
我需要为当前的 Kimi CLI 项目创建一个 Python 开发专用 Agent。请先执行以下操作：

扫描代码库，查看现有的 Agent 规范文件（src/kimi_cli/agents/ 目录）。

阅读 default agent 的配置文件，了解 Agent 规范的结构和格式。

查看 system prompt 模板文件，了解如何定制 Agent 的行为。

查看工具注册和排除的配置方式。

向我汇报 Agent 规范的结构、如何创建自定义 Agent 以及 Agent 配置文件的存储位置。
```

### 指令2：
```
现在创建 Python 开发 Agent 的目录和配置文件：

在 ~/.kimi/ 目录下创建 python-dev-agent/ 子目录。

创建 agent.yaml 配置文件：

version: 1
agent:
  name: "Python Development Specialist"
  system_prompt_path: ./system.md
  system_prompt_args:
    ROLE_ADDITIONAL: |
      You are a Python development specialist with deep expertise in:
      - Python 3.8+ features and best practices
      - Popular Python frameworks (Django, FastAPI, Flask, SQLAlchemy, Pydantic)
      - Testing frameworks (pytest, unittest, mocking)
      - Package management (poetry, pipenv, requirements.txt)
      - Performance optimization (async/await, multiprocessing)
      - Security best practices
    EXPERTISE: "Python, Django, FastAPI, pytest, SQLAlchemy, Pydantic"
  tools:
    - "kimi_cli.tools.file:ReadFile"
    - "kimi_cli.tools.file:WriteFile"
    - "kimi_cli.tools.file:Replace"
    - "kimi_cli.tools.shell:Shell"
    - "kimi_cli.tools.web:SearchWeb"
    - "kimi_cli.tools.web:FetchURL"
    - "kimi_cli.tools.todo:SetTodoList"
  exclude_tools:
    - "kimi_cli.tools.multiagent:Task"
```

### 指令3：
```
接下来创建 system.md 提示词模板文件：

在 ~/.kimi/python-dev-agent/ 目录下创建 system.md：

You are {{KIMI_NAME}}, an expert Python developer with 10+ years of experience.

{{ROLE_ADDITIONAL}}

Current working directory: {{KIMI_WORK_DIR}}
Available tools: {{KIMI_TOOLS}}
Available skills: {{KIMI_SKILLS}}

Your expertise includes: {{EXPERTISE}}

Development Approach:
1. Follow PEP 8 style guidelines and Python best practices
2. Write type-annotated code using modern type hints
3. Use async/await for I/O operations where appropriate
4. Write comprehensive tests with pytest
5. Use context managers for resource management
6. Implement proper error handling and logging
7. Follow the principle of least surprise in API design

When generating code:
- Use meaningful variable and function names
- Add docstrings following Google style
- Include proper imports and __all__ declarations
- Consider performance implications
- Add security considerations where relevant

Always provide explanations for design decisions and suggest improvements.
```

### 指令4：
```
最后，测试和注册自定义 Agent：

在 Kimi CLI 的配置中注册新的 Agent，或通过命令行指定：

kimi --agent ~/.kimi/python-dev-agent/agent.yaml

测试 Agent 的功能：
- 询问 Python 相关问题
- 请求生成 Python 代码
- 要求审查和优化现有 Python 代码
- 请求添加测试用例

验证 Agent 的工具访问和排除是否正确。

检查 Agent 的行为是否符合预期的 Python 专家角色。

更新文档，说明如何创建和使用自定义 Agent。
```

---

## Task 8: 创建 Docker 容器管理 Skill

**需求**：创建一个用于 Docker 容器管理的 Skill，能够帮助用户管理 Docker 容器、镜像等。

### 指令1：
```
我需要为当前的 Kimi CLI 项目创建一个 Docker 容器管理 Skill。请先执行以下操作：

扫描代码库，查看现有的 Skill 文件（~/.kimi/.agents/skills/ 或 src/kimi_cli 中的 skills 目录）。

阅读现有 Skill 的格式和结构（如 gen-changelog、gen-docs 等）。

了解 Skill 如何被调用和执行。

查看 Shell 工具的使用方式，了解如何执行 Docker 命令。

向我汇报 Skill 的格式、如何创建新 Skill 以及 Skill 的调用方式。
```

### 指令2：
```
现在创建 Docker 管理 Skill 文件：

在 ~/.kimi/.agents/skills/ 目录下创建 docker-manager.md：

---
name: docker-manager
description: Manage Docker containers, images, volumes, and networks
---

You are a Docker expert with deep knowledge of:

- Docker containers: create, start, stop, restart, remove, logs, exec
- Docker images: build, pull, push, tag, prune
- Docker volumes: create, remove, inspect
- Docker networks: create, connect, disconnect, remove
- Docker Compose: up, down, logs, ps

You can help users with:
1. Creating and managing containers with appropriate configurations
2. Building and managing Docker images
3. Setting up Docker Compose for multi-container applications
4. Troubleshooting Docker issues
5. Optimizing Docker configurations for performance and security

When executing Docker commands:
- Always use the -f flag to remove containers if needed
- Use --rm flag for temporary containers
- Specify proper resource limits (CPU, memory)
- Use proper network isolation
- Follow Docker security best practices

Example commands you might use:
- `docker run -d --name myapp -p 80:80 nginx`
- `docker ps -a`
- `docker logs -f myapp`
- `docker-compose up -d`
- `docker system prune -a`

Always explain what each command does and why it's needed.
```

### 指令3：
```
接下来测试 Docker 管理 Skill：

在 Kimi CLI 的交互模式中调用 Skill：

/skill:docker-manager

测试以下场景：
- 询问如何运行一个 Nginx 容器
- 请求创建 Docker Compose 配置
- 询问如何查看容器日志
- 请求清理未使用的 Docker 资源

验证 Skill 是否能够提供正确和有用的建议。

测试复杂场景，如多容器应用部署。

检查 Skill 的输出是否清晰、命令是否准确。
```

### 指令4：
```
最后，完善和文档化 Docker 管理 Skill：

根据测试结果优化 Skill 的提示词。

添加更多常见使用场景和示例命令。

创建 Skill 的使用文档：

Docker Manager Skill 使用指南
=============================

调用方式：/skill:docker-manager

主要功能：
- 容器管理
- 镜像管理
- Compose 配置生成
- 故障排除
- 资源清理

使用示例：
1. 启动一个容器：询问 "如何运行一个 Redis 容器？"
2. 创建 Compose 文件：请求 "为我的应用创建 docker-compose.yml"
3. 查看日志：询问 "如何查看容器日志？"

将文档保存为 ~/.kimi/.agents/skills/docker-manager-guide.md。
```

---

## Task 9: 集成 Context7 MCP 服务器

**需求**：将 Context7 MCP 服务器集成到 Kimi CLI 中，扩展工具能力。

### 指令1：
```
我需要为当前的 Kimi CLI 项目集成 Context7 MCP 服务器。请先执行以下操作：

扫描代码库，查看 MCP 集成的相关代码（src/kimi_cli/acp/mcp.py 或类似位置）。

查看 MCP 服务器的配置方式，了解如何注册和配置 MCP 服务器。

找到 MCP 配置文件的存储位置（~/.kimi/mcp.json 或类似）。

查看现有的 MCP 服务器集成示例。

向我汇报 MCP 服务器的配置格式、集成方式以及如何添加新的 MCP 服务器。
```

### 指令2：
```
现在使用 CLI 命令添加 Context7 MCP 服务器：

执行以下命令添加 HTTP-based MCP 服务器：

kimi mcp add --transport http context7 \
  https://mcp.context7.com/mcp \
  --header "CONTEXT7_API_KEY: ctx7sk-your-api-key"

或手动配置到 ~/.kimi/mcp.json：

{
  "mcpServers": {
    "context7": {
      "url": "https://mcp.context7.com/mcp",
      "headers": {
        "CONTEXT7_API_KEY": "your-api-key"
      }
    }
  }
}
```

### 指令3：
```
接下来测试 MCP 服务器的连接和功能：

测试 MCP 服务器连接：

kimi mcp test context7

验证连接是否成功，查看可用的工具列表。

重启 Kimi CLI，确保 MCP 服务器在启动时自动连接。

测试 Context7 提供的工具功能：
- 询问 AI 使用 Context7 的搜索功能
- 请求使用其他 Context7 提供的工具

验证工具调用是否正常工作。
```

### 指令4：
```
最后，配置和管理 MCP 服务器：

列出所有已配置的 MCP 服务器：

kimi mcp list

管理 MCP 服务器：

kimi mcp remove context7  # 移除服务器
kimi mcp auth context7     # 授权 OAuth（如果需要）

在配置中设置工具过滤和权限：

{
  "mcpServers": {
    "context7": {
      "url": "https://mcp.context7.com/mcp",
      "headers": {
        "CONTEXT7_API_KEY": "your-api-key"
      },
      "includeTools": ["search", "summarize"],
      "excludeTools": ["sensitive-tool"],
      "trust": false
    }
  }
}

更新文档，说明如何集成和管理 MCP 服务器。
```

---

## Task 10: 创建代码性能分析工具

**需求**：创建一个自定义工具，用于分析代码性能并提供优化建议。

### 指令1：
```
我需要为当前的 Kimi CLI 项目创建一个代码性能分析工具。请先执行以下操作：

扫描代码库，查看现有工具的实现方式（src/kimi_cli/tools/ 目录）。

阅读一个简单工具的实现（如 file/read.py），了解工具的结构和接口。

查看工具注册的方式（src/kimi_cli/soul/toolset.py）。

了解 Pydantic 模型的使用方式，用于工具参数定义。

向我汇报工具的接口定义、如何创建新工具以及工具注册的流程。
```

### 指令2：
```
现在创建代码性能分析工具：

创建 src/kimi_cli/tools/performance/analyzer.py 文件：

from pathlib import Path
from typing import override
from kosong.tooling import CallableTool2, ToolOk
from pydantic import BaseModel, Field

class AnalyzePerformanceParams(BaseModel):
    path: str = Field(description="Path to the code file to analyze")
    language: str = Field(default="python", description="Programming language")

class PerformanceAnalyzer(CallableTool2[AnalyzePerformanceParams]):
    name = "PerformanceAnalyzer"
    description = "Analyze code performance and provide optimization suggestions"
    params = AnalyzePerformanceParams

    @override
    async def __call__(self, params: AnalyzePerformanceParams) -> ToolOk:
        file_path = Path(params.path)
        if not file_path.exists():
            return ToolOk(output=f"File not found: {params.path}")

        content = file_path.read_text()
        analysis = self._analyze_code(content, params.language)
        return ToolOk(output=analysis)

    def _analyze_code(self, content: str, language: str) -> str:
        """Analyze code and provide performance suggestions"""
        suggestions = []

        if language == "python":
            suggestions = self._analyze_python(content)

        return self._format_report(suggestions)

    def _analyze_python(self, content: str) -> list:
        """Analyze Python code for performance issues"""
        issues = []

        # Check for common performance issues
        if "list comprehension" in content.lower():
            issues.append({
                "type": "info",
                "message": "List comprehension is being used, which is good for performance"
            })

        if "for i in range(len(" in content:
            issues.append({
                "type": "warning",
                "message": "Consider using enumerate() instead of range(len())"
            })

        return issues

    def _format_report(self, issues: list) -> str:
        """Format the analysis report"""
        if not issues:
            return "No performance issues found."

        report = "Performance Analysis Report:\n\n"
        for issue in issues:
            emoji = "✅" if issue["type"] == "info" else "⚠️"
            report += f"{emoji} {issue['message']}\n"

        return report
```

### 指令3：
```
接下来在工具集注册器中注册新工具：

编辑 src/kimi_cli/soul/toolset.py 文件：

from kimi_cli.tools.performance.analyzer import PerformanceAnalyzer

在工具集初始化时添加新工具：

def get_default_toolset() -> ToolSet:
    return ToolSet(
        tools=[
            # ... existing tools ...
            PerformanceAnalyzer(),
        ]
    )
```

### 指令4：
```
最后，测试性能分析工具：

重启 Kimi CLI，确保工具正确加载。

测试工具功能：

在交互模式中：
"请使用 PerformanceAnalyzer 工具分析 src/kimi_cli/app.py 文件的性能"

或直接让 AI 使用：
"分析这个 Python 文件的性能，并提供优化建议"

验证工具是否能够：
- 正确读取文件
- 分析代码中的性能问题
- 提供有用的优化建议

测试不同语言的支持（如果实现了多语言支持）。

更新工具的文档和使用示例。
```

---

## Task 11: 配置多 Agent 协作系统

**需求**：配置前端和后端子 Agent 协作，实现专业化的任务分配。

### 指令1：
```
我需要为当前的 Kimi CLI 项目配置多 Agent 协作系统。请先执行以下操作：

扫描代码库，查看 multiagent 工具的实现（src/kimi_cli/tools/multiagent/ 目录）。

阅读 Agent 规范文件，了解 subagents 的配置方式。

查看主 Agent 如何调用子 Agent。

理解 Agent 之间的上下文共享机制。

向我汇报多 Agent 系统的工作原理、如何配置子 Agent 以及任务分配机制。
```

### 指令2：
```
现在创建前端开发子 Agent：

在 ~/.kimi/frontend-dev-agent/ 目录下创建 agent.yaml：

version: 1
agent:
  name: "Frontend Development Specialist"
  system_prompt_path: ./system.md
  system_prompt_args:
    ROLE_ADDITIONAL: |
      You are a frontend development specialist with expertise in:
      - Modern JavaScript/TypeScript (ES6+, React, Vue, Svelte)
      - CSS frameworks (Tailwind, CSS Modules, Styled Components)
      - State management (Redux, Zustand, Pinia)
      - Build tools (Vite, Webpack, Rollup)
      - Performance optimization and accessibility
    EXPERTISE: "React, Vue, TypeScript, CSS, Tailwind, Vite"
  tools:
    - "kimi_cli.tools.file:ReadFile"
    - "kimi_cli.tools.file:WriteFile"
    - "kimi_cli.tools.shell:Shell"
    - "kimi_cli.tools.web:SearchWeb"
    - "kimi_cli.tools.todo:SetTodoList"

创建 frontend-dev-agent/system.md 提示词文件。
```

### 指令3：
```
接下来创建后端开发子 Agent：

在 ~/.kimi/backend-dev-agent/ 目录下创建 agent.yaml：

version: 1
agent:
  name: "Backend Development Specialist"
  system_prompt_path: ./system.md
  system_prompt_args:
    ROLE_ADDITIONAL: |
      You are a backend development specialist with expertise in:
      - Python frameworks (Django, FastAPI, Flask)
      - Database design and optimization
      - API design (REST, GraphQL)
      - Authentication and authorization
      - Performance optimization (caching, async)
    EXPERTISE: "Python, Django, FastAPI, SQLAlchemy, PostgreSQL, Redis"
  tools:
    - "kimi_cli.tools.file:ReadFile"
    - "kimi_cli.tools.file:WriteFile"
    - "kimi_cli.tools.shell:Shell"
    - "kimi_cli.tools.web:SearchWeb"
    - "kimi_cli.tools.todo:SetTodoList"

创建 backend-dev-agent/system.md 提示词文件。
```

### 指令4：
```

最后，在主 Agent 中配置子 Agent：

编辑主 Agent 的配置文件（或创建新的协调 Agent）：

version: 1
agent:
  name: "Full-Stack Development Coordinator"
  system_prompt_path: ./system.md
  system_prompt_args:
    ROLE_ADDITIONAL: |
      You are a full-stack development coordinator.
      You have access to specialized frontend and backend agents.
      Delegate tasks appropriately based on the task requirements.
  tools:
    - "kimi_cli.tools.file:ReadFile"
    - "kimi_cli.tools.file:WriteFile"
    - "kimi_cli.tools.multiagent:Task"
  subagents:
    frontend:
      path: ~/.kimi/frontend-dev-agent/agent.yaml
      description: "Specialized in frontend development"
    backend:
      path: ~/.kimi/backend-dev-agent/agent.yaml
      description: "Specialized in backend development"

测试多 Agent 协作：

"请创建一个全栈 Web 应用的登录功能"

验证主 Agent 是否能够：
- 识别任务需要前端和后端
- 正确调用相应的子 Agent
- 协调子 Agent 之间的协作
- 整合子 Agent 的输出

更新文档，说明如何配置和使用多 Agent 系统。
```

---

## Task 12: 代码架构分析 - Agent 规范系统

**需求**：阅读并分析 Kimi CLI 的 Agent 规范系统，理解其设计和工作原理。

### 指令1：
```
请阅读并分析 Kimi CLI 的 Agent 规范系统。请执行以下操作：

扫描代码库，找到 Agent 规范相关的文件（src/kimi_cli/agents/ 目录）。

阅读 agentspec.py 文件，了解 Agent 规范的解析和加载机制。

查看 default agent 的配置文件，了解规范的结构。

查看 system prompt 模板文件，了解模板变量的使用方式。

向我汇报 Agent 规范系统的目录结构、核心组件以及规范文件的格式。
```

### 指令2：
```

现在请深入分析 Agent 规范的结构和语法：

分析 agent.yaml 配置文件的各个字段：
- version：版本号
- name：Agent 名称
- description：Agent 描述
- system_prompt_path：提示词模板路径
- system_prompt_args：提示词模板参数
- tools：启用的工具列表
- exclude_tools：排除的工具列表
- subagents：子 Agent 配置

理解每个字段的作用和约束。

分析 Agent 继承机制（extend 字段）。
```

### 指令3：
```

接下来分析 Agent 规范的加载和解析过程：

查看 agentspec.py 中的 load_agentspec() 函数，了解如何加载 Agent 规范。

理解 YAML 解析的过程和错误处理。

查看模板变量的替换机制，了解 {{KIMI_NAME}}、{{KIMI_WORK_DIR}} 等变量是如何被替换的。

分析工具注册和过滤的逻辑。

理解 subagents 的加载和引用机制。
```

### 指令4：
```

最后，生成 Agent 规范系统的架构分析报告：

创建一份详细的文档，包括：
1. Agent 规范系统概述
2. 规范文件格式详解（YAML 结构）
3. 字段参考手册
4. 模板变量列表
5. 工具注册和过滤机制
6. 子 Agent 配置和使用
7. 创建自定义 Agent 的完整流程
8. 最佳实践和常见模式
9. 示例代码和配置

提供完整的示例 Agent 规范，包括：
- 基础 Agent
- 带继承的 Agent
- 带子 Agent 的 Agent
- 专用领域的 Agent

使用图表展示 Agent 规范系统的架构和加载流程。

报告应技术性强、信息全面，便于开发者深入理解和扩展系统。
```

---

## 关键文件参考

### Kimi CLI 关键文件路径
- 斜杠命令注册：`src/kimi_cli/soul/slash.py`
- 主应用入口：`src/kimi_cli/app.py`
- 会话管理：`src/kimi_cli/session.py`
- 上下文管理：`src/kimi_cli/soul/context.py`
- Agent 核心：`src/kimi_cli/soul/kimisoul.py`
- Agent 运行时：`src/kimi_cli/soul/agent.py`
- 文件读取工具：`src/kimi_cli/tools/file/read.py`
- 文件写入工具：`src/kimi_cli/tools/file/write.py`
- 文件替换工具：`src/kimi_cli/tools/file/replace.py`
- 文件搜索工具：`src/kimi_cli/tools/file/grep_local.py`
- 文件工具集：`src/kimi_cli/tools/file/utils.py`
- Shell 命令工具：`src/kimi_cli/tools/shell/__init__.py`
- Web 搜索工具：`src/kimi_cli/tools/web/search.py`
- Web 获取工具：`src/kimi_cli/tools/web/fetch.py`
- MCP 集成：`src/kimi_cli/acp/mcp.py`
- 配置管理：`src/kimi_cli/config.py`
- Agent 规范系统：`src/kimi_cli/agents/`
- Agent 规范加载：`src/kimi_cli/agentspec.py`
- 工具集管理：`src/kimi_cli/soul/toolset.py`
- Multiagent 工具：`src/kimi_cli/tools/multiagent/`
- 配置文件位置：`~/.kimi/config.toml`
- MCP 配置文件：`~/.kimi/mcp.json`
- Skills 目录：`~/.kimi/.agents/skills/`
