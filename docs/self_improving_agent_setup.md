# Self-Improving Agent 安装与配置指南

## 前置条件
- 环境：OpenClaw 运行环境（macOS/Linux），Node.js 18+，Skills CLI 可用。
- 权限：允许安装技能包与写入 AGENTS.md。

## 安装步骤
1. 安装 Skills CLI（若未安装）：
   - 命令示例：`npm i -g @openclaw/skills-cli` 或按技能仓库 README 执行安装。
2. 安装 self-improving-agent 技能包：
   - 通过 Skills CLI 添加技能包（示例）：
     - `skills add openclaw/skills/tree/main/skills/dennis-da-menace/agent-memory/SKILL.md`
   - 或按技能仓库提供的安装脚本执行。
3. 验证安装：
   - 运行 `skills list` 确认 self-improving-agent 在列表中。
   - 运行 `skills check` 检查依赖与版本。

## AGENTS.md 配置
- 在 AGENTS.md 的“指令与行为规范”章节添加以下代码段：

```markdown
## 指令模糊处理
“每一次我的指令你觉得模棱两可，你都要指出来所有的可能性，并且让我做选择以后 再开始行动”

## 复盘机制激活
“每一次我说复盘，你都要调用 自我提升 skill 并且把 review 记录到对应的记忆文档里面”
```

- 若存在多版本 AGENTS.md，请同步更新所有副本。

## 触发场景与行为
- 指令模糊处理：
  - 触发：检测到指令存在歧义（范围、优先级、格式、时间、依赖等）。
  - 行为：列出所有合理可能性与影响，请求用户选择或补充约束，并记录选择作为上下文。
- 复盘机制激活：
  - 触发：用户明确说“复盘”，或在任务结束后自动触发（可配置）。
  - 行为：调用 self-improving-agent 的 review 流程，生成复盘报告并写入记忆文档。

## 功能说明
- 指令模糊处理：增强指令理解准确性，避免执行偏差，提高任务成功率。
- 复盘机制激活：实现自动复盘与学习成果固化，形成持续改进闭环。

## 验证清单
- 安装与验证 self-improving-agent：`skills list` 与 `skills check` 通过。
- 更新 AGENTS.md 并重启会话或重新加载配置。
- 测试指令模糊处理：输入含歧义的指令，确认代理会列出可能性并等待选择。
- 测试复盘机制：输入“复盘”，确认生成复盘报告并写入记忆文档。

## 常见问题与排查
- 安装失败：检查网络与权限，确认 Skills CLI 版本与技能仓库可用性。
- 指令未触发模糊处理：确认 AGENTS.md 已加载且关键词匹配；检查日志。
- 复盘未写入记忆：确认 memory/ 目录可写，复盘模块路径配置正确。