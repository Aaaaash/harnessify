<p align="center">
  <img src="logo.png" width="200" />
</p>

<h1 align="center">Harnessify</h1>

<p align="center">
  <strong>A Meta Skill for building AI Agent autonomous development engineering frameworks for any software project</strong>
</p>

---

## Why Harness Engineering?

Martin Fowler offered a precise equation in his writing on AI Agent engineering:

> **Agent = Model + Harness**

The Model is the large language model itself, while the **Harness** encompasses all the configuration, constraints, and control mechanisms surrounding the model — it determines whether an Agent can evolve from "able to chat" to "able to deliver."

This equation reveals a widely underestimated truth: **The upper bound of an LLM's capability depends not on the model itself, but on the engineering quality of its Harness.**

### Feedforward and Feedback: Two Paths to Controlling an Agent

The Harness controls Agent behavior through two complementary approaches:

| Control Method | Mechanism | Typical Means | Timing |
|----------------|-----------|---------------|--------|
| **Feedforward** | Preventive guidance | AGENTS.md, Skills, module docs, modification boundaries | Before code is written |
| **Feedback** | Corrective detection | Type-Check, Lint, unit tests, E2E tests | After code is written |

The key principle is **"Keep quality left"** — the earlier a problem is caught, the lower the cost to fix it. A clear module document (Feedforward) can prevent far more errors than catching them after the fact through tests (Feedback). Good Harness Engineering strengthens both paths, building a complete quality assurance chain.

### AGENTS.md: From De Facto Standard to Open Standard

In 2025, the **AGENTS.md** specification, jointly introduced by OpenAI and Anthropic, was donated to the **Agentic AI Foundation** (hosted by the Linux Foundation), becoming the open standard for AI Agents to understand project context. As of now:

- **60,000+** open-source projects have adopted it
- **23+** AI coding tools natively support it (Claude Code, GitHub Copilot, Cursor, Windsurf, Zed, JetBrains Junie, etc.)

AGENTS.md solves the problem of "how an Agent understands a project," but it is only the starting point.

### AGENTS.md Alone Is Not Enough

LLMs are non-deterministic. Agents don't understand code semantics — they process tokens. When a project grows to dozens of modules and hundreds of files, a single AGENTS.md cannot answer questions like:

- What are the modification boundaries for this module? Which files are off-limits?
- If a Store's interface is changed, which downstream components need to be updated accordingly?
- After the Agent finishes writing code, what commands should be run, and in what order, to verify it?
- How should tests be written? Where should they go? What scenarios should they cover?

**These questions require a systematic engineering framework to answer — that is Harness Engineering.**

### The Unique Value of This Project

`harnessify` is the first systematic engineering framework that addresses "how to enable AI Agents to develop autonomously in large-scale projects." Its core principles are:

1. **Documentation-Driven Development**: Agents read documentation first, then write code, rather than blindly exploring the codebase
2. **Modification Boundary Protection**: Define explicit allow/deny rules for each module, preventing Agents from making out-of-scope changes
3. **Complete Verification Chain**: Build a multi-layered verification pipeline from static checks to E2E tests
4. **Non-Invasive to Business Code**: Only add configuration, documentation, and tests — never modify existing code

---

## 📦 What Is This?

`harnessify` is a **Meta Skill** — it is not a specific product or framework, but a set of **engineering methodology + standardized templates** that help you build a complete engineering system for AI Agent autonomous development in any software project.

Think of it as "the Skill for building Skills":

- A regular Skill solves specific problems (writing code, fixing bugs, refactoring)
- **A Meta Skill solves engineering problems** (how to enable Agents to solve specific problems efficiently, safely, and verifiably)

It generates a complete documentation system and verification infrastructure for a target project through a **5-phase** standardized process:

```
Project Analysis → Testing Infrastructure Setup → Documentation System Creation → Seed Test Writing → End-to-End Verification
```

---

## ✨ Core Features

### Progressive Documentation System

