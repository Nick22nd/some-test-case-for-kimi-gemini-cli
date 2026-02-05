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
