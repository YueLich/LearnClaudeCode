# 深挖 07：可观测性与遥测（Observability & Telemetry）

## 1. 分析目标

说明 ClaudeCode 如何观测系统行为、记录关键事件、度量成本与性能，并用于调试和治理。

## 2. 可观测链路（纯文本）

```text
执行事件
  -> 日志/诊断输出
  -> 指标与埋点
  -> 会话记录与回放
  -> 故障定位与性能分析
```

## 3. 关键机制

- **成本观测**：token/cost 累计与展示。
- **启动观测**：startup profiler checkpoint 度量启动瓶颈。
- **事件埋点**：关键路径行为上报（功能开关、策略、能力加载等）。
- **会话轨迹**：通过 transcript 与 structured message 流回溯执行过程。

## 4. 真实代码调用路径（函数级追踪）

- 成本链路：
  - `cost-tracker.ts`（成本聚合）
  - `QueryEngine.ts`（累计 usage）
- 启动性能链路：
  - `utils/startupProfiler.ts::profileCheckpoint`
  - `main.tsx` / `entrypoints/cli.tsx` 调用 checkpoint
- 埋点链路：
  - `services/analytics/index.ts::logEvent`
  - 各子系统在关键分支上报事件

## 5. 设计收益

- 对性能、成本、安全和用户体验提供统一观测基础。
- 让“问题复现”从主观描述转向可验证轨迹。

## 6. ASCII 版观测链路

```text
[Execution Events]
      |
      v
[Logs/Diagnostics]
      |
      v
[Metrics/Telemetry]
      |
      v
[Session Replay]
      |
      v
[Debugging & Perf Analysis]
```