Uses a **Progressive Disclosure** strategy so the Agent doesn't need to load the entire project's information at once:

```
AGENTS.md (index entry point)
  → docs/modules/*.md (load module docs on demand)
  → docs/WORKFLOW.md (development workflow)
  → docs/TESTING.md (testing guidelines)
```

### Modification Boundary Definitions

This is the most critical innovation of the framework. Every module document includes explicit modification boundaries:

- **Allowed**: Which operations within the module are safe
- **Forbidden**: Which operations must never be performed
- **Cascading Updates**: After modifying a certain interface, which files must be updated in sync

This is the key mechanism for preventing Agents from making "out-of-scope modifications" in large projects.

### Multi-Layered Verification Pipeline

Builds a progressive verification chain from fast to slow:

```
Layer 1: Type-Check + Lint     (seconds, static analysis)
Layer 2: Unit Tests             (seconds to minutes, logic verification)
Layer 3: E2E Tests              (minutes, user flow verification)
```

### Standardized Workflow

Defines a complete process for the Agent from requirement understanding to code submission, including Pre-flight Checklist, verification commands, failure handling strategies, and documentation update rules.

### Seed Test Auto-Generation

Writes runnable seed tests for each verification layer, proving the pipeline works end-to-end, while providing reusable templates for future tests.

---

## ⚙️ How It Works

