# memory/ - 记忆系统

> L0/L1/L2 分层记忆 + P0/P1/P2 生命周期管理

## 定位

- **作用**：解决 AI 记忆痛点（跨会话失忆、信息过载、Token 浪费）
- **更新**：自动（observer/UPDATE_MEMORY 触发）
- **来源**：OpenViking 记忆管理方案 + Vibe Coding 分层文档架构

## 解决的问题

- 跨会话失忆 → `.abstract` 索引持久化
- 信息过载 → 分层加载 (L0→L1→L2)
- Token 浪费 → 按需加载，启动只读 100 tokens

## 结构

```
memory/
├── .abstract              # L0 索引 - 启动必读 (~100 tokens)
├── P0-core/               # 永久记忆 - 永不删除
├── P1-active/             # 活跃项目 - 90 天归档
├── P2-daily/              # 日常记录 - 30 天清理
├── P2-observations/       # 洞察记录 - 60 天清理
└── P2-weekly/             # 周文档 - 90 天归档
```

## 层级说明

### L0 - 索引层
- **文件**: `.abstract`
- **Token**: ~100
- **加载时机**: 每次启动
- **内容**: P0/P1/P2 文件清单

### L1 - 摘要层
- **文件**: P1-active/*.md
- **Token**: ~500
- **加载时机**: 用户问到具体项目
- **内容**: 项目摘要 + 状态

### L2 - 详情层
- **文件**: P2-*/daily/*.md
- **Token**: ~5000+
- **加载时机**: 确认需要详情
- **内容**: 完整对话记录/洞察

## 生命周期 (P0/P1/P2)

| 级别 | 保留时间 | 清理规则 | 示例 |
|------|----------|----------|------|
| P0 | 永久 | 永不删除 | 用户偏好、核心决策 |
| P1 | 90 天 | 到期归档提示 | 当前项目任务 |
| P2 | 30 天 | 到期自动清理 | 日常对话记录 |

## 相关命令

| 命令 | 作用 | 触发脚本 |
|------|------|----------|
| `/wake` | 会话启动 | 读 `.abstract` (L0) |
| `/observer` | 写入记忆 | `memory-update-index.sh` |
| `/UPDATE_MEMORY` | 每周清理 | `memory-cleanup.sh` |

## 自动化流程

```
/observer → 写入 P2-daily/P2-observations → memory-update-index.sh → 更新 .abstract
                                                                    ↓
/UPDATE_MEMORY → 周文档 → memory-cleanup.sh → 删除过期 + 重建索引
```

## 维护规则

1. **改了记忆文件必须更新 `.abstract`** - 自动执行
2. **30 天未更新的 P2 自动删除** - cleanup 脚本
3. **90 天未更新的 P1 提示归档** - cleanup 脚本

---

*记忆是 AI 的长期工作区，边跑边调*