# ClaudeCode 编码 Agent：系统设计视角总览

本文是文档库总览，面向“先建立全局认知，再逐层深挖”。

## A. 总览阅读顺序

1. 系统边界与职责逻辑视图：`docs/logical-view-system-boundary.md`
2. 术语表：`docs/glossary.md`
3. 快速学习路径：`docs/quick-start-learning-path.md`
4. 深挖 01（核心循环）：`docs/deep-dives/01-query-core.md`
5. 深挖 02（工具执行链）：`docs/deep-dives/02-tool-execution.md`
6. 深挖 03（权限体系）：`docs/deep-dives/03-permission-system.md`
7. 深挖 04（启动优化）：`docs/deep-dives/04-startup-boot.md`
8. 深挖 05（生态扩展）：`docs/deep-dives/05-extensibility-ecosystem.md`
9. 深挖 06（状态与存储）：`docs/deep-dives/06-state-and-storage.md`
10. 深挖 07（可观测与遥测）：`docs/deep-dives/07-observability-and-telemetry.md`
11. 深挖 08（故障与恢复模式）：`docs/deep-dives/08-failure-recovery-patterns.md`
12. 执行模式对比（交互/非交互、SDK/CLI、本地/远程）：`docs/mode-comparison.md`

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

## D. 最新版本代码仓“学习地图”（2026-04）

> 目标：帮助第一次接触代码仓的同学，用“先看哪、再看哪”的方式快速建立整体模型。

### D.1 先抓 1 条主干：从入口到一次完整会话

```text
main.tsx
  -> init() / initializeToolPermissionContext() / getTools()
  -> showSetupScreens() / launchRepl()
  -> QueryEngine.submitMessage()
  -> query() / queryLoop()
  -> services/tools/toolOrchestration.ts::runTools()
  -> tools/* (Bash/MCP/Edit/...)
  -> state/* + utils/sessionStorage.ts
```

**快速定位建议（避免盲读）**
- 在 `main.tsx` 先检索：`await init(`、`initializeToolPermissionContext(`、`getTools(`、`launchRepl(`。
- 在核心循环检索：`QueryEngine.ts::submitMessage`、`query.ts::queryLoop`、`toolOrchestration.ts::runTools`。

**为什么先看这条主干**
- 它覆盖了“启动、渲染、调度、执行、落盘”的最短闭环。
- 看懂这条主干后，再去看远程模式、协作模式、插件生态会更轻松。

### D.2 按目录理解“系统层次”

1. **入口与会话装配**：`main.tsx`、`entrypoints/`、`screens/`
2. **核心循环**：`QueryEngine.ts`、`query.ts`、`query/`
3. **工具与执行面**：`tools/`、`tools.ts`、`services/tools/`
4. **状态与存储**：`state/`、`bootstrap/`、`utils/sessionStorage.ts`、`memdir/`
5. **安全与治理**：`utils/permissions/`、`services/policyLimits/`、`utils/settings/`
6. **扩展与生态**：`commands/`、`skills/`、`plugins/`、`services/mcp/`
7. **多形态运行能力**：`remote/`、`coordinator/`、`assistant/`、`voice/`

### D.3 面向学习目标的“最短路径”

- **目标 A：只想先跑通“Agent 为什么会调用工具”**
  1) `docs/deep-dives/01-query-core.md`
  2) `docs/deep-dives/02-tool-execution.md`
  3) 回到源码看 `query.ts` + `tools.ts`

- **目标 B：理解“为什么有些命令会被拦截或二次确认”**
  1) `docs/deep-dives/03-permission-system.md`
  2) `docs/deep-dives/08-failure-recovery-patterns.md`
  3) 回到源码看 `utils/permissions/` + `services/policyLimits/`

- **目标 C：理解“为什么启动这么快/还能继续优化”**
  1) `docs/deep-dives/04-startup-boot.md`
  2) 回到源码看 `main.tsx` 顶部的预取与 profile checkpoint

- **目标 D：准备做扩展（插件/技能/MCP）**
  1) `docs/deep-dives/05-extensibility-ecosystem.md`
  2) `docs/mode-comparison.md`
  3) 回到源码看 `commands/`、`skills/`、`plugins/`、`services/mcp/`

---

## E. 已完成增强清单

- 已新增术语表：`docs/glossary.md`
- 已在每个深挖文档补充“真实代码调用路径（函数级追踪）”
- 已新增执行模式对比：`docs/mode-comparison.md`
- 所有含视图文档均补充“ASCII 纯文本可读视图（Mermaid 失效时阅读）”
- 已新增深挖 06/07/08：状态与存储、可观测与遥测、故障与恢复模式
- 已补充实验剧本：`docs/experiments/02-tool-concurrency-experiments.md`
- 已补充模板：`docs/templates/deep-dive-template.md`
- 已补充“最新版本学习地图”：主干调用链、目录分层、按目标学习路径

---

## F. 下一轮建议（可选）

1. 在每个 deep-dive 文档末尾新增“10 分钟源码练习题”（给定入口函数，要求画调用链）。
2. 补一份“目录-职责速查表”（1 行描述 + 关键文件 + 常见改动风险）。
3. 增加“新手第一天学习路线”（30 分钟、2 小时、1 天三档）。
