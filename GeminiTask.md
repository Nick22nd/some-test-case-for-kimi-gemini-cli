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

---

## Task 5: 添加 OpenAI GPT 模型支持

**需求**：让 Gemini CLI 支持 OpenAI GPT 模型，使用户可以选择使用 GPT-4、GPT-4o 等模型。

### 指令1：
```
我需要为当前的 Gemini CLI 项目添加 OpenAI GPT 模型支持。请先执行以下操作：

扫描代码库，找到模型配置相关的文件（应该在 packages/core/src/config/ 目录）。

查看 models.ts 和 defaultModelConfigs.ts 的内容，了解模型定义和配置的结构。

找到 Content Generator 或 LLM Client 的创建位置，了解如何注册新的模型提供商。

向我汇报模型配置的结构、如何添加新模型提供商以及需要修改的关键文件位置。
```

### 指令2：
```
现在请在 packages/core/src/config/models.ts 中添加 OpenAI 模型定义：

在 VALID_GEMINI_MODELS 集合之外，定义 OpenAI 模型常量：
- GPT_4 = 'gpt-4'
- GPT_4_TURBO = 'gpt-4-turbo'
- GPT_4O = 'gpt-4o'
- GPT_4O_MINI = 'gpt-4o-mini'

创建 OpenAI 模型验证集合或使用现有的验证机制。

确保新模型能够被配置系统识别和验证。
```

### 指令3：
```
接下来请在 packages/core/src/config/defaultModelConfigs.ts 中添加 OpenAI 模型配置：

为每个 OpenAI 模型创建配置对象，继承自适当的基础配置：

'gpt-4': {
  extends: 'chat-base',
  modelConfig: {
    model: 'gpt-4',
    generateContentConfig: {
      temperature: 0.7,
      topP: 0.95,
      maxOutputTokens: 8192,
    },
  },
}

为 gpt-4-turbo、gpt-4o、gpt-4o-mini 创建类似配置，根据每个模型的特点调整参数。
```

### 指令4：
```
最后，实现 OpenAI API 客户端集成：

创建或修改 Content Generator，添加对 OpenAI API 的调用支持。

实现 OpenAI API 的认证机制（使用环境变量 OPENAI_API_KEY）。

确保请求和响应格式能够正确转换（OpenAI 格式到 Gemini CLI 内部格式）。

测试新添加的 OpenAI 模型，验证是否能够正常生成响应。

更新文档，说明如何配置和使用 OpenAI 模型。
```

---

## Task 6: 添加 Anthropic Claude 模型支持

**需求**：让 Gemini CLI 支持 Anthropic Claude 模型，包括 Claude 3.5 Sonnet、Claude 3 Opus 等。

### 指令1：
```
我需要为当前的 Gemini CLI 项目添加 Anthropic Claude 模型支持。请先执行以下操作：

扫描代码库，确认模型配置文件位置（packages/core/src/config/models.ts 和 defaultModelConfigs.ts）。

查看现有的模型提供商集成方式，了解如何添加新的提供商。

找到 API 调用层，了解如何实现 Claude API 的调用。

向我汇报需要修改的文件、模型配置格式以及 API 集成方式。
```

### 指令2：
```
现在请在 packages/core/src/config/models.ts 中添加 Claude 模型定义：

定义 Claude 模型常量：
- CLAUDE_3_OPUS = 'claude-3-opus-20240229'
- CLAUDE_3_SONNET = 'claude-3-sonnet-20240229'
- CLAUDE_3_5_SONNET = 'claude-3-5-sonnet-20241022'
- CLAUDE_3_5_HAIKU = 'claude-3-5-haiku-20241022'

在模型验证机制中添加 Claude 模型的支持。
```

