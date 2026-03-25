# 🤖 Claudio (v0.1-alpha)

> **C**ommand **L**ine **A**gnostic **U**niversal **D**eveloper **I**nterface **O**perator.

**Claudio** is an open-source, agnostic alternative to Claude Code. While proprietary tools try to lock you into a single model or vendor, Claudio was born to give developers freedom.

Use **any LLM** (Claude 4.6, GPT-5.4, Gemini 3.1, DeepSeek) or **local models** (via Ollama) to edit code, run tests, and manage your workflow directly from the terminal.

---

## 🚀 Why Claudio?

The CLI agent market is growing, but most solutions are "black boxes." Claudio focuses on three pillars:

1.  **Model Freedom:** Connect via OpenRouter, direct API keys (Anthropic, OpenAI, Google), or Local (Ollama/Llama.cpp).
2.  **Total Transparency:** See exactly what the agent is reading and how it’s deciding on changes.
3.  **Extensibility:** Built by devs, for devs. Add custom tools for your stack (Vue, Nuxt, Supabase, etc.) in minutes.

## 🏗️ Reference Architecture: How Claude Code Works

Claudio is inspired by — and designed to be a model-agnostic alternative to — Claude Code. Understanding its architecture is key to building Claudio well.

### The Agentic Loop

At the heart of Claude Code is a **single-threaded master loop** (sometimes called the "agentic loop" or "ReAct loop"). Each user prompt is processed in a continuous cycle of three phases:

```
User Input ──► Collect Context ──► Take Action ──► Verify Results
                   ▲                                      │
                   └──────────────────────────────────────┘
                          (repeats until goal is met)
```

1. **Collect Context** — Claude reads files, runs commands, or searches the codebase to understand the current state.
2. **Take Action** — Claude modifies code, executes shell commands, generates commits, updates dependencies, or runs tests.
3. **Verify Results** — Claude checks logs, test results, or command outputs to confirm the action had the desired effect.

This loop repeats iteratively until the task is complete or the user intervenes.

### Tool Suite

Claude Code becomes truly "agentic" through a set of **permissioned tools** that allow it to operate directly on the developer's environment:

| Tool | Purpose |
|---|---|
| `BashTool` | Execute shell commands (with user approval for sensitive ops) |
| `ReadTool` / `WriteTool` / `EditTool` | Read and manipulate files |
| `GrepTool` / `GlobTool` | Search for code patterns and find files |
| `TaskTool` / `AgentTool` | Spawn isolated sub-agents for parallel tasks |

All tools are permission-controlled — destructive or sensitive actions require explicit developer confirmation.

### Context & Memory Management

Claude Code uses a **200k-token context window** to reason over entire projects. To handle long-running sessions, it employs two memory layers:

- **Short-term memory**: The active session transcript (messages, tool outputs, file contents) held in the context window.
- **Long-term memory**: When approaching token limits, an auto-compaction mechanism summarizes less-relevant history. Key decisions, architecture notes, and coding style rules are persisted to a `CLAUDE.md` file in the repository, which the agent reads at the start of each session.

### Sub-agents & MCP (Model Context Protocol)

For complex or parallelizable tasks, Claude Code can **spawn isolated sub-agents**, each with their own scope and memory — similar to delegating to junior developers for independent subtasks. This enables multi-agent collaboration without losing control.

**MCP (Model Context Protocol)** is an open protocol that allows the agent to connect to external tools and services (databases, APIs, CI pipelines, company wikis) via dynamic context injections — enabling extensibility without baking services directly into the agent.

### Applying This Architecture to Claudio

Claudio aims to replicate this architecture in an **open, model-agnostic** way:

- The **ReAct Loop** (Phase 1 of the roadmap) implements the same Think → Act → Observe cycle.
- The **Tool Suite** will be extensible — developers can add custom tools for their stack.
- **CLAUDIO.md** (equivalent to `CLAUDE.md`) will serve as the long-term memory file.
- **MCP integration** will allow connecting to any external service.
- Instead of a single LLM, **any model** can power the loop via OpenRouter, direct APIs, or local models.

