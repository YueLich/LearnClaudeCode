# 深挖 08：故障与恢复模式（Failure Recovery Patterns）

## 1. 分析目标

总结 ClaudeCode 在 query 与工具执行中常见故障类型及其恢复策略，形成可复用模式库。

## 2. 故障恢复总览（纯文本）

```text
错误发生
  -> 错误分类（可重试/不可重试/预算相关/权限相关）
  -> 选择恢复策略（重试/compact/降级/中断）
  -> 记录轨迹
  -> 返回可解释结果
```

## 3. 典型恢复模式

- **上下文过长**：触发 compact/微压缩后重试。
- **输出上限触发**：max_output_tokens 恢复循环。
- **工具失败**：封装为 tool_result error 回注模型继续推理。
- **权限拒绝**：在权限层 deny 并保留可解释消息。
- **可重试 API 错误**：分类后执行 retry 路径。

## 4. 真实代码调用路径（函数级追踪）

- query 恢复链路：
  - `query.ts`（主循环中的恢复分支）
  - `services/compact/*`（compact 相关逻辑）
- 错误分类链路：
  - `services/api/errors.ts`
  - `services/api/withRetry.ts`
- 停止与失败钩子：
  - `query/stopHooks.ts::handleStopHooks`
  - `utils/hooks/*`（执行后失败处理）

## 5. 模式库价值

- 统一“错误 -> 策略 -> 结果”语言，降低维护心智负担。
- 为后续自动化故障演练和回归验证提供框架。

## 6. ASCII 版故障恢复链路

```text
[Error]
   |
   v
[Classify]
   |
   v
[Choose Recovery]
   |
   +--> retry
   +--> compact
   +--> degrade
   +--> interrupt
   |
   v
[Record Trace] -> [Return Explainable Result]
```
