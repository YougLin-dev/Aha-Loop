# Aha Loop

[English](README.md) | **[中文](README_ZH.md)**

> 让 AI 从一段话开始，自主完成一个完整项目

**Aha Loop** 是一个全自主 AI 开发系统，基于 [Ralph](https://github.com/snarktank/ralph) 的核心思想扩展而来。它不仅仅是一个执行引擎，而是一个具备 **规划能力**、**研究能力** 和 **监督机制** 的完整 AI 开发框架。

```
一段话描述 → 完整可运行的项目
```

> 想了解 Aha Loop 背后的故事？阅读 [设计故事](docs/design-story_zh.md)

## Aha Loop 在 Ralph 基础上做了什么？

| 原版 Ralph | Aha Loop 扩展 |
|------------|---------------|
| 从 PRD 开始 | 增加了 Vision → Architecture → Roadmap 三个前置阶段 |
| 直接执行 Story | 增加了五阶段工作流：研究 → 并行探索 → 计划审查 → 实现 → 质量审查 |
| 单一执行路径 | 增加了自主并行探索（AI 判断何时探索、创建工作区、评估结果） |
| 无监督 | 增加了 God Committee 独立监督层 |
| 结果导向 | 增加了完整的可观测性日志系统 |

如果说原版 Ralph 是一个**执行引擎**，那 Aha Loop 做的事情是给它加上了**大脑**（规划层）和**眼睛**（监督层）。

## 系统架构

```
用户: "我想做一个 AI 网关..."
                │
                ▼
        ┌───────────────┐
        │ Vision Builder │  交互式问答，构建愿景    ← Aha Loop
        └───────────────┘
                │
                ▼
        ┌───────────────┐
        │ Vision Skill  │  分析愿景，提取需求       ← Aha Loop
        └───────────────┘
                │
                ▼
        ┌───────────────┐
        │ Architect     │  研究技术，选择架构       ← Aha Loop
        └───────────────┘
                │
                ▼
        ┌───────────────┐
        │ Roadmap       │  规划里程碑和 PRD        ← Aha Loop
        └───────────────┘
                │
                ▼
       ┌────────────────────┐
       │ PRD Execution Loop │
       │  ┌──────────────┐  │
       │  │ Research     │  │  研究技术            ← Aha Loop
       │  │ Plan Review  │  │  审查计划            ← Aha Loop
       │  │ Implement    │  │  编写代码（Ralph 核心）
       │  │ Quality Check│  │  质量审查
       │  └──────────────┘  │
       └────────────────────┘
                │
        God Committee 全程监督                     ← Aha Loop
                │
                ▼
         PROJECT COMPLETE
```

## 核心理念

| 原则 | 含义 |
|------|------|
| **无限资源** | 不限制 AI 的计算、网络、存储 |
| **永不放弃** | 遇到问题自动重试、自动修复 |
| **研究先行** | 不懂就学，学会再做 |
| **多路并行** | 不确定就都试，试完再选 |
| **独立监督** | 上帝组委会全程护航 |
| **完全透明** | 所有决策可追溯 |

## 快速开始

Aha Loop 设计为与 Claude Code 配合使用。每个阶段都实现为一个 **Skill**，可以直接调用。

### Windows 设置

如果你在 Windows 上且符号链接没有生效（skills 目录显示为文本文件），运行：

```powershell
git config core.symlinks true
Remove-Item ".claude\skills" -Force; cmd /c mklink /D ".claude\skills" "..\.agents\skills"
Remove-Item ".codex\skills" -Force; cmd /c mklink /D ".codex\skills" "..\.agents\skills"
```

### 阶段 1-3：规划（在 Claude Code 中手动执行）

在项目目录下运行 Claude Code，然后通过 `/skill名称 你的描述` 调用 skill：

```
# 第一步：构建愿景 - 通过交互式问答构建项目愿景
/vision-builder 我想做一个统一多家 LLM API 的 AI 网关

# 第二步：分析愿景 - 提取结构化需求
/vision 分析我的项目愿景

# 第三步：设计架构 - 研究技术并选择架构
/architect 设计系统架构

# 第四步：创建路线图 - 规划里程碑和 PRD
/roadmap 创建项目路线图并拆分为 PRD
```

每个 skill 会生成对应的文档：
- `project.vision.md` - 项目愿景
- `project.vision-analysis.md` - 结构化需求
- `project.architecture.md` - 技术架构
- `project.roadmap.json` - 里程碑和 PRD 队列

### 阶段 4：执行（自主运行）

规划完成后，运行自主执行循环：

```bash
# 运行 PRD 执行循环
./scripts/aha-loop/aha-loop.sh
```

系统会自动为每个 Story 执行**五阶段工作流**：

1. **研究**：拉取库源码、研究实现方式、生成研究报告
2. **并行探索**：当 AI 识别到重大技术决策（存在多种可行方案）时，它会自动：
   - 为每种方案创建 git worktree
   - 并行运行多个 AI Agent 实现每个方案
   - 评估结果并推荐最佳方案
3. **计划审查**：根据研究和探索结果评估是否需要调整计划
4. **实现**：按照研究建议和探索结果编写代码
5. **质量检查**：验证实现是否满足验收标准

AI 会自主判断每个阶段是否需要执行。所有这些都是自主决策，无需人工干预。

## 目录结构

**Aha Loop 仓库结构：**

```
AhaLoop/
├── .agents/skills/             # AI 技能库（共享）
│   ├── vision-builder/         # 交互式愿景构建
│   ├── vision/                 # 愿景分析
│   ├── architect/              # 架构设计
│   ├── roadmap/                # 路线图规划
│   ├── research/               # 深度研究
│   ├── parallel-explore/       # 并行探索
│   └── ...                     # 其他技能
├── .claude/skills/             # 符号链接 → .agents/skills
├── .codex/skills/              # 符号链接 → .agents/skills
├── .god/                       # 上帝组委会
│   ├── config.json             # 组委会配置
│   ├── council/                # 议事厅
│   ├── members/                # 成员状态 (alpha/beta/gamma)
│   └── powers/                 # 权力记录
├── scripts/aha-loop/           # 执行脚本
│   ├── aha-loop.sh             # PRD 执行器
│   ├── parallel-explorer.sh    # 并行探索
│   ├── config.json             # 执行配置
│   └── templates/              # 文档模板
├── scripts/god/                # 上帝组委会脚本
├── knowledge/                  # 知识库
└── tasks/                      # PRD 文档
```

**使用 Aha Loop 后，你的项目会生成：**

```
your-project/
├── project.vision.md           # 项目愿景（/vision-builder 生成）
├── project.vision-analysis.md  # 愿景分析（/vision 生成）
├── project.architecture.md     # 技术架构（/architect 生成）
├── project.roadmap.json        # 里程碑和 PRD（/roadmap 生成）
├── tasks/                      # PRD 文档（自动生成）
│   └── prd-001-xxx.md
└── logs/                       # AI 思考日志
    └── ai-thoughts.md
```

## 致谢

- [Ralph](https://github.com/snarktank/ralph) - 提供了核心的 PRD 执行循环思想
- [Claude](https://www.anthropic.com/claude) - AI 能力支持

## 许可证

MIT

## ⭐ Star 历史

[![Star History Chart](https://api.star-history.com/svg?repos=YougLin-dev/Aha-Loop&type=Date)](https://star-history.com/#YougLin-dev/Aha-Loop&Date)

---

**相关链接：**

- 示例项目：[AI-Gateway](https://github.com/YougLin-dev/AI-Gateway) - 用早期实验性版本 Aha Loop 生成的 API 网关
- 参考项目：[snarktank/ralph](https://github.com/snarktank/ralph)

*如果觉得 Aha Loop 有意思，欢迎 Star*
