---
pos: memory/topics/proactive-agent.md
input: OpenClaw proactive-agent 技能借鉴
output: nomi Proactive 机制实现
created: 2026-03-18
updated: 2026-03-18
status: active
---

# Proactive 机制

> 借鉴 OpenClaw proactive-agent 技能，实现主动式记忆管理

---

## 核心组件

| 组件 | 文件 | 作用 |
|------|------|------|
| **HEARTBEAT** | `memory/state/HEARTBEAT.md` | 启动时扫描，主动提醒关注事项 |
| **WAL** | `memory/state/WAL.md` | 预写日志，关键事件持久化 |
| **WORKING-BUFFER** | `memory/state/WORKING-BUFFER.md` | 会话上下文 + 中断恢复 |
| **SELF-HEALING** | `memory/state/SELF-HEALING.md` | 系统自检修复 |

---

## 启动流程变化

```
旧：git pull → 06-NOW → .abstract → "醒了"
新：git pull → 06-NOW → .abstract → HEARTBEAT → SELF-HEALING → WAL → Buffer → "醒了" + 主动提醒
```

---

## 实现时间线

| 日期 | 事件 |
|------|------|
| 2026-03-18 | Phase 1 上线：创建四个核心文件，更新 06-NOW 流程 |

---

## 待办

- [ ] Phase 1 运行效果测试
- [ ] 根据体验调优
- [ ] 考虑是否需要自动化（定时任务）