### 指令3：
```
接下来请在 packages/core/src/config/defaultModelConfigs.ts 中添加 Claude 模型配置：

为每个 Claude 模型创建配置对象：

'claude-3-5-sonnet-20241022': {
  extends: 'chat-base',
  modelConfig: {
    model: 'claude-3-5-sonnet-20241022',
    generateContentConfig: {
      temperature: 0.7,
      topP: 0.95,
      maxOutputTokens: 8192,
    },
  },
}

根据每个 Claude 模型的特点（context window、speed 等）调整配置参数。
```

### 指令4：
```
最后，实现 Anthropic Claude API 客户端集成：

创建 Claude API 客户端，处理 API 认证（使用 ANTHROPIC_API_KEY 环境变量）。

实现 Claude API 的消息格式转换（Claude Messages API 到 Gemini CLI 内部格式）。

处理流式响应（如果支持）和错误处理。

测试 Claude 模型集成，验证响应质量和速度。

更新配置示例，说明如何切换到 Claude 模型。
```

---

## Task 7: 创建自定义代码审查 Agent

**需求**：创建一个专门用于代码审查的 Agent，能够分析代码质量、安全性、性能等问题。

### 指令1：
```
我需要为当前的 Gemini CLI 项目创建一个代码审查 Agent。请先执行以下操作：

扫描代码库，找到 Agent 系统的核心文件（packages/core/src/agents/ 目录）。

查看 agentLoader.ts 和 types.ts，了解 Agent 的定义结构和加载机制。

查看现有 Agent 的实现示例（如果有）。

向我汇报 Agent 的定义格式、如何创建自定义 Agent 以及 Agent 注册方式。
```

### 指令2：
```
现在创建代码审查 Agent 的配置文件（.agent 卡片）：

在 packages/core/src/agents/ 目录下创建 code-reviewer.agent.md 文件：

---
name: code-reviewer
description: Expert code reviewer focusing on quality, security, and performance
model: gemini-2.5-pro
temperature: 0.3
max_turns: 5
timeout_mins: 10
tools: ["read_file", "grep", "git"]
---

You are an expert code reviewer with deep knowledge of:
- Software engineering best practices
- Security vulnerabilities and common attacks
- Performance optimization techniques
- Code maintainability and readability
- Testing strategies

When reviewing code:
1. Identify potential security issues (SQL injection, XSS, etc.)
2. Point out performance bottlenecks
3. Suggest improvements for code clarity
4. Check for proper error handling
5. Verify adherence to coding standards
6. Provide specific line numbers for issues
7. Balance criticism with positive feedback

Always provide actionable recommendations with examples.
```

### 指令3：
```
接下来注册新的代码审查 Agent：

在 packages/core/src/agents/agentLoader.ts 中添加 Agent 加载逻辑：

确保 .agent.md 文件能够被自动发现和加载。

验证 Agent 的元数据解析（frontmatter 部分能够正确读取）。

测试 Agent 是否能够被正确实例化和调用。
```

### 指令4：
```
最后，添加 /code-review 斜杠命令来触发代码审查 Agent：

在 packages/cli/src/ui/commands/ 目录下创建 codeReview.ts 文件。

定义命令结构，支持参数如 /code-review --file=<路径> 或 /code-review --branch=<分支名>。

在 action 函数中调用代码审查 Agent，传入待审查的代码上下文。

使用 Ink 组件展示审查结果，包括问题列表、严重程度评级和建议。

测试完整的代码审查流程。
```

---

## Task 8: 集成自定义 MCP 服务器

**需求**：将自定义 MCP 服务器集成到 Gemini CLI 中，扩展工具能力。

### 指令1：
```
我需要为当前的 Gemini CLI 项目集成一个自定义 MCP 服务器。请先执行以下操作：

扫描代码库，找到 MCP 集成相关的代码（packages/core/src/tools/mcp-client-manager.ts 或类似位置）。

查看 MCP 服务器的配置方式，了解如何注册和启动 MCP 服务器。

找到配置文件位置（~/.gemini-cli/config 或 mcpServers.json）。

查看现有的 MCP 服务器集成示例。

向我汇报 MCP 服务器的配置格式、集成方式以及需要修改的文件。
```

