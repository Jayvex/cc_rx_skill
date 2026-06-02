# cc-reasonix-dev

> 一个 Claude Code Skill，让 Claude Code 出方案、[Reasonix](https://github.com/esengine/DeepSeek-Reasonix) 自动执行——结合两个 AI 编程助手的优势。

<p align="center">
  <img src="https://img.shields.io/badge/Claude%20Code-Skill-purple?style=flat-square&logo=anthropic&logoColor=white" alt="Claude Code Skill"/>
  <img src="https://img.shields.io/badge/Reasonix-DeepSeek-blue?style=flat-square&logo=deepseek&logoColor=white" alt="Reasonix"/>
  <img src="https://img.shields.io/badge/License-MIT-green?style=flat-square" alt="MIT License"/>
</p>

<p align="center">
  <a href="./README.md">English</a> · <strong>中文</strong>
</p>

## 这是什么？

**cc-reasonix-dev** 是一个 [Claude Code Skill](https://claude.ai/code)，实现两阶段工作流：

1. **Claude Code** 分析项目，生成详细的实施方案
2. **Reasonix**（基于 DeepSeek）按方案逐步执行开发

这样你就能同时获得：
- Claude Code 更强的分析和规划能力
- Reasonix 基于 DeepSeek 前缀缓存的低成本执行（缓存命中率高达 99%）

## 演示

```
$ /reasonix-dev 创建一个 Express.js REST API，包含用户 CRUD 操作

🔍 分析项目结构...
📋 生成实施方案...
📝 方案已写入 .reasonix-plan.md
🚀 交给 Reasonix 执行...

[Reasonix 读取方案并逐步实现]

✅ 完成！方案文件已清理。
```

## 前置条件

使用此 Skill 前，请确保已安装以下工具：

### 1. Claude Code

Claude Code 是 Anthropic 的终端 AI 编程助手。

```bash
# 安装 Claude Code
npm install -g @anthropic-ai/claude-code

# 或使用桌面端：https://claude.ai/download
```

- **官网**：https://claude.ai/code
- **文档**：https://docs.anthropic.com/en/docs/claude-code

### 2. Node.js（v22+）

Reasonix 需要 Node.js 22 或更高版本。

```bash
# 检查 Node 版本
node --version

# 通过 nvm 安装（推荐）
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
nvm install 22
nvm use 22

# 或从官网下载：https://nodejs.org/
```

### 3. Reasonix

Reasonix 是一个 DeepSeek 原生的终端 AI 编程助手。

```bash
# 全局安装
npm install -g reasonix

# 或使用 npx（无需安装）
npx reasonix code
```

- **GitHub**：https://github.com/esengine/DeepSeek-Reasonix
- **官网**：https://esengine.github.io/DeepSeek-Reasonix/

### 4. DeepSeek API Key

Reasonix 使用 DeepSeek API 进行推理。

1. 前往 https://platform.deepseek.com/api_keys
2. 创建 API Key
3. 设置环境变量：

```bash
# Linux / macOS
export DEEPSEEK_API_KEY=sk-your-key-here

# Windows PowerShell
$env:DEEPSEEK_API_KEY = "sk-your-key-here"

# Windows CMD
set DEEPSEEK_API_KEY=sk-your-key-here
```

或者在首次运行 Reasonix 时按提示输入——它会自动保存 Key。

## 安装 Skill

### 方式一：克隆仓库后复制（推荐）

```bash
# 克隆本仓库
git clone https://github.com/Jayvex/cc_rx_skill.git
cd cc_rx_skill

# 复制 Skill 到 Claude Code 的 skills 目录
# 全局安装（所有项目可用）：
cp -r .claude/skills/reasonix-dev ~/.claude/skills/

# 或按项目安装（仅当前项目可用）：
cp -r .claude/skills/reasonix-dev /你的项目路径/.claude/skills/
```

### 方式二：手动创建

```bash
# 创建 Skill 目录
mkdir -p ~/.claude/skills/reasonix-dev

# 下载 Skill 文件
curl -o ~/.claude/skills/reasonix-dev/SKILL.md \
  https://raw.githubusercontent.com/Jayvex/cc_rx_skill/main/.claude/skills/reasonix-dev/SKILL.md
```

### 验证安装

在 Claude Code 中输入 `/reasonix-dev`——如果出现自动补全提示，说明安装成功。

## 使用方法

```
/reasonix-dev <你的任务描述>
```

### 示例

```bash
# 添加功能
/reasonix-dev 添加用户认证功能，使用 JWT 和 bcrypt 密码加密

# 重构代码
/reasonix-dev 将数据库层从原生 SQL 重构为 Prisma ORM

# 添加测试
/reasonix-dev 使用 Jest 为所有 Controller 函数编写单元测试

# 创建新项目
/reasonix-dev 创建一个 CLI 工具，支持将 CSV 文件流式转换为 JSON

# 修复问题
/reasonix-dev 修复 WebSocket 连接处理器中的内存泄漏
```

### 可选参数

可以在任务描述中传入参数：

```bash
# 指定模型
/reasonix-dev --model deepseek-pro 实现 OAuth2 登录，支持 Google 和 GitHub

# 限制费用
/reasonix-dev --budget 1.00 为所有模块添加完善的错误处理

# 最大推理强度
/reasonix-dev --effort max 优化数据库查询层，性能提升 10 倍
```

## 工作原理

```
┌─────────────────────────────────────────────────────┐
│                    你的任务                           │
│   "添加用户认证，使用 JWT 和 bcrypt"                  │
└──────────────────────┬──────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────┐
│              Claude Code（阶段 1）                    │
│                                                      │
│  • 扫描项目结构                                       │
│  • 识别技术栈和代码模式                                │
│  • 生成详细的分步实施方案                              │
│  • 将方案写入 .reasonix-plan.md                       │
└──────────────────────┬──────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────┐
│              Reasonix（阶段 2）                       │
│                                                      │
│  • 读取方案文件                                       │
│  • 按方案创建/修改文件                                 │
│  • 遵循方案中的代码规范                                │
│  • 报告执行进度                                       │
└──────────────────────┬──────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────┐
│                   清理                                │
│  • 删除 .reasonix-plan.md                             │
│  • 完成！🎉                                         │
└─────────────────────────────────────────────────────┘
```

## 为什么用两个 Agent？

| 方面 | Claude Code | Reasonix |
|------|-------------|----------|
| **优势** | 深度分析、架构设计、方案规划 | 低成本高效执行 |
| **模型** | Claude (Anthropic) | DeepSeek（前缀缓存优化） |
| **成本** | 每 token 较高 | 缓存命中时约低 99% |
| **适合** | "做什么" | "怎么做" |

两者结合，既获得 Claude Code 优质的规划能力，又享受 Reasonix 的低成本执行。

## 常见问题

**Q：可以不用 Reasonix 吗？**
A：不行，此 Skill 专门设计为 Claude Code → Reasonix 的工作流。你可以修改 Skill 适配其他 Agent，但默认流程需要 Reasonix。

**Q：Reasonix 执行出错怎么办？**
A：Claude Code 生成的方案非常详细，包含精确的文件路径和代码模式。如果 Reasonix 偏离方案，可以重新运行 Skill 或手动修复。

**Q：费用大概多少？**
A：Claude Code 的费用取决于你的 Anthropic 套餐。Reasonix 的费用取决于 DeepSeek 定价，但借助前缀缓存稳定性，一般任务约 $0.005。

**Q：可以用于已有项目吗？**
A：当然可以！这是主要使用场景。Claude Code 会分析你的现有代码库，生成尊重项目模式的方案。

## 许可证

MIT License — 详见 [LICENSE](LICENSE)。

## 致谢

- [Claude Code](https://claude.ai/code) by Anthropic
- [Reasonix](https://github.com/esengine/DeepSeek-Reasonix) by esengine
- [DeepSeek](https://deepseek.com) 提供 API 支持
