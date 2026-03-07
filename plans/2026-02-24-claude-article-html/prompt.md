---
pos: plans/2026-02-24-claude-article-html/prompt.md
input: docs/use Claude code .md 文章
output: 审美好看的 HTML 网页
update: 需求变更时更新
---

# 项目定义：Claude Code 实战文章 HTML 网页

## 目标

将 5000+ 字的 Claude Code 实战文章转化为一个**审美好看的单文件 HTML 网页**，支持中等交互（滚动动画 + 目录导航 + 代码块复制）。

---

## 排除项

- [ ] 不需要后端服务（纯静态 HTML）
- [ ] 不需要用户登录/账号系统
- [ ] 不需要复杂搜索功能
- [ ] 不需要主题切换
- [ ] 不使用外部 JS 框架（React/Vue 等）

---

## 交付物

1. **单文件 HTML** - 所有 CSS/JS 内嵌
2. **深色现代设计** - 极客感，高对比渐变
3. **目录导航** - 左侧固定，滚动高亮
4. **代码块复制** - 一键复制按钮
5. **滚动动画** - fade-in 效果

---

## 完成标准

- [ ] 文章 10 个章节完整呈现
- [ ] 目录导航滚动高亮
- [ ] 代码块可一键复制
- [ ] 响应式设计（桌面 + 移动）
- [ ] 无 console 错误
- [ ] Lighthouse 性能评分 > 80

---

## 约束条件

- **技术栈**：HTML5 + Tailwind CSS（内嵌）+ 原生 JavaScript
- **设计**：深色现代风，p-6/p-8 间距，rounded-xl 圆角
- **位置**：`workspace/claude-article/index.html`
- **单文件**：所有代码封装在一个 HTML 文件中
- **质量红线**：单文件≤800 行、单函数≤30 行、嵌套≤3 层
