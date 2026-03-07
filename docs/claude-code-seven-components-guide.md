# Claude Code 七大组件完全指南

> AI 开发参考文档 · 2026 版
>
> **用途**：为 AI 助手提供 Claude Code 完整功能参考，支持代码生成、配置编写、问题解答
>
> **来源**：基于 Anthropic 官方文档和实战经验整理

---

## 目录

1. [CLAUDE.md - 项目记忆](#1-claudemd---项目记忆)
2. [Hooks - 自动化任务](#2-hooks---自动化任务)
3. [SubAgents - 任务分解](#3-subagents---任务分解)
4. [Commands - 快捷命令](#4-commands---快捷命令)
5. [Skills - 可复用技能](#5-skills---可复用技能)
6. [MCP Servers - 外部连接](#6-mcp-servers---外部连接)
7. [Plugins - 打包工作流](#7-plugins---打包工作流)
8. [组件组合实战](#8-组件组合实战)
9. [避坑清单](#9-避坑清单)
10. [快速查阅表](#10-快速查阅表)

---

## 1. CLAUDE.md - 项目记忆

### 1.1 是什么

CLAUDE.md 是 Claude Code 的项目上下文文件，会在每次会话开始时自动加载到对话中。

**解决痛点**：每次都要重复交代项目背景、技术栈、代码规范。

### 1.2 文件位置

| 位置 | 作用范围 | 推荐场景 |
|------|----------|----------|
| `项目根目录/CLAUDE.md` | 当前项目 | 团队共享、项目特定配置 |
| `~/.claude/CLAUDE.md` | 全局所有项目 | 个人偏好、通用规范 |

### 1.3 标准模板

```markdown
# 项目概述

[用 1-2 句话描述项目是做什么的]

---

## 技术栈

- **语言**: TypeScript 5.x / Python 3.11
- **框架**: NestJS / FastAPI / React
- **包管理**: pnpm / uv / npm
- **数据库**: PostgreSQL / MongoDB

---

## 项目命令

```bash
# 开发
npm run dev          # 启动开发服务器
npm run build        # 构建生产版本
npm run preview      # 预览生产构建

# 测试
npm run test         # 运行单元测试
npm run test:coverage  # 生成覆盖率报告
npm run test:e2e     # 运行端到端测试

# 代码质量
npm run lint         # ESLint 检查
npm run lint:fix     # 自动修复格式问题
npm run format       # Prettier 格式化
npm run typecheck    # TypeScript 类型检查
```

---

## 代码规范

### TypeScript

- 使用 strict 模式
- 优先使用 ES 模块语法 (import/export)
- 类型定义放在 `types/` 目录
- 组件用函数式，不用类组件
- 使用 async/await，避免 Promise 链

### 文件组织

- 组件放在 `components/`，每个组件独立文件夹
- 工具函数放在 `lib/` 或 `utils/`
- API 类型定义放在 `types/api.ts`
- 全局样式放在 `styles/globals.css`

### 命名约定

- 组件：PascalCase (UserProfile.tsx)
- 函数/变量：camelCase (getUserById)
- 常量：UPPER_SNAKE_CASE (MAX_RETRY_COUNT)
- 测试文件：*.test.ts 或 *.spec.ts

---

## 工作流

### 开发流程

1. 创建功能分支：`git checkout -b feat/xxx`
2. 实现功能并编写测试
3. 运行 `npm run lint && npm run test`
4. 提交代码：`git commit -m "feat: xxx"`
5. 推送并创建 PR

### 提交规范

```
feat: 新功能
fix: 修复 bug
docs: 文档更新
style: 代码格式调整
refactor: 重构
test: 测试相关
chore: 构建/工具链相关
```

### 测试要求

- 优先写单元测试
- 覆盖率目标：80%+
- 测试用例命名：`should [expected behavior] when [condition]`

---

## 关键文件

| 文件 | 说明 |
|------|------|
| `src/main.ts` | 应用入口 |
| `src/app.module.ts` | 根模块 |
| `prisma/schema.prisma` | 数据库模型 |
| `docker-compose.yml` | 本地开发环境 |

---

## 注意事项

- 修改数据库模型后必须运行 `pnpm prisma migrate dev`
- API 变更后同步更新 OpenAPI 文档
- 敏感配置从 `.env` 读取，不要硬编码
```

### 1.4 最佳实践

**✅ 推荐**：
- 保持简洁，只写关键信息
- 命令要可执行、可验证
- 规范要具体，避免模糊描述
- 定期更新，保持与实际一致

**❌ 避免**：
- 写成长篇大论的文档
- 包含过时的命令或配置
- 规范与实际代码不符
- 频繁修改导致 AI 困惑

### 1.5 迭代优化

CLAUDE.md 不是一次性的，需要根据使用效果持续调整：

```markdown
<!-- 第 1 版：基础命令 -->
- npm run dev: 启动开发服务器
- npm run test: 运行测试

<!-- 第 2 版：添加规范 -->
# 代码规范
- 使用 TypeScript strict 模式
- 优先使用 ES 模块

<!-- 第 3 版：添加强调 -->
# 重要规则
IMPORTANT: 提交前必须运行 lint 和 test
MUST: 所有公共方法必须有类型注解
```

---

## 2. Hooks - 自动化任务

### 2.1 是什么

Hooks 允许在 Claude Code 的特定事件发生时自动执行 shell 命令。

**解决痛点**：每次代码修改后都要手动运行 lint、test、format，机械重复。

### 2.2 配置文件

**位置**：
- 项目级：`.claude/settings.json`
- 全局：`~/.claude.json`

### 2.3 可用事件类型

| 事件 | 触发时机 | 适用场景 |
|------|----------|----------|
| `session_start` | 会话开始时 | 加载环境、检查依赖 |
| `user_prompt_submit` | 用户提交 prompt 后 | 预处理、日志记录 |
| `tool_use` | 任何工具调用后 | 通用后置处理 |
| `Edit` | 文件编辑后 | 自动 lint、格式化 |
| `Bash` | 命令执行后 | 验证、清理 |
| `Write` | 文件写入后 | 权限设置、备份 |

### 2.4 标准配置模板

```json
{
  "Edit": [
    {
      "command": "npm run lint -- --fix $FILE",
      "description": "Auto-fix lint issues for edited file",
      "onError": "continue"
    },
    {
      "command": "npm run test -- --related $FILE",
      "description": "Run tests related to edited file",
      "onError": "stop"
    }
  ],
  "Write": {
    "command": "chmod +x $FILE && echo 'Made executable'",
    "description": "Make new files executable",
    "onError": "ignore"
  },
  "session_start": {
    "command": "git status && echo '---' && git log --oneline -3",
    "description": "Show git status on session start",
    "onError": "continue"
  }
}
```

### 2.5 可用变量

| 变量 | 说明 | 示例 |
|------|------|------|
| `$FILE` | 当前编辑的文件路径 | `src/user.service.ts` |
| `$DIR` | 当前工作目录 | `/Users/xxx/project` |
| `$ARGUMENTS` | 用户输入的参数 | `fix-issue 123` |

### 2.6 实战案例

#### 案例 1：自动修复代码风格

```json
{
  "Edit": {
    "command": "npm run lint -- --fix $FILE",
    "description": "Auto-fix lint issues"
  }
}
```

#### 案例 2：质量控制链

```json
{
  "Edit": [
    {
      "command": "npm run lint -- --fix $FILE",
      "description": "Fix lint issues"
    },
    {
      "command": "npm run format -- $FILE",
      "description": "Format file"
    },
    {
      "command": "npm run test -- --passWithNoTests --related $FILE",
      "description": "Run related tests"
    }
  ]
}
```

#### 案例 3：会话启动检查

```json
{
  "session_start": {
    "command": "echo '=== Git Status ===' && git status && echo '\\n=== Recent Commits ===' && git log --oneline -5",
    "description": "Show git status and recent commits"
  }
}
```

#### 案例 4：环境检查

```json
{
  "session_start": [
    {
      "command": "command -v node && node --version || echo 'Node.js not installed'",
      "description": "Check Node.js installation"
    },
    {
      "command": "test -f .env && echo '.env exists' || echo 'Warning: .env not found'",
      "description": "Check .env file"
    }
  ]
}
```

### 2.7 错误处理

```json
{
  "Edit": {
    "command": "npm run lint -- --fix $FILE",
    "description": "Auto-fix lint issues",
    "onError": "continue"  // continue | stop | ignore
  }
}
```

| onError 值 | 行为 |
|------------|------|
| `continue` | 记录错误但继续执行后续 hooks |
| `stop` | 停止执行后续 hooks |
| `ignore` | 静默忽略错误 |

### 2.8 注意事项

**⚠️ 谨慎使用**：
- 数据修改类操作（删除、覆盖）建议保持手动确认
- 耗时命令（>5s）会拖慢交互体验
- 有副作用的命令（推送代码、发送邮件）需格外小心

**✅ 推荐**：
-  lint、format 等无风险操作可放心启用
- 测试命令设置 `--passWithNoTests` 避免中断
- 使用 `onError: continue` 保证流程不中断

---

## 3. SubAgents - 任务分解

### 3.1 是什么

SubAgents 是专门的 AI 子代理，每个负责一个子任务。它们有独立的上下文，可以并行工作，最后合并结果。

**解决痛点**：单个 Agent 处理复杂任务时容易遗漏边界情况、在不同部分之间不一致、中途跑偏。

### 3.2 适用场景

| 场景 | 是否需要 SubAgents |
|------|-------------------|
| 简单问答、单文件修改 | ❌ 不需要 |
| 添加一个新 API 端点 | ❌ 不需要 |
| 重构认证系统（多模块） | ✅ 推荐 |
| 竞品分析报告（多数据源） | ✅ 推荐 |
| 全栈功能开发（前后端 + 测试） | ✅ 推荐 |

### 3.3 任务分解模式

#### 模式 1：串行分解

```
主 Agent：规划整体架构
    ↓
SubAgent 1：实现模块 A
    ↓
SubAgent 2：实现模块 B（依赖 A）
    ↓
SubAgent 3：集成测试
    ↓
主 Agent：汇总验证
```

#### 模式 2：并行分解

```
主 Agent：规划任务列表
    ├→ SubAgent 1：处理模块 A（并行）
    ├→ SubAgent 2：处理模块 B（并行）
    ├→ SubAgent 3：处理模块 C（并行）
    └→ 主 Agent：汇总结果
```

#### 模式 3：混合模式

```
主 Agent：分析需求 → 生成任务图
    ↓
第一阶段（并行）：
    ├→ SubAgent 1：读取 GitHub 代码
    ├→ SubAgent 2：读取文档网站
    └→ SubAgent 3：读取社交媒体
    ↓
第二阶段（串行）：
    └→ 主 Agent：汇总分析报告
```

### 3.4 实战案例：重构认证系统

**任务**：重构一个认证系统，包括登录、Token 管理、权限验证。

```
【主 Agent】
职责：规划整体架构、协调子任务、最终整合

【SubAgent 1 - 登录逻辑】
职责：
- 分析现有登录流程
- 实现新的登录接口
- 添加登录测试

【SubAgent 2 - Token 管理】
职责：
- 设计 Token 结构
- 实现 Token 生成/验证/刷新
- 添加 Token 测试

【SubAgent 3 - 权限验证】
职责：
- 定义权限模型
- 实现权限中间件
- 添加权限测试

【SubAgent 4 - 单元测试】
职责：
- 更新所有受影响的单元测试
- 确保覆盖率达标
```

### 3.5 Token 效率

根据实战数据，SubAgents 可显著减少 token 消耗：

| 任务类型 | 单一 Agent | SubAgents | 节省 |
|----------|-----------|-----------|------|
| 小功能（单文件） | ~5K | ~5K | 无差异 |
| 中功能（多文件） | ~50K | ~30K | 40% |
| 大功能（多模块） | ~200K | ~80K | 60% |

**原理**：每个 SubAgent 只关注自己的子任务，不需要携带整个项目的上下文。

### 3.6 使用建议

**何时使用**：
- 任务可明确分解为独立子任务
- 需要访问多个外部数据源
- 任务执行时间预计 >30 分钟
- 需要保持多个文件/模块的一致性

**何时无需使用**：
- 简单问答、单文件修改
- 任务本身难以分解
- 子任务之间有强依赖

---

## 4. Commands - 快捷命令

### 4.1 是什么

Commands 是可自定义的斜杠命令，让你用简短的命令快速触发复杂的 prompt。

**解决痛点**：重复输入相同的指令很麻烦，容易遗漏要点。

### 4.2 文件位置

```
.claude/commands/
├── review.md              # /review
├── fix-issue.md           # /fix-issue
├── deploy.md              # /deploy
└── docs/
    └── api.md             # /docs api
```

### 4.3 基础语法

**最简单**：`commands/review.md`
```markdown
请对当前分支的改动进行代码审查，重点检查：

1. 代码质量和可维护性
2. 潜在的安全漏洞
3. 性能问题
4. 测试覆盖率
5. 文档完整性

请使用 GitHub CLI 查看 PR 详情。
```

**使用**：`/review`

### 4.4 带参数的命令

`commands/fix-issue.md`
```markdown
请分析并修复 GitHub issue: $ARGUMENTS

步骤：
1. 使用 `gh issue view $ARGUMENTS` 获取 issue 详情
2. 搜索代码库找到相关文件
3. 实现必要的修改
4. 编写并运行测试验证修复
5. 创建提交并推送 PR

如果 $ARGUMENTS 为空，请提示用户提供 issue 编号。
```

**使用**：
- `/fix-issue 123` - 修复 issue #123
- `/fix-issue` - 提示输入 issue 编号

### 4.5 高级示例

#### 示例 1：代码审查命令

`commands/review.md`
```markdown
请对当前分支进行全面的代码审查。

## 审查范围
- 当前分支与 main 的差异
- 新增/修改的文件
- 相关的测试文件

## 审查维度

### 1. 代码质量
- [ ] 是否有重复代码？
- [ ] 函数是否足够短小（<30 行）？
- [ ] 命名是否清晰？

### 2. 安全性
- [ ] 是否有 SQL 注入风险？
- [ ] 是否有 XSS 风险？
- [ ] 敏感信息是否硬编码？

### 3. 性能
- [ ] 是否有 N+1 查询问题？
- [ ] 是否有不必要的循环？
- [ ] 是否有内存泄漏风险？

### 4. 测试
- [ ] 新增代码是否有测试？
- [ ] 测试覆盖率是否达标？
- [ ] 是否有边界情况测试？

### 5. 文档
- [ ] 公共 API 是否有注释？
- [ ] 复杂逻辑是否有说明？
- [ ] 更新 README 了吗？

## 输出格式

请用以下格式输出审查结果：

### 🟢 做得好的
- [列表]

### 🟡 需要改进
- [列表 + 具体建议]

### 🔴 必须修复
- [列表 + 阻塞原因]
```

#### 示例 2：功能开发命令

`commands/feature.md`
```markdown
请帮我实现一个功能：$ARGUMENTS

## 实现流程

### 1. 需求分析
- 理解功能需求
- 识别边界情况
- 确认数据结构

### 2. 设计
- API 设计（如适用）
- 数据模型设计
- 接口设计

### 3. 实现
- 创建必要文件
- 编写核心逻辑
- 添加错误处理

### 4. 测试
- 编写单元测试
- 编写集成测试（如适用）
- 验证边界情况

### 5. 文档
- 更新 API 文档
- 添加使用示例

## 代码规范

- 遵循项目现有的代码风格
- 所有公共方法必须有类型注解
- 复杂逻辑必须有注释
- 必须有测试覆盖

## 验证

完成后请运行：
```bash
npm run lint && npm run test
```
```

#### 示例 3：部署命令

`commands/deploy.md`
```markdown
请执行部署流程。

## 前置检查

- [ ] 本地测试通过
- [ ] Lint 检查通过
- [ ] 版本号已更新
- [ ] CHANGELOG 已更新

## 部署步骤

### 1. 构建
```bash
npm run build
```

### 2. 运行测试
```bash
npm run test:coverage
```

### 3. 创建 Git Tag
```bash
git tag v[版本号]
git push origin v[版本号]
```

### 4. 触发 CI/CD
[根据项目实际情况填写]

### 5. 验证部署
- [ ] 访问生产环境
- [ ] 检查核心功能
- [ ] 查看日志无错误

## 回滚方案

如果部署失败：
```bash
git revert HEAD
git push origin main
```
```

### 4.6 团队共享

将 `.claude/commands/` 目录提交到 Git，团队成员共享：

```bash
git add .claude/commands/
git commit -m "chore: 添加团队 Commands"
```

新成员 clone 后即可使用 `/review`、`/deploy` 等命令。

---

## 5. Skills - 可复用技能

### 5.1 是什么

Skills 是你自定义的可复用技能包。把一个完整工作流封装起来，下次直接调用。

**解决痛点**：有些复杂操作，每次都要一步步教 Claude，很累。

### 5.2 与 Commands 的区别

| 特性 | Commands | Skills |
|------|----------|--------|
| 触发方式 | 手动输入 `/cmd` | AI 自动识别 |
| 参数 | 支持 $ARGUMENTS | 支持多参数 |
| 复杂度 | 简单 prompt | 完整工作流 |
| 调用链 | 单一执行 | 可链式调用 |
| 类比 | 快捷键 | 操作手册 |

### 5.3 文件结构

```
.claude/skills/
├── add-api-endpoint/
│   └── SKILL.md
├── deploy/
│   └── SKILL.md
└── code-review/
    └── SKILL.md
```

### 5.4 SKILL.md 标准格式

```markdown
---
name: skill-name
description: 一句话描述技能功能
---

# Skill 名称

## 触发条件

当用户请求 [某种类型的任务 ] 时，自动触发此技能。

## 工作流程

### 步骤 1：需求确认
- 询问用户 [必要信息]
- 确认 [边界条件]

### 步骤 2：[具体操作]
- [操作细节]

### 步骤 3：验证
- [验证方法]

## 输出要求

- 输出格式：[代码/文档/报告]
- 代码风格：[遵循项目规范]
- 测试要求：[必须有测试]

## 注意事项

- [注意点 1]
- [注意点 2]
```

### 5.5 实战案例

#### 案例 1：添加 API 端点

`.claude/skills/add-api-endpoint/SKILL.md`
```markdown
---
name: add-api-endpoint
description: 添加新的 API 端点，包括路由、控制器、验证、测试和文档更新
---

# Add API Endpoint

添加一个新的 API 端点的完整流程。

## 触发条件

当用户请求添加 API 端点、创建新接口、实现 xxx 功能时自动触发。

## 工作流程

### 步骤 1：需求确认

询问用户以下信息：
- 端点名称/功能描述
- HTTP 方法（GET/POST/PUT/DELETE）
- 路由路径（如 `/api/users/:id`）
- 请求参数（如有）
- 返回数据结构

### 步骤 2：创建文件

按项目结构创建：
1. 路由文件：`src/routes/xxx.routes.ts`
2. 控制器：`src/controllers/xxx.controller.ts`
3. 服务层：`src/services/xxx.service.ts`
4. DTO/验证：`src/dto/xxx.dto.ts`
5. 测试文件：`src/controllers/xxx.controller.spec.ts`

### 步骤 3：实现逻辑

- 控制器：处理请求/响应
- 服务层：业务逻辑
- DTO：请求验证
- 路由：注册端点

### 步骤 4：编写测试

- 单元测试：测试控制器方法
- 集成测试：测试完整流程
- 边界情况：空值、错误处理

### 步骤 5：更新文档

- 更新 API 文档（README 或 OpenAPI）
- 添加使用示例

## 代码规范

- 遵循 NestJS 最佳实践
- 使用 Dependency Injection
- 所有方法必须有返回类型
- 使用 async/await

## 验证

完成后运行：
```bash
npm run lint && npm run test -- xxx.controller.spec.ts
```

## 注意事项

- 路由命名遵循 RESTful 规范
- 错误处理统一使用 HttpException
- 敏感数据需要验证和过滤
```

#### 案例 2：代码审查技能

`.claude/skills/code-review/SKILL.md`
```markdown
---
name: code-review
description: 全面的代码审查，覆盖质量、安全、性能、测试、文档
---

# Code Review

全面的代码审查技能。

## 触发条件

当用户请求审查代码、检查代码质量、PR 审查时自动触发。

## 审查维度

### 1. 代码质量
- 代码重复
- 函数长度（>30 行需重构）
- 命名清晰度
- 代码可读性

### 2. 安全性
- SQL 注入
- XSS 攻击
- CSRF 保护
- 敏感信息泄露
- 认证授权问题

### 3. 性能
- N+1 查询
- 不必要的循环
- 内存泄漏风险
- 缓存使用

### 4. 测试
- 测试覆盖率
- 边界情况
- Mock 使用
- 测试可维护性

### 5. 文档
- 公共 API 注释
- 复杂逻辑说明
- README 更新

## 输出格式

### 🟢 做得好的
[正面反馈]

### 🟡 需要改进
[改进建议 + 代码示例]

### 🔴 必须修复
[阻塞问题 + 修复方案]

## 注意事项

- 语气要建设性，不是批评
- 提供具体可执行的建议
- 优先级排序（阻塞 > 重要 > 建议）
```

### 5.6 链式调用

Skills 之间可以互相调用：

```markdown
---
name: deploy
description: 完整部署流程
---

# Deploy

## 工作流程

1. 先调用 `run-tests` Skill 确保测试通过
2. 调用 `build` Skill 构建项目
3. 调用 `bump-version` Skill 更新版本号
4. 执行部署
5. 调用 `create-release` Skill 创建 Release
```

---

## 6. MCP Servers - 外部连接

### 6.1 是什么

MCP（Model Context Protocol）是 Anthropic 推出的开放标准，让 AI 工具能连接外部数据源。

**解决痛点**：Claude Code 只能访问本地文件和命令，无法读取外部资源。

### 6.2 支持的服务器

| 类型 | 示例 | 用途 |
|------|------|------|
| 设计 | Figma | 读取设计稿自动实现 UI |
| 代码 | GitHub | 读取仓库、PR、Issue |
| 文档 | Google Drive | 读取需求文档、规范 |
| 沟通 | Slack | 读取团队讨论历史 |
| 数据库 | PostgreSQL | 直接查询数据库 |
| API | 自定义 REST API | 调用内部服务 |

### 6.3 配置方法

**位置**：`~/.claude/mcp.json` 或项目级 `.claude/mcp.json`

```json
{
  "mcpServers": {
    "figma": {
      "command": "npx",
      "args": ["-y", "@figma/mcp-server"],
      "env": {
        "FIGMA_API_KEY": "your_api_key"
      }
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@github/mcp-server"],
      "env": {
        "GITHUB_TOKEN": "your_token"
      }
    }
  }
}
```

### 6.4 实战案例

#### 案例 1：从 Figma 读取设计稿

**配置**：
```json
{
  "mcpServers": {
    "figma": {
      "command": "npx",
      "args": ["-y", "@figma/mcp-server"],
      "env": {
        "FIGMA_API_KEY": "your_key"
      }
    }
  }
}
```

**使用**：
```
请读取这个 Figma 文件的设计稿，然后用 React 实现它：
https://www.figma.com/file/xxx/design-system

设计要求：
- 使用 Tailwind CSS
- 组件要可复用
- 支持响应式
```

**Claude 会**：
1. 通过 MCP 连接 Figma
2. 读取设计稿的图层、样式、布局
3. 生成对应的 React 代码
4. 保持设计规范一致

#### 案例 2：从 GitHub 读取 Issue

**配置**：
```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@github/mcp-server"],
      "env": {
        "GITHUB_TOKEN": "your_token"
      }
    }
  }
}
```

**使用**：
```
请读取 my-repo 的 issue #123，然后实现描述的功能。
```

#### 案例 3：多数据源组合

**场景**：全栈开发流程

```
【数据源】
├── Google Drive：读取产品需求文档
├── Figma：读取 UI 设计稿
├── GitHub：读取现有代码和 API 文档
└── Slack：读取相关讨论历史

【Claude 的工作】
1. 从 Drive 理解需求
2. 从 Figma 获取设计
3. 从 GitHub 了解现有架构
4. 从 Slack 理解背景讨论
5. 生成完整实现方案
```

### 6.5 可用 MCP Servers

官方和社区提供的 MCP Servers：

- `@figma/mcp-server` - Figma 设计稿
- `@github/mcp-server` - GitHub 仓库
- `@google-drive/mcp-server` - Google Drive
- `@slack/mcp-server` - Slack
- `@postgres/mcp-server` - PostgreSQL 数据库
- `@filesystem/mcp-server` - 文件系统操作

查看完整列表：https://github.com/modelcontextprotocol

### 6.6 安全注意事项

**⚠️ 重要**：
- 只安装可信来源的 MCP Servers
- API 密钥妥善保管，不要提交到 Git
- 定期轮换密钥
- 限制 MCP 的访问权限（只读 vs 读写）

**✅ 推荐**：
- 使用环境变量存储密钥
- 在项目级 `.env` 文件中定义
- 将 `.env` 添加到 `.gitignore`

---

## 7. Plugins - 打包工作流

### 7.1 是什么

Plugins 是打包好的功能组合，可以包含 Commands、SubAgents、Skills、Hooks、MCP 配置等任何组件。

**解决痛点**：每个项目都要重新配置 Commands、Skills、Hooks，复制粘贴容易遗漏。

### 7.2 与 Commands/Skills 的区别

| 特性 | Commands | Skills | Plugins |
|------|----------|--------|---------|
| 内容 | 单个 prompt | 工作流模板 | 完整套件 |
| 包含 | 仅 prompt | prompt + 流程 | Commands+Skills+Hooks+MCP |
| 安装 | 无需 | 无需 | 一键安装 |
| 类比 | 快捷键 | 操作手册 | 工具箱 |

### 7.3 安装官方插件

```bash
# 添加插件市场
/plugin marketplace add anthropics/claude-code-plugins

# 安装插件
/plugin install pr-review-toolkit
/plugin install code-review
/plugin install deploy-toolkit
```

### 7.4 官方插件示例

#### PR Review Toolkit

**包含内容**：
- 6 个专业审查代理：
  - `comment-analyzer` - 注释分析
  - `pr-test-analyzer` - 测试分析
  - `silent-failure-hunter` - 静默失败检测
  - `type-design-analyzer` - 类型设计分析
  - `code-reviewer` - 代码审查
  - `code-simplifier` - 代码简化
- 审查命令：`/pr-review-toolkit:review-pr`
- 支持并行执行

**使用**：
```bash
# 全面审查
/pr-review-toolkit:review-pr all

# 只审查测试
/pr-review-toolkit:review-pr tests

# 只审查类型设计
/pr-review-toolkit:review-pr types
```

#### Code Review

**包含内容**：
- 代码质量检查命令
- 安全检查命令
- 性能检查命令
- 测试覆盖检查命令

**使用**：
```bash
/review quality    # 代码质量
/review security   # 安全检查
/review performance # 性能检查
/review tests      # 测试覆盖
```

### 7.5 创建团队私有插件

#### 步骤 1：创建插件目录

```
my-team-plugins/
├── marketplace.json
└── plugins/
    ├── coding-standards/
    │   ├── commands/
    │   ├── skills/
    │   └── hooks.json
    └── pr-workflow/
        ├── commands/
        ├── agents/
        └── mcp.json
```

#### 步骤 2：定义 marketplace.json

```json
{
  "name": "my-team-marketplace",
  "description": "Our team's coding standards and workflows",
  "plugins": [
    {
      "name": "team-coding-standards",
      "path": "plugins/coding-standards",
      "description": "团队代码规范和工作流"
    },
    {
      "name": "team-pr-workflow",
      "path": "plugins/pr-workflow",
      "description": "团队 PR 审查流程"
    }
  ],
  "repository": {
    "type": "git",
    "url": "https://github.com/my-org/my-team-plugins"
  }
}
```

#### 步骤 3：团队安装

```bash
# 添加团队市场
/plugin marketplace add my-org/my-team-plugins

# 安装插件
/plugin install team-coding-standards
/plugin install team-pr-workflow
```

### 7.6 插件内容示例

**plugin/coding-standards/commands/review.md**
```markdown
请审查代码，遵循团队规范：

1. 代码质量（函数长度<30 行，嵌套<3 层）
2. 类型安全（必须有类型注解）
3. 测试覆盖（新增代码必须有测试）
4. 文档完整（公共 API 必须有注释）

团队规范文档：docs/coding-standards.md
```

**plugin/coding-standards/skills/team-api/SKILL.md**
```markdown
---
name: team-api
description: 按团队规范添加 API 端点
---

遵循团队 API 开发规范：
1. 必须使用 DTO 进行参数验证
2. 必须有单元测试和集成测试
3. 必须更新 OpenAPI 文档
4. 必须添加错误日志
```

**plugin/coding-standards/hooks.json**
```json
{
  "Edit": [
    {
      "command": "npm run lint -- --fix $FILE",
      "description": "Auto-fix lint"
    },
    {
      "command": "npm run test -- --passWithNoTests --related $FILE",
      "description": "Run related tests"
    }
  ]
}
```

---

## 8. 组件组合实战

### 8.1 CLAUDE.md + Hooks

**场景**：新成员快速上手

```
【配置】
1. CLAUDE.md 提供项目上下文
   - 技术栈、命令、规范

2. Hooks 自动执行质量检查
   - 编辑后自动 lint
   - 提交前自动测试

【效果】
- 新成员无需询问项目规范
- Claude 根据 CLAUDE.md 给出准确建议
- Hooks 确保每次修改符合规范
- 双重保障，零配置上手
```

### 8.2 SubAgents + MCP

**场景**：竞品功能分析

```
【任务】分析竞品 X 的功能

【分解】
主 Agent：规划分析框架
    ├→ SubAgent 1：从 GitHub 读取竞品代码（GitHub MCP）
    ├→ SubAgent 2：从文档网站读取竞品文档（Web Fetch MCP）
    ├→ SubAgent 3：从社交媒体读取用户反馈（Social Media MCP）
    └→ 主 Agent：汇总分析报告

【输出】
- 功能对比表
- 技术栈分析
- 用户评价汇总
- 改进建议
```

### 8.3 Commands + Skills

**场景**：功能开发工作流

```
【流程】
1. /feature-dev "添加用户收藏功能"
   → 触发 Commands
   → 自动加载 add-feature Skill

2. Skill 执行：
   - 需求确认
   - 创建文件
   - 实现逻辑
   - 编写测试
   - 更新文档

3. /review
   → 触发代码审查 Commands
   → 按照 review Skill 执行审查

4. /commit
   → 生成提交信息
   → 推送代码

【效果】
- Commands 提供快捷入口
- Skills 确保执行标准化
- 一个命令完成复杂任务
```

### 8.4 Plugins 一键配置

**场景**：新项目启动

```
【传统方式】
1. 手动配置 CLAUDE.md（1 小时）
2. 创建 Commands（2 小时）
3. 编写 Skills（3 小时）
4. 配置 Hooks（1 小时）
5. 安装 MCP Servers（1 小时）
总计：8 小时

【Plugins 方式】
/plugin install team-coding-standards

自动配置：
- 10 个 Commands（代码审查、提交、部署等）
- 5 个 Skills（代码规范、测试要求、文档标准等）
- 3 个 Hooks（代码格式化、测试运行、安全扫描）
- 2 个 MCP 服务器（Jira、Figma）
- 项目级 CLAUDE.md 模板
总计：1 分钟

【效果】
- 一次配置，全员共享
- 新成员加入，三个命令同步工作流
- 团队标准化
```

---

## 9. 避坑清单

### 9.1 CLAUDE.md

| 问题 | 建议 |
|------|------|
| 写成长篇文档 | 保持简洁，只写关键信息 |
| 命令不可执行 | 定期验证命令有效性 |
| 规范与实际不符 | 代码变更后同步更新 |
| 从不更新 | 像优化 prompt 一样持续迭代 |

### 9.2 Hooks

| 问题 | 建议 |
|------|------|
| 一次性启用太多 | 从简单的 lint 开始逐步添加 |
| 耗时命令拖慢体验 | 避免 >5s 的命令 |
| 数据修改自动执行 | 删除、覆盖等操作保持手动确认 |
| 错误处理不当 | 使用 onError 控制行为 |

### 9.3 SubAgents

| 问题 | 建议 |
|------|------|
| 简单任务也用 | 简单任务用主 Agent 就够了 |
| 任务分解过细 | 分解到 1-2 小时可完成的粒度 |
| 子任务强依赖 | 确保子任务可独立执行 |

### 9.4 Commands

| 问题 | 建议 |
|------|------|
| prompt 太模糊 | 具体说明审查维度/检查项 |
| 不测试就使用 | 先手动执行验证效果 |
| 团队不共享 | 提交到 Git，全员共享 |

### 9.5 Skills

| 问题 | 建议 |
|------|------|
| 流程过于复杂 | 保持简单，3-5 步最佳 |
| 触发条件不明确 | 清晰定义何时触发 |
| 不验证就依赖 | 测试多次确保稳定 |

### 9.6 MCP Servers

| 问题 | 建议 |
|------|------|
| 安装不可信来源 | 只用官方和知名开发者的 |
| 密钥硬编码 | 使用环境变量存储 |
| 权限过大 | 限制为只读（如可能） |

### 9.7 Plugins

| 问题 | 建议 |
|------|------|
| 不审查就安装 | 查看 plugin 包含的内容 |
| 安装太多 | 只安装真正需要的 |
| 不更新 | 定期检查更新 |

---

## 10. 快速查阅表

### 10.1 文件位置

| 组件 | 文件位置 |
|------|----------|
| CLAUDE.md | `项目根目录/CLAUDE.md` 或 `~/.claude/CLAUDE.md` |
| Hooks | `.claude/settings.json` 或 `~/.claude.json` |
| Commands | `.claude/commands/*.md` |
| Skills | `.claude/skills/*/SKILL.md` |
| MCP | `.claude/mcp.json` 或 `~/.claude/mcp.json` |
| Plugins | `~/.claude/plugins/` |

### 10.2 命令速查

```bash
# 插件管理
/plugin marketplace add <marketplace>
/plugin install <plugin-name>
/plugin list

# 会话管理
/quit          # 结束当前会话
/help          # 显示帮助

# 内置命令
/review        # 代码审查（如已配置）
/deploy        # 部署（如已配置）
/test          # 运行测试（如已配置）
```

### 10.3 Hook 事件

| 事件 | 触发时机 |
|------|----------|
| `session_start` | 会话开始时 |
| `user_prompt_submit` | 用户提交 prompt 后 |
| `tool_use` | 任何工具调用后 |
| `Edit` | 文件编辑后 |
| `Write` | 文件写入后 |
| `Bash` | 命令执行后 |

### 10.4 变量

| 变量 | 说明 |
|------|------|
| `$FILE` | 当前编辑的文件路径 |
| `$DIR` | 当前工作目录 |
| `$ARGUMENTS` | 用户输入的参数 |

### 10.5 决策树

```
需求类型 → 推荐组件
─────────────────────────
项目配置 → CLAUDE.md
自动化 → Hooks
复杂任务 → SubAgents
快捷入口 → Commands
工作流封装 → Skills
外部连接 → MCP Servers
完整套件 → Plugins
```

### 10.6 学习路径

```
入门（第 1 周）
├── 创建 CLAUDE.md
└── 配置 1-2 个简单 Hooks

进阶（第 2 周）
├── 创建 Commands（/review）
└── 尝试 SubAgents

熟练（第 3 周）
├── 编写 Skills
└── 安装 MCP Servers

精通（第 4 周）
├── 创建 Plugins
└── 组件组合使用
```

---

## 附录

### A. 官方文档

- [Claude Code overview](https://code.claude.com/docs/en/overview)
- [Hooks guide](https://code.claude.com/docs/en/hooks-guide)
- [SubAgents](https://code.claude.com/docs/en/sub-agents)
- [Skills](https://code.claude.com/docs/en/skills)
- [Plugins](https://code.claude.com/docs/en/plugins)
- [MCP](https://www.anthropic.com/news/model-context-protocol)

### B. 最佳实践

- [Claude Code: Best practices for agentic coding](https://www.anthropic.com/engineering/claude-code-best-practices)

### C. 社区资源

- [Marketplace](https://claudecodemarketplace.com/)
- [MCP Servers](https://github.com/modelcontextprotocol)

---

*本文档基于 Anthropic 官方文档和实战经验整理 · 2026 版*