### 指令2：
```
现在创建自定义 MCP 服务器的配置：

在配置文件中添加 MCP 服务器定义：

{
  "mcpServers": {
    "my-custom-server": {
      "command": "node",
      "args": ["path/to/server.js"],
      "env": {
        "API_KEY": "$MY_API_KEY",
        "CUSTOM_PARAM": "value"
      },
      "cwd": "./server-dir",
      "includeTools": ["tool1", "tool2"],
      "excludeTools": ["sensitive-tool"],
      "trust": false
    }
  }
}

确保配置格式符合项目的要求，环境变量能够正确解析。
```

### 指令3：
```
接下来实现 MCP 服务器的启动和工具注册：

在 mcp-client-manager.ts 中添加启动逻辑：

读取配置中的 MCP 服务器定义。

为每个服务器创建客户端连接。

实现工具列表的获取和注册（includeTools 和 excludeTools 过滤）。

处理服务器的启动、停止和重新加载。

添加错误处理，处理服务器启动失败、连接超时等情况。
```

### 指令4：
```
最后，添加 MCP 服务器管理命令：

在 packages/cli/src/ui/commands/ 目录下创建 mcp.ts 文件。

定义子命令：
- /mcp list：列出所有已配置的 MCP 服务器
- /mcp start <server>：启动指定的 MCP 服务器
- /mcp stop <server>：停止指定的 MCP 服务器
- /mcp test <server>：测试服务器连接和工具可用性

测试 MCP 服务器的启动、停止和工具调用功能。

验证从 MCP 服务器提供的工具是否能够在 CLI 中正常使用。
```

---

## Task 9: 添加 OAuth 认证提供者

**需求**：为第三方服务添加 OAuth 认证支持，实现安全的 API 访问。

### 指令1：
```
我需要为当前的 Gemini CLI 项目添加 OAuth 认证提供者。请先执行以下操作：

扫描代码库，找到认证相关的代码（packages/core/src/agents/auth-provider/ 或 packages/core/src/mcp/ 目录）。

查看现有的认证实现（Google Auth、OAuth2 等），了解认证提供者的接口和生命周期。

找到认证配置的存储位置（配置文件、凭证存储等）。

向我汇报认证提供者的接口定义、如何添加新的认证方式以及凭证存储机制。
```

### 指令2：
```
现在创建自定义 OAuth 认证提供者：

在 packages/core/src/agents/auth-provider/ 目录下创建 custom-provider.ts 文件：

import { BaseA2AAuthProvider, HttpHeaders } from './base-provider';

export class CustomAuthProvider extends BaseA2AAuthProvider {
  readonly type = 'custom';

  private accessToken: string | null = null;

  async initialize(): Promise<void> {
    // 初始化认证状态
    // 可能需要从配置读取或触发 OAuth 流程
  }

  async headers(): Promise<HttpHeaders> {
    if (!this.accessToken) {
      await this.refreshToken();
    }
    return {
      'Authorization': `Bearer ${this.accessToken}`,
      'Content-Type': 'application/json'
    };
  }

  private async refreshToken(): Promise<void> {
    // 实现 token 刷新逻辑
    // 调用第三方 OAuth 端点
  }

  async revoke(): Promise<void> {
    // 撤销认证
    this.accessToken = null;
  }
}
```

### 指令3：
```
接下来注册新的认证提供者：

在认证管理器中添加 CustomAuthProvider 的注册：

确保提供者类型 'custom' 能够被识别。

在配置系统中添加相关参数支持：

{
  "auth": {
    "custom": {
      "clientId": "your-client-id",
      "clientSecret": "your-client-secret",
      "authorizationEndpoint": "https://auth.example.com/oauth/authorize",
      "tokenEndpoint": "https://auth.example.com/oauth/token",
      "scopes": ["read:api", "write:api"]
    }
  }
}

实现 OAuth 设备授权流程（如果适用）。
```

