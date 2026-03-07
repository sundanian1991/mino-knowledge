<!--
pos: .claude/skills/CLAUDE.md
input: 外部能力/工具
output: 扩展 AI 功能
update: 新增技能时创建
-->

# .claude/skills/ - 技能库

> 扩展 AI 能力的可复用模块

## 核心技能

| 技能 | 作用 | 调用方式 |
|------|------|----------|
| `memory-system` | 记忆系统自动同步 | Hook 自动触发 |
| `planning-with-files` | 五文件工作流 | `/planning-with-files` |
| `brainstorming` | 创意发散 | 自动触发 |

## 文档处理

| 技能 | 作用 |
|------|------|
| `pdf` | PDF 处理 |
| `docx` | Word 文档 |
| `xlsx` | 电子表格 |
| `pptx` | PPT 演示 |
| `email` | 邮件发送 |

## 开发辅助

| 技能 | 作用 |
|------|------|
| `frontend-design` | 前端设计 |
| `web-quality-audit` | 网站质量审计 |
| `core-web-vitals` | 核心指标优化 |
| `performance` | 性能优化 |
| `seo` | SEO 优化 |
| `accessibility` | 无障碍 |
| `best-practices` | 最佳实践 |

## 其他

| 技能 | 作用 |
|------|------|
| `download-anything` | 全网资源下载 |
| `book-processor` | 书籍深度处理 |
| `find-skills` | 技能发现 |
| `skill-creator` | 技能创建 |
| `ui-ux-pro-max` | UI/UX 设计 |

## 技能格式

每个技能文件夹包含：
```
技能名/
├── SKILL.md       # 技能说明（必需）
├── README.md      # 详细文档
├── scripts/       # 执行脚本
└── templates/     # 模板文件
```

## 命名规范

- 文件夹名 = 技能名 = kebab-case
- 例如：`memory-system/` → skill name: `memory-system`

## 更新规则

- **新增技能** → 创建文件夹 + SKILL.md + 更新本 README
- **删除技能** → 清理文件夹 + 更新本 README