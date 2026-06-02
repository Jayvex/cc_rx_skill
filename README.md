# cc-reasonix-dev

> A Claude Code skill that lets Claude Code plan and [Reasonix](https://github.com/esengine/DeepSeek-Reasonix) execute — combining the best of both AI coding agents.

<p align="center">
  <img src="https://img.shields.io/badge/Claude%20Code-Skill-purple?style=flat-square&logo=anthropic&logoColor=white" alt="Claude Code Skill"/>
  <img src="https://img.shields.io/badge/Reasonix-DeepSeek-blue?style=flat-square&logo=deepseek&logoColor=white" alt="Reasonix"/>
  <img src="https://img.shields.io/badge/License-MIT-green?style=flat-square" alt="MIT License"/>
</p>

<p align="center">
  <strong>English</strong> · <a href="./README.zh-CN.md">中文</a>
</p>

## What Is This?

**cc-reasonix-dev** is a [Claude Code skill](https://claude.ai/code) that orchestrates a two-phase workflow:

1. **Claude Code** analyzes your project and generates a detailed implementation plan
2. **Reasonix** (powered by DeepSeek) executes the plan step by step

This gives you the best of both worlds:
- Claude Code's superior reasoning for architecture and planning
- Reasonix's cost-effective execution powered by DeepSeek's prefix-cache stability (up to 99% cache hit rate)

## Demo

```
$ /reasonix-dev Create a REST API with Express.js, including user CRUD operations

🔍 Analyzing project structure...
📋 Generating implementation plan...
📝 Plan written to .reasonix-plan.md
🚀 Handing off to Reasonix for execution...

[Reasonix reads the plan and implements each step]

✅ Done! Plan file cleaned up.
```

## Prerequisites

Before using this skill, make sure you have:

### 1. Claude Code

Claude Code is Anthropic's AI coding agent for the terminal.

```bash
# Install Claude Code
npm install -g @anthropic-ai/claude-code

# Or use the desktop app: https://claude.ai/download
```

- **Website**: https://claude.ai/code
- **Docs**: https://docs.anthropic.com/en/docs/claude-code

### 2. Node.js (v22+)

Reasonix requires Node.js 22 or later.

```bash
# Check your Node version
node --version

# Install via nvm (recommended)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
nvm install 22
nvm use 22

# Or download from https://nodejs.org/
```

### 3. Reasonix

Reasonix is a DeepSeek-native AI coding agent for your terminal.

```bash
# Install globally
npm install -g reasonix

# Or use npx (no install needed)
npx reasonix code
```

- **GitHub**: https://github.com/esengine/DeepSeek-Reasonix
- **Website**: https://esengine.github.io/DeepSeek-Reasonix/

### 4. DeepSeek API Key

Reasonix uses the DeepSeek API for inference.

1. Go to https://platform.deepseek.com/api_keys
2. Create an API key
3. Set it as an environment variable:

```bash
# Linux / macOS
export DEEPSEEK_API_KEY=sk-your-key-here

# Windows PowerShell
$env:DEEPSEEK_API_KEY = "sk-your-key-here"

# Windows CMD
set DEEPSEEK_API_KEY=sk-your-key-here
```

Or let Reasonix prompt you on first run — it persists the key automatically.

## Installation

### Option A: Clone & Copy (Recommended)

```bash
# Clone this repo
git clone https://github.com/Jayvex/cc_rx_skill.git
cd cc_rx_skill

# Copy the skill to your Claude Code skills directory
# Global (available in all projects):
cp -r .claude/skills/reasonix-dev ~/.claude/skills/

# Or per-project (available in one project):
cp -r .claude/skills/reasonix-dev /path/to/your/project/.claude/skills/
```

### Option B: Manual Setup

```bash
# Create the skill directory
mkdir -p ~/.claude/skills/reasonix-dev

# Download the skill file
curl -o ~/.claude/skills/reasonix-dev/SKILL.md \
  https://raw.githubusercontent.com/Jayvex/cc_rx_skill/main/.claude/skills/reasonix-dev/SKILL.md
```

### Verify Installation

In Claude Code, type `/reasonix-dev` — if it appears in the autocomplete, the skill is installed.

## Usage

```
/reasonix-dev <your task description>
```

### Examples

```bash
# Add a feature
/reasonix-dev Add user authentication with JWT tokens and bcrypt password hashing

# Refactor code
/reasonix-dev Refactor the database layer from raw SQL queries to Prisma ORM

# Add tests
/reasonix-dev Write unit tests for all controller functions using Jest

# Create something new
/reasonix-dev Create a CLI tool that converts CSV files to JSON with streaming support

# Fix issues
/reasonix-dev Fix the memory leak in the WebSocket connection handler
```

### Optional Parameters

You can pass parameters in the task description:

```bash
# Use a specific model
/reasonix-dev --model deepseek-pro Implement OAuth2 with Google and GitHub providers

# Cap spending
/reasonix-dev --budget 1.00 Add comprehensive error handling across all modules

# Maximum reasoning effort
/reasonix-dev --effort max Optimize the database query layer for 10x performance
```

## How It Works

```
┌─────────────────────────────────────────────────────┐
│                    Your Task                         │
│   "Add user auth with JWT and bcrypt"               │
└──────────────────────┬──────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────┐
│              Claude Code (Phase 1)                   │
│                                                      │
│  • Scans project structure                           │
│  • Identifies tech stack & patterns                  │
│  • Generates detailed step-by-step plan              │
│  • Writes plan to .reasonix-plan.md                  │
└──────────────────────┬──────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────┐
│              Reasonix (Phase 2)                      │
│                                                      │
│  • Reads the plan file                               │
│  • Creates/modifies files as specified               │
│  • Follows code conventions from the plan            │
│  • Reports progress                                  │
└──────────────────────┬──────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────┐
│                   Cleanup                            │
│  • Removes .reasonix-plan.md                         │
│  • Done! 🎉                                         │
└─────────────────────────────────────────────────────┘
```

## Why Two Agents?

| Aspect | Claude Code | Reasonix |
|--------|-------------|----------|
| **Strength** | Deep analysis, architecture, planning | Cost-effective execution |
| **Model** | Claude (Anthropic) | DeepSeek (prefix-cache optimized) |
| **Cost** | Higher per token | ~99% cheaper with cache |
| **Best for** | "What to build" | "Build it" |

By combining them, you get Claude Code's superior planning at the top, and Reasonix's cost-efficient execution at the bottom.

## FAQ

**Q: Can I use this without Reasonix?**
A: No, this skill specifically requires Reasonix for the execution phase. You could adapt the skill to use another agent, but the default flow is Claude Code → Reasonix.

**Q: What if Reasonix makes mistakes?**
A: Claude Code generates very detailed plans with exact file paths and code patterns. If Reasonix deviates, you can re-run the skill or manually fix the issues.

**Q: How much does it cost?**
A: Claude Code costs depend on your Anthropic plan. Reasonix costs depend on DeepSeek pricing, but with prefix-cache stability, typical runs cost ~$0.005 for moderate tasks.

**Q: Can I use it with existing projects?**
A: Yes! That's the main use case. Claude Code analyzes your existing codebase and generates plans that respect your project's patterns.

## License

MIT License — see [LICENSE](LICENSE) for details.

## Acknowledgments

- [Claude Code](https://claude.ai/code) by Anthropic
- [Reasonix](https://github.com/esengine/DeepSeek-Reasonix) by esengine
- [DeepSeek](https://deepseek.com) for the API