### 指令4：
```
最后，添加认证管理命令和凭证存储：

在 packages/cli/src/ui/commands/ 目录下创建 auth.ts 文件。

定义命令：
- /auth login <provider>：触发登录流程
- /auth logout <provider>：撤销认证
- /auth status <provider>：查看认证状态

实现凭证的安全存储（使用操作系统密钥链或加密文件）。

测试完整的 OAuth 流程，包括授权、token 刷新和撤销。

验证带有认证的 API 调用是否正常工作。
```

---

## Task 10: 创建自定义终端主题

**需求**：为 Gemini CLI 创建一个自定义终端主题，提供不同的视觉体验。

### 指令1：
```
我需要为当前的 Gemini CLI 项目创建自定义终端主题。请先执行以下操作：

扫描代码库，找到 UI 主题相关的代码（packages/cli/src/ui/themes/ 或类似目录）。

查看现有主题的实现，了解主题系统的接口和配置方式。

找到主题配置文件（可能是 JSON 或 TypeScript 文件）。

查看 Ink 组件库如何应用主题样式。

向我汇报主题系统的结构、如何定义新主题以及主题应用的机制。
```

### 指令2：
```
现在创建自定义主题配置：

在 packages/cli/src/ui/themes/ 目录下创建 customTheme.ts 文件：

import { CustomTheme } from '../types';

export const customTheme: CustomTheme = {
  type: 'custom',
  name: 'Custom',
  text: {
    primary: '#00ff00',
    secondary: '#00cc00',
    accent: '#00ff88',
    muted: '#008800',
    error: '#ff4444',
    warning: '#ffaa00',
    success: '#00ff00'
  },
  background: {
    primary: '#001100',
    secondary: '#002200',
    diff: {
      added: '#003300',
      removed: '#330000'
    }
  },
  border: {
    primary: '#00aa00',
    secondary: '#006600',
    focus: '#00ff88'
  },
  syntax: {
    keyword: '#ff00ff',
    string: '#00ff00',
    comment: '#008800',
    function: '#00aaff'
  }
};

根据个人喜好调整颜色值，创建和谐的配色方案。
```

### 指令3：
```

接下来注册自定义主题：

在主题管理器中添加新主题的注册：

确保 customTheme 能够被主题系统识别和加载。

在配置系统中添加主题选择支持：

{
  "ui": {
    "customThemes": {
      "my-theme": "packages/cli/src/ui/themes/customTheme"
    },
    "theme": "my-theme"
  }
}

支持通过命令行或配置文件切换主题。
```

### 指令4：
```
最后，添加主题管理命令和预览功能：

在 packages/cli/src/ui/commands/ 目录下创建 theme.ts 文件。

定义命令：
- /theme list：列出所有可用主题
- /theme set <name>：设置当前主题
- /theme preview <name>：预览主题效果（显示示例 UI 组件）

在 App.tsx 或 AppContainer.tsx 中应用主题到所有 UI 组件。

测试主题切换功能，验证所有组件的样式是否正确应用。

确保主题在不同终端环境下的兼容性。
```

---

## Task 11: 代码架构分析 - Agent 系统解析

**需求**：阅读并分析 Gemini CLI 的 Agent 系统，理解其架构设计。

### 指令1：
```
请阅读并分析 Gemini CLI 的 Agent 系统架构。请执行以下操作：

扫描 packages/core/src/agents/ 目录，列出所有关键文件。

阅读 agentLoader.ts 文件，了解 Agent 的加载机制和生命周期。

阅读 types.ts 文件，理解 Agent 的类型定义和接口。

阅读 base-agent.ts（如果存在），了解 Agent 的基类实现。

向我汇报 Agent 系统的目录结构、核心组件以及它们之间的关系。
```

