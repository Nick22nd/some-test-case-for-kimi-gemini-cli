# Gemini CLI 任务指令用例

本文档包含针对 Gemini CLI 仓库 (https://github.com/google-gemini/gemini-cli) 的任务指令用例。每个任务按照指令分解，方便用户复制粘贴逐步执行。

---

## Task 1: 添加 /context 指令

**需求**：需要增加 Claude Code 的 /context 指令，将上下文的信息分类，展示对应的百分比。

### 指令1：
```
我需要为当前的 Gemini CLI 项目添加一个新功能 /context。请先执行以下操作：

扫描代码库，找到处理斜杠指令（Slash Commands）的核心逻辑文件。

找到负责统计 Token 数量或管理会话历史（Session History）的类或函数。

确认项目当前使用的 Token 计算库（如 tiktoken 或 Google SDK 自带的计数器）。

向我汇报这些关键文件的位置以及 /context 指令应该插入的代码位置。
```

### 指令2：
```
现在请根据上一步找到的逻辑，编写一个辅助函数 calculate_context_breakdown()。要求如下：

分类逻辑：将当前 Context 中的消息分为以下四类：
- System: 系统预设提示词
- User: 用户输入的历史记录
- Assistant: 模型之前的回复
- Tools/Files: 通过外部工具加载的内容或读取的文件内容

数据处理：计算每一类的 Token 总数，并获取当前模型支持的总 Context Limit（如果无法动态获取，请先定义一个常量）。

输出格式：返回一个字典，包含各分类的 Token 数、百分比以及总计已使用百分比。
```

### 指令3：
```
接下来请在 packages/cli/src/ui/commands/ 目录下创建 context.ts 文件，定义 /context 斜杠命令：

使用 SlashCommand 接口定义命令结构，包括 name、description 等属性。

在 action 函数中调用 calculate_context_breakdown() 获取上下文统计信息。

使用 Ink 组件库渲染一个格式化的统计表格，显示各类别的 Token 数量和百分比。

添加颜色标识，让不同类型的信息在终端中更易区分。
```

### 指令4：
```
最后，在 packages/cli/src/services/BuiltinCommandLoader.ts 中注册新命令：

导入刚创建的 context 命令。

将 context 命令添加到 allDefinitions 数组中。

确保命令的依赖注入配置正确（如 config 参数）。

运行项目并测试 /context 命令，验证统计信息是否正确显示。
```

---

## Task 2: 添加 /search 指令

**需求**：在代码库中进行文本搜索，并显示匹配结果。

### 指令1：
```
我需要为 Gemini CLI 添加一个 /search 指令，用于在代码库中搜索文本。请先执行以下操作：

扫描代码库，找到处理文件搜索相关的工具函数。

确认项目是否已经有 grep 或类似的搜索实现（可能位于 packages/cli/src/utils/ 或类似目录）。

找到斜杠命令的注册位置和命令定义的模板文件。

向我汇报现有的搜索功能实现和 /search 命令应该插入的位置。
```

### 指令2：
```
现在请创建搜索功能的辅助函数 search_codebase()。要求如下：

函数参数：接受搜索关键词、搜索目录（默认为当前工作目录）、文件类型过滤（可选）、是否区分大小写等参数。

搜索逻辑：使用 ripgrep 或项目已有的搜索工具进行搜索。

结果处理：返回包含文件路径、行号、匹配内容行的结果列表。

错误处理：处理搜索目录不存在、无权限访问等异常情况。
```

### 指令3：
```
接下来请在 packages/cli/src/ui/commands/ 目录下创建 search.ts 文件，定义 /search 斜杠命令：

定义命令结构，支持子命令如 /search --case-sensitive、/search --type=ts 等。

在 action 函数中解析用户参数，调用 search_codebase() 函数。

使用 Ink 组件渲染搜索结果，包括文件名、行号和匹配内容。

添加分页功能，如果结果过多，支持上下滚动查看。
```

### 指令4：
```
最后，在 packages/cli/src/services/BuiltinCommandLoader.ts 中注册新命令：

导入刚创建的 search 命令。

将 search 命令添加到 allDefinitions 数组中。

配置命令的参数解析和验证逻辑。

运行项目并测试 /search 命令，验证搜索功能是否正常工作。
```

---

## Task 3: 添加 /export 指令

**需求**：导出当前会话记录为 JSON 或 Markdown 文件。

### 指令1：
```
我需要为 Gemini CLI 添加一个 /export 指令，用于导出当前会话记录。请先执行以下操作：

扫描代码库，找到会话管理相关的文件（可能位于 packages/cli/src/utils/sessions.ts 或类似位置）。

找到会话数据的存储结构和类型定义。

找到文件写入相关的工具函数或模块。

确认项目是否已经有文件导出的功能可以参考。

向我汇报会话数据的结构、文件写入工具的位置以及 /export 命令应该插入的位置。
```

### 指令2：
```
现在请创建导出功能的辅助函数 export_session()。要求如下：

函数参数：接受会话 ID、导出格式（json 或 markdown）、输出文件路径等参数。

JSON 格式导出：将会话消息序列化为 JSON，包括时间戳、角色、内容等完整信息。

Markdown 格式导出：将会话转换为易读的 Markdown 格式，使用代码块标记不同角色的消息。

文件写入：使用项目的文件写入工具，处理文件路径验证、权限检查等。

错误处理：处理会话不存在、文件写入失败等异常情况。
```

### 指令3：
```
接下来请在 packages/cli/src/ui/commands/ 目录下创建 export.ts 文件，定义 /export 斜杠命令：

定义命令结构，支持子命令如 /export --format=json、/export --output=./conversation.md 等。

在 action 函数中获取当前会话 ID，解析用户指定的导出格式和输出路径。

调用 export_session() 函数执行导出操作。

使用 Ink 组件显示导出进度和成功/失败消息。
```

### 指令4：
```
最后，在 packages/cli/src/services/BuiltinCommandLoader.ts 中注册新命令：

导入刚创建的 export 命令。

将 export 命令添加到 allDefinitions 数组中。

配置命令的参数解析和默认值设置。

运行项目并测试 /export 命令，验证导出功能是否正常工作，检查生成的文件格式是否正确。
```

---

## Task 4: 添加 /token 指令

**需求**：显示当前会话的 Token 使用统计。

### 指令1：
```
我需要为 Gemini CLI 添加一个 /token 指令，用于显示 Token 使用统计。请先执行以下操作：

扫描代码库，找到 Token 计数相关的代码和函数。

找到会话历史（Session History）的存储位置和数据结构。

确认项目使用的 Token 计算库和计数方法。

找到命令注册的位置和命令定义的模板。

向我汇报 Token 计数的实现方式、会话数据结构以及 /token 命令应该插入的位置。
```

### 指令2：
```
现在请创建 Token 统计功能的辅助函数 calculate_token_stats()。要求如下：

函数参数：接受会话对象或会话 ID 作为输入。

Token 计算：遍历会话中的所有消息，计算每条消息的 Token 数量。

统计信息：计算以下数据：
- 总 Token 数量（输入 + 输出）
- 用户消息 Token 数
- 助手回复 Token 数
- 系统提示词 Token 数
- 工具调用 Token 数
- 上下文窗口使用百分比

返回格式：返回一个结构化的对象，包含所有统计信息。
```

### 指令3：
```
接下来请在 packages/cli/src/ui/commands/ 目录下创建 token.ts 文件，定义 /token 斜杠命令：

定义命令结构，可以添加选项如 /token --verbose 显示详细信息，或 /token --history 显示历史统计。

在 action 函数中获取当前会话，调用 calculate_token_stats() 函数。

使用 Ink 组件渲染一个美观的统计面板，使用进度条、表格或图表展示数据。

添加颜色标识，让不同的统计项在终端中更易区分。
```

### 指令4：
```
最后，在 packages/cli/src/services/BuiltinCommandLoader.ts 中注册新命令：

导入刚创建的 token 命令。

将 token 命令添加到 allDefinitions 数组中。

确保命令的依赖注入配置正确。

运行项目并测试 /token 命令，验证统计信息是否正确显示，测试各种选项参数是否正常工作。
```

---

## 关键文件参考

### Gemini CLI 关键文件路径
- 命令处理核心：`packages/cli/src/ui/hooks/slashCommandProcessor.ts`
- 命令加载器：`packages/cli/src/services/BuiltinCommandLoader.ts`
- 命令类型定义：`packages/cli/src/ui/commands/types.ts`
- 会话管理：`packages/cli/src/utils/sessions.ts`
- 会话工具：`packages/cli/src/utils/sessionUtils.ts`
- 文件工具：`packages/cli/src/utils/fileUtils.ts`
- 配置管理：`packages/cli/src/config/config.ts`
