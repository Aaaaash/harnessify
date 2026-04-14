# WORKFLOW.md Template

The following template is suitable for Agent development workflows in any project. Adapt the commands and conventions to your specific project.

---

```markdown
# Agent Development Workflow

## Overview

This document defines the standard process that Agents must follow when making code changes, ensuring every change has clear boundaries, is properly verified, and produces traceable deliverables.

## Process Overview

```
Requirement Analysis → Module Location → Documentation Review → Implementation → Verification Pipeline → Documentation Update → Commit
```

## Phase 1: Requirement Analysis & Module Location

### 1.1 Read AGENTS.md
- Skim the project overview and tech stack
- Locate the relevant modules via the **Module Index Table**
- Find the corresponding `docs/modules/*.md` links

### 1.2 Read Module Documentation
- **Must be completed before modifying any code**
- Focus on: responsibilities, modification boundaries (allowed/prohibited), public interfaces, cascading updates
- If the requirement spans multiple modules, **read all related module documentation**

### 1.3 Confirm Scope of Changes
- List the files to be modified
- Confirm no violations of the "Prohibited" rules in module documentation
- Identify any cascading updates required

## Phase 2: Implementation

### Pre-flight Checklist

Confirm before writing any code:
- [ ] Have read AGENTS.md and relevant module documentation
- [ ] Modification boundaries are clear; changes will not exceed module responsibilities
- [ ] Understand the relevant Store's state structure and type definitions

### Implementation Standards
- [Fill in based on project conventions: import path rules, naming conventions, styling guidelines, etc.]

### Cross-Module Modification Rules
- Modifying shared code: Must verify the impact on all consumers
- Modifying type definitions: Must check all usages of that type
- Modifying Store interfaces: Must check all components consuming that Store

## Phase 3: Verification Pipeline

### Verification Commands (Execute in Order)

```bash
# Layer 1: Static analysis
[typecheck command]
[lint command]

# Layer 2: Unit tests
[unit test command]

# Layer 3: E2E tests
[e2e test command]
```

### Quick Verification (During Development)
```bash
# Run only tests related to your changes
[test command for specific files]
```

### Pass Criteria
- typecheck: 0 errors (excluding known issues)
- lint: 0 errors (warnings are acceptable)
- Tests: All passing

### Failure Handling
- Newly introduced errors: Must be fixed before committing
- Pre-existing errors: Document in the commit message; do not block the commit

## Phase 4: Documentation Update

### Must Update When
- New files added → Update the "File Inventory" in the corresponding module documentation
- Public interface modified → Update "Public Interface"
- New inter-module dependency added → Update "Dependencies" and "Cascading Updates"
- New test files added → Update "Testing Requirements"

### No Update Needed When
- Internal component implementation changes (no interface impact)
- Bug fixes (no change to the behavioral contract)
- Minor style adjustments

## Phase 5: Commit

### Commit Message Convention

[Fill in based on project conventions, example:]

```
<type>: <brief description>

<detailed explanation (optional)>
```

## Common Pitfalls

[Fill in based on project-specific architectural concerns, for example:]
- Isolation rules for multi-entry applications
- Differences in routing strategies
- Style isolation rules
- Mock data management rules
```
