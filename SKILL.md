---
name: harnessify
description: Scaffold a complete agent-driven development workflow for any project. Analyzes project architecture, sets up progressive-disclosure documentation (AGENTS.md as index + per-module docs), integrates a multi-layer verification pipeline (type-check, lint, unit tests, E2E tests), creates WORKFLOW.md and TESTING.md, and writes seed tests. Use when the user wants to set up an agent workflow, enable autonomous agent development, create development documentation, or scaffold testing infrastructure for a project.
---

# Setup Agent Workflow

Scaffold an agent-driven autonomous development workflow for any project: progressive-disclosure documentation + verification pipeline + development conventions.

## Trigger Scenarios

When the user wants to:
- Enable AI Agents to autonomously complete feature development and verified delivery
- Set up a development workflow / documentation system for a project
- Integrate testing infrastructure (unit tests + E2E tests + lint)

## Execution Flow

```
Phase 1: Project Analysis → Phase 2: Infrastructure → Phase 3: Documentation System → Phase 4: Seed Tests → Phase 5: Verification
```

---

## Phase 1: Project Analysis

**Goal**: Understand project architecture and determine the technology stack.

### Information to Gather

| Dimension | Method | Key Output |
|-----------|--------|------------|
| Tech Stack | Read `package.json` / `requirements.txt` / `go.mod`, etc. | Framework, language, build tools |
| Project Structure | Traverse `src/` directory | Module breakdown, entry files |
| Build Config | Read build configuration files | Build commands, path aliases |
| Existing Tests | Check test framework installation and config | Current test status |
| State Management | Read Store files | State structure and Actions |
| Type Definitions | Read Types files | Data models |

### Module Identification Rules

Split modules at the following granularity (each module corresponds to one document):
- **Feature directory** = one module (e.g., `components/sidebar/` → sidebar module)
- **State management** = standalone module (e.g., `stores/` → stores module)
- **Shared layer** = standalone module (e.g., `shared/` or `common/`)
- Multi-app projects: modules under each app are numbered independently

---

## Phase 2: Set Up Testing Infrastructure

**Goal**: Install the verification toolchain without modifying existing business code.

### Tech Stack Selection Table

Based on the tech stack identified in Phase 1, choose the corresponding tools:

| Project Type | Unit Tests | E2E Tests | Lint |
|-------------|------------|-----------|------|
| React/Vue/Svelte (Vite) | Vitest + @testing-library | Playwright | ESLint |
| React (CRA/Next.js) | Jest + @testing-library | Playwright | ESLint |
| Node.js | Vitest or Jest | As needed | ESLint |
| Python | pytest | Playwright (if web) | ruff / flake8 |
| Go | go test | As needed | golangci-lint |

### General Steps

1. **Install dependencies**: All as devDependencies
2. **Create configuration files**:
   - Unit test config (environment, path aliases, coverage)
   - E2E test config (browsers, baseURL, dev server)
   - Lint config (language rules; do not enable type-aware rules to avoid blocking on existing code)
3. **Add scripts**: Use standardized command names (see table below)
4. **Update .gitignore**: Exclude test artifacts

### Standard Script Names

```json
{
  "typecheck": "...",
  "lint": "...",
  "lint:fix": "...",
  "test:unit": "...",
  "test:unit:watch": "...",
  "test:e2e": "...",
  "test": "..."
}
```

### Configuration Guidelines

- Unit test config must maintain **path alias consistency** with the main build config
- E2E config should set up **webServer** to automatically start the dev server
- Lint config uses **lenient mode** (does not block on existing code)
- Multi-entry projects (MPA) require independent E2E projects for each entry point

---

## Phase 3: Documentation System Creation

**Goal**: Establish a progressive-disclosure documentation architecture.

### Language Adaptation

Generate all documentation (AGENTS.md, module docs, WORKFLOW.md, TESTING.md) in the same language as the user's request. Default to English if unclear.

### Documentation Structure

```
AGENTS.md                         # Main entry: project overview + module index
├── docs/
│   ├── WORKFLOW.md               # Agent development workflow
│   ├── TESTING.md                # Testing conventions
│   └── modules/                  # Module-level docs (read on demand)
│       ├── [module-name].md
│       └── ...
```

### 3.1 Refactor/Create AGENTS.md

