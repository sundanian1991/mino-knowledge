# memory/tasks/ - 任务文档

> 任务全生命周期记录

## 定位

- **作用**：存储任务定义和文档
- **更新**：任务创建/完成时
- **生命周期**：跟随任务周期

## 子目录

```
tasks/
├── CLAUDE.md       # 本文件
├── active/         # 进行中的任务
├── completed/      # 已完成任务
└── system/         # 系统级任务（SOP 等）
```

## 任务格式

```markdown
---
task: 任务名
status: active | completed | blocked
created: YYYY-MM-DD
---
```

## 更新规则

- 任务开始 → 创建文档
- 任务完成 → 更新 status + 记录结果
- 每周五汇总到 weekly