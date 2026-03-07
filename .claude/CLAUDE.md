# .claude/ - AI 系统配置

> mino 的"操作系统内核" - AI 行为规则与能力扩展

## 结构

```
.claude/
├── rules/           # 规则库（P0 核心配置）
├── commands/        # Slash 命令
└── skills/          # MCP Skills（扩展能力）
```

## 子模块

### rules/ - 核心规则库

**文件清单**：
| 文件 | 地位 | 功能 |
|------|------|------|
| `01-IDENTITY.md` | P0 | AI 身份定义（nimi） |
| `02-SOUL.md` | P0 | 9 条气质/工作原则 |
| `03-USER.md` | P0 | 年老师画像 |
| `04-MEMORY.md` | P0 | 长期记忆规则 |
| `06-NOW.md` | P0 | 会话启动流程 |
| `WORK.md` | P1 | 工作手册（含五文件工作流） |

**维护规则**：新增规则文件需更新此表格

### commands/ - Slash 命令

**文件清单**：
| 文件 | 命令 | 作用 |
|------|------|------|
| `wake.md` | `/wake` | 会话启动 |
| `observer.md` | `/observer` | 手动洞察分析 |
| `memory-system.md` | `/memory-system` | 每周记忆整理 |
| `planning-with-files.md` | `/planning-with-files` | 五文件工作流 |

**维护规则**：新增命令需更新此表格

### skills/ - MCP Skills

**核心技能**：
| 技能 | 功能 |
|------|------|
| `memory-system/` | 记忆系统同步 |
| `planning-with-files/` | 五文件工作流 |
| `brainstorming/` | 创意发散 |
| `download-anything/` | 全网资源下载 |
| `pdf/` `docx` `xlsx` `pptx/` | 文档处理 |
| `mcp-builder/` | MCP 工具创建 |

**完整列表**：`ls .claude/skills/`

## 加载顺序

```
/wake → 06-NOW.md → memory/.abstract → 其他按需加载
```

## 维护规则

1. **新增规则文件** → 更新 rules/表格
2. **新增命令** → 更新 commands/表格
3. **技能变更** → 更新 skills/列表
4. **文件更新需同步更新此 README**

---

*规则是 AI 的操作系统，边跑边调*
