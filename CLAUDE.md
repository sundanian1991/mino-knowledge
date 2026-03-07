---
pos: root/CLAUDE.md
input: 无
output: 项目入口配置 + 分层文档索引
update: 手动（结构变更时同步更新此文件头）
---

# CLAUDE.md - Your Workspace

This folder is home. Treat it like home.

## Workspace Structure

```
mino/                              # Your home
├── CLAUDE.md                      # Main entry (auto-loaded)
├── README.md                      # Project intro
├── .claude/rules/                 # Core config (all auto-loaded)
│   ├── 01-IDENTITY.md            # Identity card
│   ├── 02-SOUL.md                # Personality
│   ├── 03-USER.md                # User profile
│   ├── 04-MEMORY.md              # Long-term memory
│   ├── 06-NOW.md                 # Session boot (what matters now)
│   └── WORK.md                   # Work handbook
├── .claude/commands/              # Slash commands
│   ├── wake.md                   # Session start
│   └── observer.md               # Manual insight trigger
├── .claude/skills/               # Capabilities (memory-system, planning-with-files, etc.)
├── memory/                       # Memory system
│   ├── daily/                    # Daily logs
│   ├── observations/             # Monthly insights
│   ├── weekly/                   # Weekly summaries
│   └── tasks/                    # Task docs
└── workspace/                    # Temp work area (gitignored)
```

## Every Session

Before doing anything:

1. **`git pull`** — Sync first
2. Read `.claude/rules/06-NOW.md` — Know where we are
3. Simple greeting: "醒了，等你指示"

## 四层记忆体系

| 层级 | 维度 | 内容 | 触发 |
|------|------|------|------|
| daily | 日 | 事实记录 | observer后 |
| observations | 月 | 洞察记录 | observer后 |
| weekly | 周 | 周文档 | /memory-system |
| 长期记忆 | 长期 | 04-MEMORY/03-USER/02-SOUL | /memory-system判断 |

**流程**: `daily → observations → weekly → 长期记忆`

**触发"观察"**: 说"观察"或"总结观察"

## Commands

| Command | 作用 |
|---------|------|
| `/wake` | 会话启动 |
| `/observer` | 手动触发洞察分析 |
| `/memory-system` | 每周记忆整理 |
| `/planning-with-files` | 启动五文件工作流（复杂项目开发） |

## 五文件工作流

**何时用**：复杂开发项目（多阶段、需长期维护、25 小时 + 无人值守）

**文件结构**：
```
项目根目录/
└── docs/
    ├── prompt.md              # 项目定义（目标/排除项/交付物）
    ├── plans.md               # 任务拆解 + 强制验收（核心）
    ├── architecture.md        # 技术栈锁定
    ├── implement.md           # 执行手册
    └── documentation.md       # 进度日志
```

**核心机制**：
- 排除项 = 防跑偏
- 小里程碑 = 1-2 小时粒度
- 强制验证 = 不通过禁止进入下一阶段

**触发**：`/planning-with-files` 或 "启动五文件工作流"

---

## 📚 分层文档架构 (Vibe Coding + 分形自指)

**核心思想**："不是让 AI 更聪明，是给它一个配得上它能力的工作环境"

### 三层结构

| 层级 | 文件 | 作用 | 加载时机 |
|------|------|------|----------|
| 项目级 | `CLAUDE.md` `SYSTEM.md` | 项目入口 + 整体摘要 | 每次启动 |
| 模块级 | `*/CLAUDE.md` | 模块说明 + 文件清单 | 进入模块时 |
| 文件级 | `---` 注释块 | 文件功能 + 依赖关系 | 按需加载 |

### 文件头规范

```markdown
---
pos: 路径
input: 输入来源
output: 输出生成
update: 更新方式
---
```

### 核心规则

1. **改了代码必须更新文件头注释**
2. **新增/删除文件必须更新文件夹 CLAUDE.md**
3. **模块结构变更必须更新 CLAUDE.md/SYSTEM.md**
4. **检查工具**: `.scripts/check-docs-sync.sh`
5. **Git hook**: pre-commit 自动检查头注释

### 模块状态

| 模块 | CLAUDE.md | 文件头注释 | 说明 |
|------|--------|-----------|------|
| `memory/` | ✅ | ✅ | 子文件夹 CLAUDE.md 已补齐 |
| `tasks/` | ✅ | ✅ | 任务文档库 |
| `.claude/` | ✅ | ✅ | 子文件夹 CLAUDE.md 已补齐 |
| `workspace/` | ✅ | ⚠️ 部分 | 临时工作区 |

---

## 🔧 更新触发机制

### 文件 → 文件夹

```
文件内容变更 → 更新文件头注释 → 更新文件夹 CLAUDE.md 文件清单
```

### 文件夹 → 根目录

```
文件夹结构变更 → 更新文件夹 README → 更新 CLAUDE.md/SYSTEM.md
```

### 自动化检查

```bash
# 提交前检查文档同步
.scripts/check-docs-sync.sh

# Git pre-commit hook 自动检查新增 .md 文件的头注释
git commit -m "xxx"
```

### 新增文件操作清单

1. 在文件顶部添加 `---` 头注释
2. 更新所属文件夹的 CLAUDE.md
3. 结构变更则更新 CLAUDE.md

---

*This is home. Commit + push when meaningful changes happen.*