AGENTS.md serves as the **index entry point** with this structure:

1. **Project Overview** (2–3 sentences)
2. **Tech Stack Table**
3. **Quick Commands** (dev/build/test/lint/typecheck)
4. **Module Index Table** (core): one row per module, linking to docs/modules/*.md
5. **Agent Workflow Links**: pointing to docs/WORKFLOW.md and docs/TESTING.md
6. **Development Conventions** (concise rules, one per line)
7. **Current Progress**

Key principles:
- **Do not dump component lists**; details go into module docs
- Module index table grouped by app/layer
- Each module entry includes: module name, doc link, path, one-line responsibility

### 3.2 Create Module Docs

One document per module, using the unified template at [templates/module-template.md](templates/module-template.md).

Key content:
- **Responsibility**: one sentence
- **File List**: table listing all files
- **Public Interface**: exports + dependencies
- **Modification Boundaries**: allowed / prohibited / cascading updates (the most important section)
- **Data Flow**: how data flows in and out
- **Test Requirements**: test file paths + scenarios to cover

### 3.3 Create WORKFLOW.md

End-to-end agent development workflow, template at [templates/workflow-template.md](templates/workflow-template.md).

5 Phases:
1. Requirement Understanding → read AGENTS.md to locate modules
2. Read module docs → clarify boundaries and dependencies
3. Code Implementation (with Pre-flight Checklist)
4. Run the verification pipeline
5. Documentation update + commit

Must include:
- **Verification commands** (referencing actual commands configured in Phase 2)
- **Commit conventions**
- **Common pitfalls** (project-specific caveats)

### 3.4 Create TESTING.md

Testing conventions, template at [templates/testing-template.md](templates/testing-template.md).

Must include:
- Test file naming and directory rules
- **Copy-pasteable test templates** (Store tests, utility tests, component tests, E2E tests)
- Coverage requirements
- New feature test checklist

---

## Phase 4: Write Seed Tests

**Goal**: Write runnable seed tests for each verification layer to prove the pipeline works.

### Seed Test Strategy

| Layer | Number of Tests | Coverage Target |
|-------|----------------|-----------------|
| Unit Tests | 1 file per Store/utility module | Initial state + all Actions |
| E2E Tests | 1–2 files per app entry | Page load + core navigation |

### Writing Principles

1. **Read source code before writing tests**; never fabricate non-existent interfaces
2. Use the project's actual path aliases
3. E2E tests use text content/role-based locators; avoid brittle CSS selectors
4. Each test is independent; no reliance on execution order

---

## Phase 5: End-to-End Verification

**Goal**: Ensure the entire system is operational.

### Verification Checklist

```
Verification Pipeline:
- [ ] Lint tool runs successfully
- [ ] All unit tests pass
- [ ] All E2E tests pass

Documentation Completeness:
- [ ] AGENTS.md contains module index table
- [ ] WORKFLOW.md contains 5 Phases
- [ ] TESTING.md contains copy-pasteable templates
- [ ] All module docs exist and match the code

Configuration Files:
- [ ] Unit test config is correct
- [ ] E2E test config is correct
- [ ] Lint config is correct
- [ ] All scripts are in place
```

---

## Parallel Execution Recommendations

To maximize efficiency, the following tasks can run in parallel:

```
Phase 2 (Infrastructure) ──┬──→ Phase 4 (Seed Tests, depends on Phase 2)
                            │
Phase 3.1 (AGENTS.md) ──────── Can run in parallel with Phase 2
Phase 3.2 (Module Docs) ────── Can run in parallel with Phase 2
Phase 3.3 (WORKFLOW.md) ────── Depends on Phase 2 (references commands)
Phase 3.4 (TESTING.md) ─────── Depends on Phase 2 (references templates)
                            │
                            └──→ Phase 5 (Verification, after all complete)
```

## Key Constraints

- **Do not modify existing business code**: Only add configuration, documentation, and tests
- **Documentation must be based on actual code**: Always read source code before writing docs; never fabricate
- **Lint in lenient mode**: Do not let existing code issues block the toolchain
- **Modification boundaries in module docs are the core value**: This is the key mechanism to prevent Agents from making out-of-scope changes
- **Language adaptation**: Generate all documentation in the language of the user's request; default to English if the language is unclear