> **References:**
> - [How Claude Code Works — Official Docs](https://code.claude.com/docs/en/how-claude-code-works)
> - [Claude Code Overview](https://code.claude.com/docs/en/overview)
> - [Claude Code System Architecture (DeepWiki)](https://deepwiki.com/anthropics/claude-code/1.1-system-architecture)

---

## 🛠️ Tech Stack (Proposed)

* **Runtime:** [Bun](https://bun.sh/) (for maximum startup and execution speed).
* **Orchestration:** [Vercel AI SDK](https://sdk.vercel.ai/) (native multi-provider support).
* **Terminal UI:** [Ink](https://github.com/vadimdemedes/ink) (React-based interactive interface).
* **Code Analysis:** [Tree-sitter](https://tree-sitter.github.io/tree-sitter/) for structural file understanding.

## 🗺️ Launch Roadmap

- [ ] **Phase 1:** MVP with basic ReAct Loop (Think -> Act -> Observe).
- [ ] **Phase 2:** Unified Diff support for file editing without corrupting code.
- [ ] **Phase 3:** Local file indexing (RAG) for large projects.
- [ ] **Phase 4:** Native Docker integration for secure command execution.

## ✨ Planned Features (inspired by Claude Code)

The features below are based on the functionalities of Claude Code (Anthropic) and represent what Claudio plans to implement to match and surpass the proprietary tool — with full model freedom.

### 🖥️ Multi-Platform Integration
- [ ] **Native Terminal Agent** — context-aware agent running directly in the shell.
- [ ] **VS Code Extension** — live sidebar with visual diffs and inline code changes.
- [ ] **JetBrains Extension** — integration for IntelliJ, WebStorm, and other JetBrains IDEs.
- [ ] **Web Interface** — use Claudio from any browser without local installation.
- [ ] **Mobile (iOS/Android)** — assign, review, and tweak tasks on the go.
- [ ] **Slack Bot** — coding assistance integrated into collaborative Slack environments.

### 🤖 Autonomous & Agentic Coding
- [ ] **End-to-End Development Cycles** — code generation, refactoring, debugging, testing, and explanations across entire repositories.
- [ ] **Full Codebase Context** — understands the entire project without additional indexing servers or setup.
- [ ] **Long-Running Sessions** — execute multi-step programming tasks consistently over extended sessions.
- [ ] **Auto Mode** — proactively finds and runs repetitive or required code actions with guardrails.

### ⏪ Checkpoint System ("Time-Travel Debugging")
- [ ] **Automatic State Snapshots** — saves code state before every AI-generated change.
- [ ] **Instant Restore (Rewind)** — restore code, conversation, or both to any previous checkpoint (`/rewind` command).
- [ ] **Git Integration** — works alongside version control, specifically safeguarding AI-initiated edits.

### 🔀 Subagents & Hooks
- [ ] **Specialized Sub-Agents** — create dedicated agents (e.g., one for migrations, one for testing) that run concurrently and independently.
- [ ] **File/Directory Hooks** — react to file or directory changes for complex workflow automation.
- [ ] **Agent Parallelization** — run multiple agents simultaneously for full codebase management.

### 🛠️ Developer Tasks & Commands
- [ ] **Bug Fixing & Debugging** — identify and fix bugs across the codebase with natural language.
- [ ] **Multi-File Refactoring** — rename, restructure, and refactor across many files at once.
- [ ] **Automated Testing & CI/CD Integration** — write and run tests, integrate with CI/CD pipelines.
- [ ] **Automated Pull Request Handling** — create, review, and merge PRs through natural language commands.
- [ ] **Natural-Language Codebase Explanations** — ask questions about any part of the codebase and get clear explanations.
- [ ] **Documentation Generation** — search, cross-reference, and generate documentation automatically.
- [ ] **DevOps / Housekeeping via CLI** — run routine dev-ops tasks through bash/CLI commands.

### 🔌 API & SDK
- [ ] **Agent SDK** — build custom workflows and agents with full context, permission, and subagent management.
- [ ] **Custom CI/CD Integration** — connect Claudio to any custom pipeline or compliance workflow.
- [ ] **`claudio.md` Project Files** — per-project configuration files for easy onboarding and advanced setup.

### 🏢 Enterprise Features
- [ ] **SSO & User Permissions** — Single Sign-On and fine-grained permissions management.
- [ ] **Usage Management & Audit Logs** — track usage across teams for compliance and billing.
- [ ] **External Service Integrations** — connect to Microsoft 365, SharePoint, OneDrive, Outlook, and Teams for cross-source context.

---

## 🤝 How to contribute?

We need architects, prompt engineers, and developers passionate about productivity.

1.  **Give a ⭐️ to the repo** to help with visibility.
2.  **Open an [Issue](https://github.com/your-user/claudio/issues)** with feature or architecture suggestions.
3.  **Fork it** and submit your Pull Request! We are currently defining commit message standards and code styles.

---

## 📜 License

Distributed under the **Apache 2.0** license. Feel free to use, modify, and distribute.

---
*Built with ☕ and a desire to break vendor lock-in.*
