# some-test-case-for-kimi-gemini-cli
some-test-case-for-kimi-gemini-cli

- Kimi Cli仓库地址： https://github.com/MoonshotAI/kimi-cli
- Gemini Cli 仓库地址：https://github.com/google-gemini/gemini-cli

# 需要把一些固定的任务

1. 你需要针对Kimi CLI 或者  Gemini CLI， 给出完成一项需求/功能/主题

2. 你需要按照指令来分解这个任务，然后生成一步一步的方便用户复制粘贴指令， 用户将会把指令一步一步下发给另外的大模型， 让他执行指令， 修改对应的仓库


## 例子

### Gemini

#### Task1 需要增加Claude code的 /context指令， 将上下文的信息分类， 展示对应的百分比

1. 指令1：
我需要为当前的 Gemini CLI 项目添加一个新功能 /context。请先执行以下操作：

扫描代码库，找到处理斜杠指令（Slash Commands）的核心逻辑文件。

找到负责统计 Token 数量或管理会话历史（Session History）的类或函数。

确认项目当前使用的 Token 计算库（如 tiktoken 或 Google SDK 自带的计数器）。

向我汇报这些关键文件的位置以及 /context 指令应该插入的代码位置

1. 指令2：
现在请根据上一步找到的逻辑，编写一个辅助函数 calculate_context_breakdown()。要求如下：

分类逻辑：将当前 Context 中的消息分为以下四类：

System: 系统预设提示词。

User: 用户输入的历史记录。

Assistant: 模型之前的回复。

Tools/Files: 通过外部工具加载的内容或读取的文件内容。

数据处理：计算每一类的 Token 总数，并获取当前模型支持的总 Context Limit（如果无法动态获取，请先定义一个常量）。

输出格式：返回一个字典，包含各分类的 Token 数、百分比以及总计已使用百分比。