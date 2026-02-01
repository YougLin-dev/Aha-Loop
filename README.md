# Aha Loop

**[English](README.md)** | [中文](README_ZH.md)

> From a single paragraph to a complete project — fully autonomous AI development

**Aha Loop** is a fully autonomous AI development system, extended from [Ralph](https://github.com/snarktank/ralph). It's not just an execution engine, but a complete AI development framework with **planning capabilities**, **research abilities**, and **oversight mechanisms**.

```
One paragraph description → Complete runnable project
```

> Want to know the story behind Aha Loop? Read [The Design Story](docs/design-story.md)

## What Aha Loop Adds to Ralph

| Original Ralph | Aha Loop Extensions |
|----------------|---------------------|
| Starts from PRD | Added Vision → Architecture → Roadmap pre-phases |
| Direct Story execution | Added five-phase workflow: Research → Exploration → Plan Review → Implement → Quality Review |
| Single execution path | Added autonomous parallel exploration (AI decides when to explore, creates worktrees, evaluates) |
| No oversight | Added God Committee independent supervision |
| Result-oriented | Added complete observability logging |

If Ralph is an **execution engine**, Aha Loop adds the **brain** (planning layer) and **eyes** (oversight layer).

## System Architecture

```
User: "I want to build an AI gateway..."
                │
                ▼
        ┌───────────────┐
        │ Vision Builder │  Interactive Q&A, build vision    ← Aha Loop
        └───────────────┘
                │
                ▼
        ┌───────────────┐
        │ Vision Skill  │  Analyze vision, extract requirements  ← Aha Loop
        └───────────────┘
                │
                ▼
        ┌───────────────┐
        │ Architect     │  Research tech, select architecture    ← Aha Loop
        └───────────────┘
                │
                ▼
        ┌───────────────┐
        │ Roadmap       │  Plan milestones and PRDs              ← Aha Loop
        └───────────────┘
                │
                ▼
       ┌────────────────────┐
       │ PRD Execution Loop │
       │  ┌──────────────┐  │
       │  │ Research     │  │  Research technology     ← Aha Loop
       │  │ Plan Review  │  │  Review plan             ← Aha Loop
       │  │ Implement    │  │  Write code (Ralph core)
       │  │ Quality Check│  │  Quality review
       │  └──────────────┘  │
       └────────────────────┘
                │
        God Committee monitors throughout              ← Aha Loop
                │
                ▼
         PROJECT COMPLETE
```

## Core Principles

| Principle | Meaning |
|-----------|---------|
| **Unlimited Resources** | No limits on AI's compute, network, storage |
| **Never Give Up** | Auto-retry, auto-repair on issues |
| **Research First** | Learn before doing |
| **Parallel Paths** | Try all options, choose the best |
| **Independent Oversight** | God Committee monitors everything |
| **Full Transparency** | All decisions are traceable |

## Quick Start

Aha Loop is designed to work with Claude Code. Each phase is implemented as a **Skill** that you can invoke directly.

### Windows Setup

If you're on Windows and symlinks aren't working (skills directory shows as text file), run:

```powershell
git config core.symlinks true
Remove-Item ".claude\skills" -Force; cmd /c mklink /D ".claude\skills" "..\.agents\skills"
Remove-Item ".codex\skills" -Force; cmd /c mklink /D ".codex\skills" "..\.agents\skills"
```

### Phase 1-3: Planning (Manual with Claude Code)

Run Claude Code in your project directory, then use skills via `/skill-name your description`:

```
# Step 1: Build Vision - Interactive Q&A to construct project vision
/vision-builder I want to build an AI gateway that unifies multiple LLM APIs

# Step 2: Analyze Vision - Extract structured requirements
/vision Analyze my project vision

# Step 3: Design Architecture - Research tech and select architecture
/architect Design the system architecture

# Step 4: Create Roadmap - Plan milestones and PRDs
/roadmap Create project roadmap and break down into PRDs
```

Each skill generates corresponding documents:
- `project.vision.md` - Your project vision
- `project.vision-analysis.md` - Structured requirements
- `project.architecture.md` - Technical architecture
- `project.roadmap.json` - Milestones and PRD queue

### Phase 4: Execution (Autonomous)

Once planning is complete, run the autonomous execution loop:

```bash
# Run PRD execution loop
./scripts/aha-loop/aha-loop.sh
```

The system will automatically execute a **five-phase workflow** for each Story:

1. **Research**: Fetch library source code, study implementations, generate research reports
2. **Parallel Exploration**: When AI identifies major technical decisions (multiple valid approaches), it autonomously:
   - Creates git worktrees for each approach
   - Runs parallel AI agents to implement each option
   - Evaluates results and recommends the best solution
3. **Plan Review**: Evaluate research and exploration findings, adjust plan if needed
4. **Implement**: Write code following research recommendations and exploration results
5. **Quality Check**: Validate implementation meets acceptance criteria

AI decides when each phase is needed. All of these are autonomous decisions. No manual intervention required.

## Directory Structure

**Aha Loop Repository:**

```
AhaLoop/
├── .agents/skills/             # AI skill library (shared)
│   ├── vision-builder/         # Interactive vision building
│   ├── vision/                 # Vision analysis
│   ├── architect/              # Architecture design
│   ├── roadmap/                # Roadmap planning
│   ├── research/               # Deep research
│   ├── parallel-explore/       # Parallel exploration
│   └── ...                     # Other skills
├── .claude/skills/             # Symlink → .agents/skills
├── .codex/skills/              # Symlink → .agents/skills
├── .god/                       # God Committee
│   ├── config.json             # Committee configuration
│   ├── council/                # Council chamber
│   ├── members/                # Member status (alpha/beta/gamma)
│   └── powers/                 # Power records
├── scripts/aha-loop/           # Execution scripts
│   ├── aha-loop.sh             # PRD executor
│   ├── parallel-explorer.sh    # Parallel exploration
│   ├── config.json             # Execution config
│   └── templates/              # Document templates
├── scripts/god/                # God Committee scripts
├── knowledge/                  # Knowledge base
└── tasks/                      # PRD documents
```

**After using Aha Loop, your project will have:**

```
your-project/
├── project.vision.md           # Project vision (/vision-builder generates)
├── project.vision-analysis.md  # Vision analysis (/vision generates)
├── project.architecture.md     # Tech architecture (/architect generates)
├── project.roadmap.json        # Milestones and PRDs (/roadmap generates)
├── tasks/                      # PRD documents (auto-generated)
│   └── prd-001-xxx.md
└── logs/                       # AI thought logs
    └── ai-thoughts.md
```

## Credits

- [Ralph](https://github.com/snarktank/ralph) - Core PRD execution loop concept
- [Claude](https://www.anthropic.com/claude) - AI capabilities

## License

MIT

## ⭐ Star History

[![Star History Chart](https://api.star-history.com/svg?repos=YougLin-dev/Aha-Loop&type=Date)](https://star-history.com/#YougLin-dev/Aha-Loop&Date)

---

**Links:**

- Example: [AI-Gateway](https://github.com/YougLin-dev/AI-Gateway) - API gateway built with early experimental Aha Loop
- Reference: [snarktank/ralph](https://github.com/snarktank/ralph)

*If you find Aha Loop interesting, please Star*
