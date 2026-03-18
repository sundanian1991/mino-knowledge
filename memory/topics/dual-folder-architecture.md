---
pos: memory/topics/dual-folder-architecture.md
input: nomi vs Mino 定位讨论
output: 双文件夹差异化定位确立
created: 2026-03-06
updated: 2026-03-06
status: active
---

# 双文件夹架构

> nomi 与 Mino 的差异化定位

---

## 定位方案

| 维度 | **nomi** | **Mino** |
|------|---------|----------|
| **路径** | `~/.myagents/projects/mino` | `~/Documents/projects/ai-agents/my-agent` |
| **角色** | 活力自主的探索者 | 深度工作的伙伴 |
| **记忆** | 自己维护独立体系 | 自己维护独立体系 |
| **风格** | 轻量、敢尝试、快速迭代 | 厚重、稳扎稳打、业务沉淀 |
| **用途** | 新想法试验田、个性化探索 | 主力生产环境、供应商管理业务 |
| **更新频率** | 高（边跑边调） | 中（稳定演进） |

---

## 工作流

```
MyAgents 启动 → nomi (轻量配置加载)
              ↓
         需要深度工作时 → git pull my-agent
              ↓
         在 my-agent 中执行任务、写记忆、commit
```

---

## 关键原则

1. **隔离风险** - nomi 里随便试，试错了不影响 Mino 的业务沉淀
2. **风格互补** - nomi 跳脱，Mino 稳重
3. **互不干扰** - 两边记忆独立，不会冲突或重复
4. **各自成长** - 可以对比两个 AI 的进化路径

---

## 历史

- **2026-03-06**：定位方案确立，写入 2026-W09 周文档
