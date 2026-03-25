# 🤖 Claudio (v0.1-alpha)

> **C**ommand **L**ine **A**gnostic **U**niversal **D**eveloper **I**nterface **O**perator.

**Claudio** is an open-source, agnostic alternative to Claude Code. While proprietary tools try to lock you into a single model or vendor, Claudio was born to give developers freedom.

Use **any LLM** (Claude 4.6/4, GPT-5.4, Gemini 3.1, DeepSeek) or **local models** (via Ollama) to edit code, run tests, and manage your workflow directly from the terminal.

---

## 🚀 Why Claudio?

The CLI agent market is growing, but most solutions are "black boxes." Claudio focuses on three pillars:

1.  **Model Freedom:** Connect via OpenRouter, direct API keys (Anthropic, OpenAI, Google), or Local (Ollama/Llama.cpp).
2.  **Total Transparency:** See exactly what the agent is reading and how it’s deciding on changes.
3.  **Extensibility:** Built by devs, for devs. Add custom tools for your stack (Vue, Nuxt, Supabase, etc.) in minutes.

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
