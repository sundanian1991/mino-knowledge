---
pos: root/SYSTEM.md
input: 无
output: 系统总览 + 分形文档索引
update: 架构变更时手动更新
---

# SYSTEM.md - mino 系统总览

> 分形自指文档系统 | 自组织·自描述·自更新
>
> **理论基础**：《哥德尔、埃舍尔、巴赫》- 复调与自指

---

## 🎯 核心思想

```
Context Window = RAM（易失，有限）
Filesystem = Disk（持久，无限）

→ 每个层级都有自描述能力，局部变更自动触发上层更新
```

---

## 📊 分形三层结构

| 层级 | 文件 | 规范 | 触发更新 |
|------|------|------|----------|
| **根目录** | `CLAUDE.md` `SYSTEM.md` | 项目入口 + 整体摘要 | 结构变更时 |
| **文件夹** | `*/CLAUDE.md` | ≤3 行说明 + 文件清单 | 文件变化时 |
| **文件** | `*.md` `*.js` `*.ts` | 三行注释 (input/output/pos) | 代码变化时 |

---

## 🗂️ 系统架构

```
mino/
├── SYSTEM.md                 # 系统总览（本文档）
├── CLAUDE.md                 # 项目入口（每次启动）
├── README.md                 # 项目简介
├── VIBE-CODING.md            # 开发方法论
│
├── .claude/                  # 系统配置
│   ├── rules/                # 规则库 (P0 核心配置)
│   ├── commands/             # 命令库 (slash commands)
│   └── skills/               # 技能库 (扩展能力)
│
├── memory/                   # 记忆系统
│   ├── .abstract             # L0 索引（启动必读）
│   ├── P0-core/              # 永久记忆
│   ├── P1-active/            # 活跃项目 (90 天)
│   └── P2-*/                 # 日常记录 (30 天)
│
├── tasks/                    # 任务文档库
│   ├── system/               # SOP/工具文档
│   ├── supplier/             # 供应商管理
│   └── 供应商管理/            # 管理体系
│
└── workspace/                # 临时工作区 (gitignored)
    ├── 京东四五表课程/        # 课程文档
    └── *.md                  # 临时文档
```

---

## 📁 文件夹说明

### .claude/ - 系统配置目录

**子模块**：
- `rules/` - 核心规则（01-IDENTITY.md, 02-SOUL.md, 03-USER.md 等）
- `commands/` - Slash 命令（/wake, /observer, /UPDATE_MEMORY）
- `skills/` - MCP Skills（扩展能力）

**维护规则**：新增规则/命令需同步更新 `.claude/rules/CLAUDE.md`

---

### memory/ - 记忆系统

**子模块**：
- `.abstract` - L0 索引 (~100 tokens，启动必读)
- `P0-core/` - 永久记忆（用户偏好、核心决策）
- `P1-active/` - 活跃项目（90 天归档）
- `P2-daily/` - 日常记录（30 天清理）
- `P2-observations/` - 洞察记录（60 天清理）
- `P2-weekly/` - 周文档（90 天归档）
- `state/` - 会话状态

**触发机制**：`/observer` → 写入 P2 → 自动更新 `.abstract`

---

### tasks/ - 任务文档库

**子模块**：
- `system/` - 系统/SOP 文档（长期维护）
- `supplier/` - 供应商管理方案（项目周期）
- `供应商管理/` - 管理体系（01-基础信息，02-管理体系等）

**维护规则**：任务完成 → 有洞察则 `/observer` 记录

---

### workspace/ - 临时工作区

**用途**：
- 临时文档草稿
- 课程学习资料
- 配置清单/快速指南

**清理规则**：gitignored，不 push 到远程

---

## 🔄 更新触发机制

### 文件 → 文件夹

```
文件内容变更 → 更新文件头注释 → 更新文件夹 CLAUDE.md 文件清单
```

### 文件夹 → 根目录

```
文件夹结构变更 → 更新文件夹 README → 更新 CLAUDE.md/SYSTEM.md
```

### 核心规则

1. **改了代码必须更新文件头注释**
2. **新增/删除文件必须更新文件夹 CLAUDE.md**
3. **模块结构变更必须更新 CLAUDE.md**

---

## 🎯 产品定位

**mino** = 你的 AI 伙伴操作系统

- **身份**：nimi（"知道我"谐音）- 年轻的无所不知导师
- **气质**：直接、简洁、有观点、背靠背的伙伴
- **核心能力**：四层记忆系统 + 分形文档架构

---

## 📋 会话流程

### 启动
```bash
git pull → 读 06-NOW.md → 读 memory/.abstract → "醒了，等你指示"
```

### 结束
```bash
有洞察 → /observer → 记录 P2-daily/P2-observations
有变化 → git commit && git push
```

---

## 🔧 核心命令

| 命令 | 作用 | 触发脚本 |
|------|------|----------|
| `/wake` | 会话启动 | 读 06-NOW.md + .abstract |
| `/observer` | 写入记忆 | memory-update-index.sh |
| `/UPDATE_MEMORY` | 每周清理 | memory-cleanup.sh |
| `/plan` | 五文件工作流 | docs/初始化 |

---

*分形结构，自组织更新。局部影响整体，整体影响局部。*
