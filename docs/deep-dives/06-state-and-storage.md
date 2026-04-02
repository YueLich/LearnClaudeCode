# 深挖 06：状态与存储（State & Storage）

## 1. 分析目标

解释 ClaudeCode 如何管理会话状态、消息持久化、文件状态缓存，以及这些机制如何支撑长时任务与恢复能力。

## 2. 状态分层视图（纯文本）

```text
运行时状态（内存）
  - AppState / QueryEngine mutableMessages / readFileState

会话持久化（磁盘）
  - transcript / session metadata / snapshots

外部状态（环境）
  - cwd/git/worktree/policy settings
```

## 3. 关键机制

- **会话内状态**：由 `QueryEngine` 持有 messages、usage、permission 拒绝记录等。
- **文件状态缓存**：减少重复读取与上下文抖动。
- **会话存储**：通过 session storage 记录消息与会话元数据，支持恢复。
- **快照与历史**：文件历史快照能力帮助对比与回放。

## 4. 真实代码调用路径（函数级追踪）

- 会话状态主路径：
  - `QueryEngine.ts::submitMessage`
  - `state/AppStateStore.ts`（全局状态容器）
- 会话存储路径：
  - `utils/sessionStorage.ts::recordTranscript`
  - `utils/sessionStorage.ts::flushSessionStorage`
- 文件状态路径：
  - `utils/fileStateCache.ts`（读取缓存）
  - `utils/fileHistory.ts::fileHistoryMakeSnapshot`

## 5. 设计收益

- 将“对话内容”和“运行时控制状态”分离，降低耦合。
- 支持长会话、恢复、回放与诊断。
- 为可观测与错误恢复提供状态基础。

## 6. ASCII 版状态分层图

```text
+--------------------+
| Runtime State      |
| (AppState/Engine)  |
+--------------------+
          |
          v
+--------------------+
| Session Storage    |
| (transcript/meta)  |
+--------------------+
          |
          v
+--------------------+
| External Env State |
| (cwd/git/policy)   |
+--------------------+
```
