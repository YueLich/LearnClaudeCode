# ClaudeCode 编码 Agent：系统设计视角总览

本文是文档库总览，面向“先建立全局认知，再逐层深挖”。

## A. 总览阅读顺序

1. 系统边界与职责逻辑视图：`docs/logical-view-system-boundary.md`
2. 术语表：`docs/glossary.md`
3. 深挖 01（核心循环）：`docs/deep-dives/01-query-core.md`
4. 深挖 02（工具执行链）：`docs/deep-dives/02-tool-execution.md`
5. 深挖 03（权限体系）：`docs/deep-dives/03-permission-system.md`
6. 深挖 04（启动优化）：`docs/deep-dives/04-startup-boot.md`
7. 深挖 05（生态扩展）：`docs/deep-dives/05-extensibility-ecosystem.md`
8. 执行模式对比（交互/非交互、SDK/CLI、本地/远程）：`docs/mode-comparison.md`

---

## B. ClaudeCode 的系统设计定位

ClaudeCode 不是“对话式脚本”，而是一个 **Agent Runtime**：

- 有会话生命周期管理（状态、预算、恢复）
- 有工具编排策略（并发/串行、上下文一致性）
- 有权限与策略治理（自动模式风险控制）
- 有扩展生态（commands/skills/plugins/MCP）

---

## C. 文档库建设约定（当前仓库）

为了把仓库建设成系统设计分析文档库，建议采用以下规范：

1. **一主题一文档**：每个核心主题独立 md。
2. **必含图示**：至少一张流程图/时序图/类图（Mermaid）。
3. **固定结构**：目标、边界、流程、关键机制、风险与演进。
4. **函数级追踪**：每个深挖文档必须补“真实代码调用路径”小节。
5. **双向链接**：总览页可跳转到深挖页，深挖页可回链总览。
6. **可增量扩展**：后续可继续新增 `06+` 深挖专题（如状态存储、可观测、任务系统）。

---

## D. 已完成增强（对应上一轮下一步建议）

- 已新增术语表：`docs/glossary.md`
- 已在每个深挖文档补充“真实代码调用路径（函数级追踪）”
- 已新增执行模式对比：`docs/mode-comparison.md`
- 所有含视图文档均补充“纯文本可读视图（Mermaid 失效时阅读）”。

---

## E. 进一步完善建议（本轮已同步落实）

- 新增通用模板：`docs/templates/deep-dive-template.md`，用于后续专题统一结构。
- 后续新增专题建议：
  - `06-state-and-storage.md`（状态持久化与恢复）
  - `07-observability-and-telemetry.md`（指标、日志、事件）
  - `08-failure-recovery-patterns.md`（错误恢复模式库）
