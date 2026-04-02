# ClaudeCode 系统设计术语表

> 本术语表优先覆盖当前文档库中的高频概念，后续可持续扩展。

## 核心术语

- **Runtime**
  - 指 ClaudeCode 的运行时系统，不只是模型调用，而是包含会话状态、调度、工具执行、权限治理、可观测与持久化的整体执行环境。

- **Query Loop**
  - Agent 主循环。围绕“模型输出 -> 解析 tool_use -> 执行工具 -> 回注 tool_result -> 继续推理”进行迭代，直到终止条件满足。

- **tool_use**
  - 模型在响应中声明的工具调用意图块，包含工具名与输入参数，是从“语言层”切换到“执行层”的桥。

- **tool_result**
  - 工具执行后的回传结果消息，回注到上下文供模型下一步推理。

- **contextModifier**
  - 工具执行后对 `ToolUseContext` 的增量修改器。常用于在并发执行后按稳定顺序合并上下文变更，确保一致性。

- **Compact / Auto Compact**
  - 上下文压缩机制。用于在长会话中控制上下文体积和 token 消耗，避免窗口溢出并维持可持续运行。

- **Token Budget / Task Budget**
  - 执行预算约束。前者偏 token 消耗控制，后者偏任务级总资源约束，用于防止 agent 失控迭代。

- **Policy Gate**
  - 策略闸门。指权限、组织策略、功能开关等在执行前做的集中决策入口。

- **Permission Mode**
  - 权限模式（如 default / auto / plan 相关过渡态），决定工具请求在何种规则下被 allow/deny/ask。

- **Concurrency-safe Tool**
  - 可并发安全执行的工具调用（通常是只读）。与副作用工具区分后可提升吞吐且不破坏状态一致性。

- **MCP (Model Context Protocol)**
  - 标准化外部工具与资源接入协议。ClaudeCode 通过 MCP 客户端将外部能力统一为可调用工具/资源。

- **Fast Path**
  - 启动快路径。针对轻量命令分支提前返回，避免加载重模块，降低 CLI 启动时延。
