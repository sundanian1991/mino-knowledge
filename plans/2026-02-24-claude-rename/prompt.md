---
pos: plans/2026-02-24-claude-rename/prompt.md
input: 文档同步机制需求
output: 文档自动同步执行机制
update: 需求变更时更新
---

# 项目定义：文档同步机制

## 目标

**建立文档同步机制**，保证：
1. 每个文件夹有 CLAUDE.md 说明其作用
2. 每个 .md 文件有 `---` 头注释（input/output/pos/update）
3. 文件变更 → 自动触发更新对应文档
4. 提交前自动检查文档同步状态

## 排除项

- [ ] 不做复杂的文档生成工具
- [ ] 不引入额外的依赖包
- [ ] 不改变现有文件夹结构

## 交付物

1. ✅ 所有文件夹 CLAUDE.md 已补齐
2. ✅ 所有 .md 文件头注释已补充
3. ✅ `.scripts/check-docs-sync.sh` 可运行
4. ✅ Git 提交前 hook 自动检查

## 完成标准

- [ ] `check-docs-sync.sh` 运行无 ❌
- [ ] 新增文件时有机制提醒更新文档
- [ ] 文档机制写入 CLAUDE.md

## 技术栈

- Bash 脚本
- Git hooks

## 约束条件

- 保持简单，不引入复杂工具
- 脚本执行时间 < 5 秒