### 5-Phase Process

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│  Phase 1    │     │  Phase 2    │     │  Phase 3    │     │  Phase 4    │     │  Phase 5    │
│  Analysis   │────▶│  Infra      │────▶│  Docs       │────▶│  Seed Tests │────▶│  E2E Verify │
└─────────────┘     └─────────────┘     └─────────────┘     └─────────────┘     └─────────────┘
```

| Phase | Objective | Key Outputs |
|-------|-----------|-------------|
| **Phase 1: Project Analysis** | Understand project architecture and technology choices | Tech stack inventory, module breakdown, build configuration |
| **Phase 2: Testing Infrastructure** | Install verification toolchain | Unit test / E2E / Lint configuration, standardized scripts |
| **Phase 3: Documentation System** | Establish progressive documentation architecture | AGENTS.md, module docs, WORKFLOW.md, TESTING.md |
| **Phase 4: Seed Tests** | Prove the verification pipeline is runnable | Seed test files for each module |
| **Phase 5: End-to-End Verification** | Ensure the entire system works correctly | All verifications pass, documentation completeness check |

**Phase 2 and Phase 3 can be executed in parallel** to improve setup efficiency.

---

## 🚀 Quick Start

### Prerequisites

- An existing software project (any language and framework supported)
- An AI coding tool that supports the AGENTS.md standard

### Usage

1. **Invoke this Skill through an AI coding tool**

   In an AI coding tool that supports Skills, simply tell the Agent:

   > "Set up an Agent development workflow for this project"

   Or:

   > "Set up agent workflow", "Create development documentation system", "Build testing infrastructure"

2. **The Agent will automatically execute the 5-phase process**

   - Analyze your project structure and tech stack
   - Install and configure the testing toolchain
   - Generate the complete documentation system
   - Write seed tests
   - Run end-to-end verification

3. **Verify the outputs**

   ```bash
   # Confirm the verification pipeline is working
   npm run typecheck    # or the equivalent command for your language
   npm run lint
   npm run test:unit
   npm run test:e2e
   ```

---

## 📁 Generated Documentation Structure

After use, the following files will be added to your project:

```
your-project/
├── AGENTS.md                         # Project entry: overview + module index + quick commands
├── docs/
│   ├── WORKFLOW.md                   # Agent development workflow (5-phase standard process)
│   ├── TESTING.md                    # Testing guidelines (with reusable templates)
│   └── modules/                      # Module-level documentation
│       ├── auth.md                   # Example: authentication module
│       ├── dashboard.md              # Example: dashboard module
│       └── shared.md                 # Example: shared layer
├── e2e/                              # E2E test directory
│   └── *.spec.ts
└── [test config files]               # vitest.config.ts / playwright.config.ts etc.
```

Responsibilities of each file:

| File | Responsibility | When to Read |
|------|---------------|--------------|
| `AGENTS.md` | Global project index, the Agent's first entry point | At the start of every task |
| `docs/modules/*.md` | Module details: file inventory, interfaces, modification boundaries | Before modifying a module |
| `docs/WORKFLOW.md` | Standard development process and verification commands | During development |
| `docs/TESTING.md` | Testing guidelines and templates | When writing tests |

---

## 🎯 Use Cases

**Best suited for:**

- Medium to large projects (10+ modules) that need Agents to develop autonomously within clear boundaries
- Teams that want AI Agents to participate safely in day-to-day development
- Projects lacking testing infrastructure that want to set up a complete verification chain in one go
- Multi-developer projects that need unified Agent development standards and workflows

**Supported project types:**

| Project Type | Unit Tests | E2E Tests | Lint |
|--------------|-----------|-----------|------|
| React / Vue / Svelte (Vite) | Vitest + Testing Library | Playwright | ESLint |
| React (Next.js / CRA) | Jest + Testing Library | Playwright | ESLint |
| Node.js | Vitest or Jest | As needed | ESLint |
| Python | pytest | Playwright (if web) | ruff / flake8 |
| Go | go test | As needed | golangci-lint |

---

## 🔗 Compatibility

This framework is built on the **AGENTS.md open standard** and is compatible with all AI coding tools that support it:

| Tool | Support Status |
|------|---------------|
| Claude Code | ✅ Native support |
| GitHub Copilot | ✅ Native support |
| Cursor | ✅ Native support |
| Windsurf | ✅ Native support |
| Zed | ✅ Native support |
| JetBrains Junie | ✅ Native support |
| OpenAI Codex | ✅ Native support |
| VS Code Copilot Agent | ✅ Native support |
| Qoder | ✅ Native support |

And **23+** more tools are continuously being integrated. As long as your tool supports reading AGENTS.md, it works seamlessly with this framework.

---

## 🏪 Skill Ecosystem & Listings

`harnessify` is a standardized Skill, planned for listing on the following platforms:

| Platform | Link |
|----------|------|
| Vercel Agent Skills | [vercel.com/docs/agent-resources/skills](https://vercel.com/docs/agent-resources/skills) |
| LobeHub Skills Marketplace | [lobehub.com/skills](https://lobehub.com/skills) |
| Qoder Skills Marketplace | [qoder.com/marketplace](https://qoder.com/marketplace) |
| Skills.sh | [skills.sh](https://skills.sh/) |
| Awesome Agent Skills | [github.com/VoltAgent/awesome-agent-skills](https://github.com/VoltAgent/awesome-agent-skills) |

---

## 🤝 Contributing

Contributions are welcome! Here's how you can get involved:

1. **Open an Issue**: Report bugs, suggest improvements, or discuss new features
2. **Submit a PR**: Improve templates, enhance documentation, or optimize processes
3. **Share Your Experience**: If you've used this framework in your project, we'd love to hear about it

When contributing, please follow these guidelines:

- Keep template content generic and avoid binding to specific frameworks
- New templates must include accompanying documentation
- Clearly describe the motivation and impact of your changes in the PR description

---

## 📄 License

[MIT](LICENSE)

---

## 🙏 Acknowledgments

- **Martin Fowler** — The originator of Harness Engineering theory, whose definition of "Agent = Model + Harness" provided a thinking framework for the entire field
- **OpenAI & Anthropic** — Co-initiators of the AGENTS.md standard
- **Agentic AI Foundation (Linux Foundation)** — The steward organization of the AGENTS.md open standard, driving industry consensus on Agent engineering
- All development teams behind the **23+ AI coding tools** that support the AGENTS.md standard
- Every developer who has contributed wisdom to the practice of Agent engineering
