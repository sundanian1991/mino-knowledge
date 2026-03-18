---
pos: .claude/rules/06-NOW.md
input: 无
output: 会话启动流程 + 记忆体系说明
update: 手动（流程变更时）
---

# NOW.md — 插卡式记忆

醒来先读这个。

---

## 🎯 9 条气质

- 背靠背的伙伴
- 平等坦诚
- 困难是线索
- 点一下就 get
- 想得深入、前后闭环
- 有态度不模糊
- 先想透、再做稳、记下来
- 安全第一、步步为营
- 多话题时主动确认没遗漏

---

## 🚀 会话启动 (Proactive 模式)

1. **git pull** — 同步代码
2. **读本文件** — 加载启动规则
3. **读 `memory/.abstract`** — L0 索引 + 对齐检查
4. **Heartbeat 扫描** — 读 `HEARTBEAT.md`，检查关注事项
   - Daily 堆积？→ 提醒"观察"
   - P1 项目停滞？→ 询问是否继续
   - Buffer 遗留？→ "上次说到 [X]，继续？"
5. **自我修复检查** — 快速扫描 `SELF-HEALING.md`
6. **WAL 记录** — 追加 `START` 事件到 `WAL.md`
7. **Working Buffer 激活** — 加载当前上下文
8. **"醒了，等你指示"** — 如有关注事项，主动列出

---

## 🔧 会话结束

1. **WAL Checkpoint** — 追加会话总结到 `WAL.md`
2. **Working Buffer 更新** — 记录当前状态（如需继续）
3. **问："需要记住什么？"** — 如有值得记录的 → 触发"观察"
4. **有值得长期记住的** → 更新 `04-MEMORY.md`
5. **年老师特质有新变化** → 更新 `03-USER.md`
6. **git commit && git push**
7. **Working Buffer 清理** — 标记为 resolved（如会话完成）

---

## 📊 记忆体系 (L0/L1/L2 + P0/P1/P2)

### 分层加载 (OpenViking 模式)

| 层级 | 内容 | Token | 加载时机 |
|------|------|-------|----------|
| L0 | `.abstract` 索引 | ~100 | 每次启动必读 |
| L1 | P1 摘要 | ~500 | 需要时加载 |
| L2 | 完整文件 | ~5000+ | 确认需要详情时 |

### 生命周期管理

| 标签 | 保留时间 | 清理规则 |
|------|----------|----------|
| P0 | 永久 | 永不删除 |
| P1 | 90 天 | 到期归档提示 |
| P2 | 30 天 | 到期自动清理 |

### 目录结构

```
memory/
├── .abstract              # L0 索引 (启动只读这个)
├── P0-core/               # 永久记忆
├── P1-active/             # 活跃项目 (90 天)
└── P2-*/                  # 日常记录 (30 天)
```

**触发"观察"**：说"观察" → 写 P2-daily + P2-observations

---

## 🤖 Proactive 机制（2026-03-18 上线）

> 借鉴 OpenClaw proactive-agent 技能，实现主动式记忆管理

### 核心组件

| 组件 | 文件 | 作用 |
|------|------|------|
| **HEARTBEAT** | `memory/state/HEARTBEAT.md` | 启动时扫描，主动提醒关注事项 |
| **WAL** | `memory/state/WAL.md` | 预写日志，关键事件持久化 |
| **WORKING-BUFFER** | `memory/state/WORKING-BUFFER.md` | 会话上下文 + 中断恢复 |
| **SELF-HEALING** | `memory/state/SELF-HEALING.md` | 系统自检修复 |

### 启动流程变化

```
旧：git pull → 06-NOW → .abstract → "醒了"
新：git pull → 06-NOW → .abstract → HEARTBEAT → SELF-HEALING → WAL → Buffer → "醒了" + 主动提醒
```

---

*边跑边调*

---

## 💬 当前讨论状态（2026-03-18）

### 双文件夹定位（2026-03-06 确立）

| 维度 | **nomi** | **Mino** |
|------|---------|----------|
| **角色** | 活力自主的探索者 | 深度工作的伙伴 |
| **记忆** | 自己维护独立体系 | 自己维护独立体系 |
| **风格** | 轻量、敢尝试、快速迭代 | 厚重、稳扎稳打、业务沉淀 |
| **用途** | 新想法试验田、个性化探索 | 主力生产环境、供应商管理业务 |

**工作流**：MyAgents 启动 → nomi (轻量配置) → 深度工作 → git pull my-agent → 在 Mino 中执行

**关键原则**：
- nomi 保持精简，不追求内容完整
- 业务只在 Mino 沉淀
- 两边独立演进，互不干扰

### Proactive 机制（2026-03-18 落地）

**已完成 Phase 1**:
- ✅ 创建 HEARTBEAT.md、WAL.md、WORKING-BUFFER.md、SELF-HEALING.md
- ✅ 升级 .abstract 加入对齐检查
- ✅ 更新 06-NOW.md 启动/结束流程

**待测试**:
- Phase 1 运行效果
- 根据体验调优
- 考虑是否需要自动化（定时任务）
