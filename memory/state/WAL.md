---
pos: memory/state/WAL.md
input: 会话中的关键事件
output: 持久化日志
update: 实时追加
---

# Write-Ahead Log

> 预写日志 - 关键操作先写日志再执行，确保可恢复

---

## 📝 日志格式

```
[TIMESTAMP] [TYPE] [CONTENT] [STATUS] [TAGS]
```

**TYPE 类型**:
- `START` - 会话启动
- `END` - 会话结束
- `TOPIC` - 话题切换
- `DECISION` - 重要决策
- `ACTION` - 执行动作
- `CHECKPOINT` - 检查点

**STATUS**:
- `pending` - 待处理
- `completed` - 已完成
- `interrupted` - 中断
- `needs-follow-up` - 需跟进

---

## 📜 日志记录

| 时间戳 | 类型 | 内容 | 状态 | 标签 |
|--------|------|------|------|------|
| 2026-03-18T21:30:00 | START | 会话启动 | completed | - |
| 2026-03-18T21:35:00 | TOPIC | 讨论 Proactive-Agent 机制借鉴 | completed | #proactive |
| 2026-03-18T21:40:00 | DECISION | 实现 Heartbeat + WAL + 自我修复 | completed | #design |
| 2026-03-18T21:45:00 | ACTION | 开始 Phase 1 实现 | in-progress | #impl |

---

## 🔍 需要跟进的事项

| 时间戳 | 内容 | 下次行动 |
|--------|------|----------|
| - | - | - |

---

## ♻️ 日志维护

- 自动归档：超过 30 天的记录移动到 `memory/P1-active/wal-archive/`
- 清理周期：每月一次（/UPDATE_MEMORY 时）

---

*最后更新：2026-03-18*
