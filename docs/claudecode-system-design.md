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
8. 深挖 06（状态与存储）：`docs/deep-dives/06-state-and-storage.md`
9. 深挖 07（可观测与遥测）：`docs/deep-dives/07-observability-and-telemetry.md`
10. 深挖 08（故障与恢复模式）：`docs/deep-dives/08-failure-recovery-patterns.md`
11. 执行模式对比（交互/非交互、SDK/CLI、本地/远程）：`docs/mode-comparison.md`

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
6. **ASCII 纯文本可读**：所有视图都必须提供 Mermaid 失效时可直接阅读的文本版。

---

## D. 已完成增强清单

- 已新增术语表：`docs/glossary.md`
- 已在每个深挖文档补充“真实代码调用路径（函数级追踪）”
- 已新增执行模式对比：`docs/mode-comparison.md`
- 所有含视图文档均补充“ASCII 纯文本可读视图（Mermaid 失效时阅读）”
- 已新增深挖 06/07/08：状态与存储、可观测与遥测、故障与恢复模式
- 已补充实验剧本：`docs/experiments/02-tool-concurrency-experiments.md`
- 已补充模板：`docs/templates/deep-dive-template.md`
