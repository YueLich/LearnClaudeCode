# ClaudeCode 代码仓快速学习路径（最新版）

> 适用对象：第一次阅读该仓库，希望 30~120 分钟内建立“能讲清系统如何工作”的认知。

## 1. 30 分钟速通（先建立大图）

1. 阅读总览：`docs/claudecode-system-design.md`
2. 阅读系统边界：`docs/logical-view-system-boundary.md`
3. 打开 `main.tsx`，优先看这些**可检索锚点**（建议直接 `rg -n`）：
   - 启动预取与启动性能埋点：
     - `profileCheckpoint('main_tsx_entry')`
     - `startMdmRawRead()`
     - `startKeychainPrefetch()`
   - 初始化主链路：
     - `await init()`
     - `initializeToolPermissionContext(...)`
     - `getTools(toolPermissionContext)`
   - 进入交互循环（REPL）：
     - `showSetupScreens(...)`
     - `launchRepl(root, {...})`
   - 状态更新入口：
     - `createStore(headlessInitialState, onChangeAppState)`
4. 打开 `QueryEngine.ts` 和 `query.ts`，按以下函数名确认主循环“消息 -> 工具 -> 回注 -> 继续推理”：
   - `QueryEngine.submitMessage(...)`
   - `query(...)` / `queryLoop(...)`
   - `runTools(...)`（在 `services/tools/toolOrchestration.ts`）

完成标准（自测）
- 你能用 5 句话解释：ClaudeCode 从用户输入到最终输出的主链路。

### 1.1 快速定位命令（可直接复制）

```bash
rg -n "profileCheckpoint\('main_tsx_entry'\)|startMdmRawRead\(|startKeychainPrefetch\(" main.tsx
rg -n "await init\(|initializeToolPermissionContext\(|getTools\(" main.tsx
rg -n "showSetupScreens\(|launchRepl\(|createStore\(headlessInitialState" main.tsx
rg -n "submitMessage\(|queryLoop\(|runTools\(" QueryEngine.ts query.ts services/tools/toolOrchestration.ts
```

## 2. 2 小时进阶（抓住可改动的关键面）

### 2.1 工具执行链

- 文档：`docs/deep-dives/02-tool-execution.md`
- 源码：`tools.ts`、`tools/`、`services/tools/`
- 重点问题：
  - 哪些工具可以并行？
  - tool_result 如何回到下一轮模型上下文？

### 2.2 权限与策略

- 文档：`docs/deep-dives/03-permission-system.md`
- 源码：`utils/permissions/`、`services/policyLimits/`
- 重点问题：
  - 默认模式与 auto 模式的差异是什么？
  - 哪些风险会触发拦截或降级？

### 2.3 状态、存储与恢复

- 文档：`docs/deep-dives/06-state-and-storage.md`、`docs/deep-dives/08-failure-recovery-patterns.md`
- 源码：`state/`、`bootstrap/`、`utils/sessionStorage.ts`、`remote/`
- 重点问题：
  - 会话状态在哪里存？
  - 中断后如何恢复？

## 3. 面向改代码的目录导航

```text
main.tsx / entrypoints/
  入口、启动优化、CLI 参数分流

screens/ / components/ / hooks/
  交互层（Ink UI）与会话渲染

QueryEngine.ts / query.ts / query/
  推理主循环、预算、停止条件、恢复策略

tools.ts / tools/ / services/tools/
  工具定义、调度与执行适配

utils/permissions/ / services/policyLimits/
  权限与策略治理

commands/ skills/ plugins/ services/mcp/
  扩展生态

remote/ coordinator/ assistant/ voice/
  多形态运行能力
```

## 4. 常见阅读误区

1. **误区：先逐文件细读。**
   - 建议：先抓主干调用链，再回头看局部实现。
2. **误区：把 UI 和核心调度混在一起看。**
   - 建议：先隔离 `QueryEngine/query/tools` 的 runtime 主循环。
3. **误区：忽略权限系统。**
   - 建议：任何工具执行问题，先检查 permissions + policy。

## 5. 一页复盘模板（建议每次改动后填写）

- 我改动的是哪一层？（入口 / 循环 / 工具 / 权限 / 状态 / 扩展）
- 上游调用方是谁？下游依赖是谁？
- 失败时如何回滚或降级？
- 是否影响 telemetry、session 恢复、权限策略？
