---
pos: plans/README.md
input: 所有需求文档
output: 需求索引总览
update: 新增需求时更新
---

# plans/ - 需求文档库

> 一需求一文件夹

## 结构

```
plans/
├── README.md                # 本文件（总索引）
├── 2026-02-24-claude-rename/ # 需求：README 重命名为 CLAUDE.md
│   ├── prompt.md
│   ├── plans.md
│   └── documentation.md
└── YYYY-MM-DD-<主题>/        # 新需求
    ├── prompt.md
    ├── plans.md
    └── documentation.md
```

## 需求列表

| 日期 | 主题 | 状态 |
|------|------|------|
| 2026-02-24 | claude-rename | ✅ 完成 |

---

## 使用方式

```bash
/plan5 把 README.md 重命名为 CLAUDE.md
```

**AI 自动**：
1. AskUserQuestion 确认需求主题
2. 创建 `plans/YYYY-MM-DD-<主题>/`
3. 初始化五文件模板
4. AskUserQuestion 确认需求边界
5. 等待"开始执行"

---

*边跑边调，一需求一文件夹*