### 指令2：
```

现在请深入分析 Agent 的创建和执行流程：

查看 Agent 的定义格式（.agent 文件或配置文件）。

理解 Agent 的元数据（name、description、model、temperature 等）是如何使用的。

分析 Agent 如何获取工具（tools）以及工具的注册机制。

查看 Agent 的执行逻辑，了解如何处理用户输入和生成响应。

分析 subagent 机制，理解 Agent 之间的协作方式。
```

### 指令3：
```

接下来分析 Agent 系统的设计模式：

识别 Agent 系统使用的设计模式（工厂模式、策略模式、观察者模式等）。

分析 Agent 配置的继承机制（extends 字段的作用）。

理解 Agent 上下文（context）的传递和管理方式。

分析 Agent 错误处理和重试机制。

评估系统的可扩展性，说明如何添加新类型的 Agent。
```

### 指令4：
```

最后，生成 Agent 系统的架构分析报告：

创建一份详细的架构文档，包括：
1. 系统概述和核心概念
2. 组件图和类关系
3. 关键流程图（Agent 加载、执行、子 Agent 调用）
4. 配置格式说明
5. 扩展点和使用建议

使用图表（Mermaid 或 ASCII）展示架构和流程。

提供代码示例，说明如何创建自定义 Agent。

报告应清晰易懂，便于其他开发者理解和使用 Agent 系统。
```

---

## Task 12: 代码架构分析 - MCP 集成机制

**需求**：阅读并分析 Gemini CLI 的 MCP (Model Context Protocol) 集成机制。

### 指令1：
```
请阅读并分析 Gemini CLI 的 MCP 集成机制。请执行以下操作：

扫描 packages/core/src/tools/ 目录，查找 MCP 相关的文件。

阅读 mcp-client-manager.ts 文件，了解 MCP 客户端的管理机制。

查看 MCP 相关的类型定义文件，理解接口和数据结构。

找到 MCP 服务器的配置位置和格式。

向我汇报 MCP 系统的目录结构、核心组件以及配置方式。
```

### 指令2：
```

现在请深入分析 MCP 服务器的生命周期：

了解 MCP 服务器的启动流程（如何根据配置创建服务器实例）。

分析 MCP 客户端与服务器的通信机制（stdio、HTTP、SSE 等）。

查看工具列表的获取和注册流程，理解 MCP 工具如何暴露给 CLI。

分析 MCP 服务器的停止和清理机制。

查看错误处理，了解服务器连接失败时的处理方式。
```

### 指令3：
```

接下来分析 MCP 工具的调用机制：

了解 MCP 工具的调用流程（从用户输入到工具执行）。

分析工具参数的传递和响应的解析。

查看工具权限管理（trust、includeTools、excludeTools）的实现。

理解 MCP 工具与内置工具的集成方式。

分析 MCP 工具调用的异步处理和超时机制。
```

### 指令4：
```

最后，生成 MCP 集成机制的架构分析报告：

创建一份详细的文档，包括：
1. MCP 协议简介和在 Gemini CLI 中的作用
2. 系统架构图和组件关系
3. MCP 服务器生命周期流程图
4. 工具调用流程图
5. 配置说明和示例
6. 扩展指南：如何集成新的 MCP 服务器
7. 安全性考虑和最佳实践

提供代码示例，说明如何创建自定义 MCP 服务器。

报告应包含技术细节，便于开发者进行深度集成和扩展。
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
- 模型配置：`packages/core/src/config/models.ts`
- 模型默认配置：`packages/core/src/config/defaultModelConfigs.ts`
- Agent 系统：`packages/core/src/agents/`
- Agent 加载器：`packages/core/src/agents/agentLoader.ts`
- Agent 类型：`packages/core/src/agents/types.ts`
- MCP 管理：`packages/core/src/tools/mcp-client-manager.ts`
- 认证提供者：`packages/core/src/agents/auth-provider/`
- OAuth 配置：`packages/core/src/mcp/oauth-provider.ts`
- UI 主题：`packages/cli/src/ui/themes/`
