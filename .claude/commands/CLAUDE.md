<!--
pos: .claude/commands/CLAUDE.md
input: 用户需求
output: Slash 命令定义
update: 新增命令时创建
-->

# .claude/commands/ - Slash 命令库

> 用户通过 `/命令` 调用的功能集合

## 文件结构

```
.claude/commands/
├── CLAUDE.md      # 本文件
├── wake.md        # /wake - 会话启动
└── observer.md    # /observer - 洞察分析
```

## 命令列表

| 命令 | 作用 | 触发时机 |
|------|------|----------|
| `/wake` | 会话启动 | 每次会话开始 |
| `/observer` | 手动触发洞察分析 | 有重要发现时 |