---
pos: VIBE-CODING.md
input: Vibe Coding 理念
output: mino 项目文档架构说明
update: 架构变更时更新
---

# Vibe Coding 在 mino 的实践

> 来源：Vibe Coding 分层文档架构
>
> 核心理念："不是让 AI 更聪明，是给它一个配得上它能力的工作环境"

---

## 🎯 核心问题

**AI 在复杂项目中的困境**：
```
项目太大 → 看不全 → 记不住 → 上下文断裂 → 乱改代码
```

**Vibe Coding 的方案**：
```
分层文档 → 按需加载 → 规则约束 → 自动同步 → 可持续维护
```

---

## 📚 三层文档架构

### 项目级 (Project Level)

| 文件 | 作用 | 加载时机 |
|------|------|----------|
| `CLAUDE.md` | 项目入口 + 整体摘要 | 每次启动 |

**文件头示例**：
```markdown
---
pos: root/CLAUDE.md
input: 无
output: 项目入口配置
update: 手动（结构变更时）
---
```

### 模块级 (Module Level)

| 模块 | README | 说明 |
|------|--------|------|
| memory/ | ✅ | L0/L1/L2 分层记忆 |
| tasks/ | ✅ | 任务文档库 |
| .claude/rules/ | ✅ | 系统规则库 |

**文件头示例**：
```markdown
---
pos: memory/
input: 对话内容、观察洞察
output: L0/L1/L2 分层记忆文件
update: 自动（observer/UPDATE_MEMORY 触发脚本）
---
```

### 文件级 (File Level)

**关键文件加文件头**：
```markdown
---
pos: .claude/rules/06-NOW.md
input: 无
output: 会话启动流程 + 记忆体系说明
update: 手动（流程变更时）
---
```

---

## 🔧 mino 特色实现

### 1. L0/L1/L2 分层记忆 (OpenViking)

| 层级 | 内容 | Token | 加载时机 |
|------|------|-------|----------|
| L0 | `.abstract` 索引 | ~100 | 每次启动 |
| L1 | P1 摘要 | ~500 | 需要时 |
| L2 | 完整文件 | ~5000+ | 确认需要详情 |

### 2. P0/P1/P2 生命周期

| 级别 | 保留时间 | 清理规则 |
|------|----------|----------|
| P0 | 永久 | 永不删除 |
| P1 | 90 天 | 到期归档提示 |
| P2 | 30 天 | 到期自动清理 |

### 3. 自动同步机制

**实现方式**：Skill Hook (PostToolUse)

**触发条件**：
- 检测到 `memory/` 目录下 `.md` 文件被 Write/Edit
- 自动触发 `memory-update-index.sh` 重建 `.abstract` 索引

**SKILL.md 配置**：
```yaml
hooks:
  PostToolUse:
    - matcher: "Write|Edit"
      pathPattern: "memory/.*\\.md$"
      hooks:
        - type: command
          command: memory-update-index.sh
```

**效果**：
- 改代码 → 自动检查 → 更新索引
- 无需手动维护 `.abstract`

---

**脚本**：
- `memory-update-index.sh` - 轻量索引更新（observer 用）
- `memory-cleanup.sh` - 完整清理 + 重建索引（UPDATE_MEMORY 用）

---

## 📋 核心规则

### 文档更新规则

1. **改了代码必须更新对应文档**
2. **新建模块必须加 CLAUDE.md**
3. **删除文件必须从索引移除**

### 文件头规范

```markdown
---
pos: 文件路径
input: 输入来源
output: 输出生成
update: 更新方式（自动/手动）
---
```

### 模块 README 模板

```markdown
# 模块名 - 简短说明

> 一句话定位 + 来源（如果有）

---

## 作用

**解决什么问题**：
- 痛点 1 → 方案 1
- 痛点 2 → 方案 2

---

## 结构

```
模块/
├── 文件 1.md
└── 文件 2.md
```

---

## 相关命令

| 命令 | 作用 |
|------|------|
| `/xxx` | 说明 |

---

## 维护规则

1. 规则 1
2. 规则 2
```

---

## 🎯 价值

| 价值 | 说明 |
|------|------|
| 快速上手 | 新 AI 看 README 就懂结构 |
| 按需加载 | 不一次性读全量，节省 Token |
| 可持续 | 改了代码知道更新哪里 |
| 自动化 | 记忆系统自动同步索引 |

---

## 📊 完整结构

```
mino/
├── CLAUDE.md                        # 项目级入口
├── memory/
│   ├── CLAUDE.md                    # 模块级：记忆系统
│   └── .abstract                    # L0 索引
├── tasks/
│   └── CLAUDE.md                    # 模块级：任务库
└── .claude/
    ├── rules/
    │   ├── CLAUDE.md                # 模块级：规则库
    │   └── 06-NOW.md                # 会话启动
    └── commands/
        ├── wake.md                  # /wake
        ├── observer.md              # /observer
        └── UPDATE_MEMORY.md         # /UPDATE_MEMORY
```

---

## 🔗 相关文档

- `CLAUDE.md` - 项目入口
- `memory/CLAUDE.md` - 记忆系统
- `tasks/CLAUDE.md` - 任务库
- `.claude/rules/CLAUDE.md` - 系统规则

---

*边跑边调，持续演进*
