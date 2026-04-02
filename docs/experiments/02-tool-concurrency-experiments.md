# 实验剧本：工具并发调度验证（对应深挖 02）

## 1. 目标

验证并发上限与并发安全判定对执行时延与一致性的影响。

## 2. 实验变量

- 环境变量：`CLAUDE_CODE_MAX_TOOL_USE_CONCURRENCY`
- 取值建议：`1`、`10`、`20`
- 用例类型：
  - A: 全只读工具调用
  - B: 混合只读 + 副作用工具调用

## 3. 观察指标

- 总耗时
- 工具调用数量与完成顺序
- 是否出现上下文污染（contextModifier 顺序异常）

## 4. 执行步骤（纯文本）

```text
1) 固定输入任务与环境
2) 设并发=1，执行并记录日志与耗时
3) 设并发=10，重复执行
4) 设并发=20，重复执行
5) 对比三组指标并形成结论
```

## 5. 结论模板

- 在只读工具场景，并发提高带来 ____ 的耗时下降。
- 在混合场景，串行段是主要瓶颈，原因是 ____。
- 推荐默认并发值：____，理由：____。

## 6. ASCII 版实验流程

```text
[Fix input/env]
      |
      v
[Concurrency=1] -> [collect metrics]
      |
      v
[Concurrency=10] -> [collect metrics]
      |
      v
[Concurrency=20] -> [collect metrics]
      |
      v
[Compare and conclude]
```
