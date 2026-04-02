# LearnClaudeCode

以系统设计视角学习 ClaudeCode，并将本仓库建设为**系统设计分析文档库**。

> 注：所有含“视图/流程图”的文档都提供了ASCII 纯文本可读版本（Mermaid 无法显示时可直接阅读）。

## 文档入口

- 总览：[`docs/claudecode-system-design.md`](docs/claudecode-system-design.md)
- 系统边界与职责逻辑视图：[`docs/logical-view-system-boundary.md`](docs/logical-view-system-boundary.md)
- 术语表：[`docs/glossary.md`](docs/glossary.md)
- 执行模式对比：[`docs/mode-comparison.md`](docs/mode-comparison.md)

## 深挖专题（按推荐阅读顺序）

1. [`docs/deep-dives/01-query-core.md`](docs/deep-dives/01-query-core.md)
2. [`docs/deep-dives/02-tool-execution.md`](docs/deep-dives/02-tool-execution.md)
3. [`docs/deep-dives/03-permission-system.md`](docs/deep-dives/03-permission-system.md)
4. [`docs/deep-dives/04-startup-boot.md`](docs/deep-dives/04-startup-boot.md)
5. [`docs/deep-dives/05-extensibility-ecosystem.md`](docs/deep-dives/05-extensibility-ecosystem.md)
6. [`docs/deep-dives/06-state-and-storage.md`](docs/deep-dives/06-state-and-storage.md)
7. [`docs/deep-dives/07-observability-and-telemetry.md`](docs/deep-dives/07-observability-and-telemetry.md)
8. [`docs/deep-dives/08-failure-recovery-patterns.md`](docs/deep-dives/08-failure-recovery-patterns.md)

## 实验与模板

- 并发调度实验剧本：[`docs/experiments/02-tool-concurrency-experiments.md`](docs/experiments/02-tool-concurrency-experiments.md)
- 深挖专题模板：[`docs/templates/deep-dive-template.md`](docs/templates/deep-dive-template.md)


## 提交前同步建议（避免冲突）

```bash
# 1) 同步主线（按你的远程配置替换）
git fetch origin
git rebase origin/main

# 2) 处理冲突后继续
git add <resolved-files>
git rebase --continue

# 3) 再提交增量修改
git commit -m "docs: incremental update"
```
