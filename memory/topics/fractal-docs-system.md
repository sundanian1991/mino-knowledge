---
pos: memory/topics/fractal-docs-system.md
input: Vibe Coding + 分形自指理念
output: mino 三层文档架构
created: 2026-02-23
updated: 2026-03-07
status: active
---

# 分形文档系统

> 不是让 AI 更聪明，是给它一个配得上它能力的工作环境

---

## 核心思想

```
Context Window = RAM（易失，有限）
Filesystem = Disk（持久，无限）
→ 重要的东西写进文件里。
```

---

## 三层结构

| 层级 | 文件 | 作用 | 加载时机 |
|------|------|------|----------|
| 项目级 | `CLAUDE.md` `SYSTEM.md` | 项目入口 + 整体摘要 | 每次启动 |
| 模块级 | `*/CLAUDE.md` | 模块说明 + 文件清单 | 进入模块时 |
| 文件级 | `---` 注释块 | 文件功能 + 依赖关系 | 按需加载 |

---

## 文件头规范

```markdown
---
pos: 路径
input: 输入来源
output: 输出生成
update: 更新方式
---
```

---

## 核心规则

1. 改了代码必须更新文件头注释
2. 新增/删除文件必须更新文件夹 CLAUDE.md
3. 模块结构变更必须更新 CLAUDE.md/SYSTEM.md

---

## 历史

- **2026-02-23**：引入分形文档系统
- **2026-03-07**：Token 消耗优化完成，更新 .abstract
