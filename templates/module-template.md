# Module Documentation Template

The following template is suitable for module-level documentation in any project. Adapt the content to your specific project.

---

```markdown
# [Module Name]

## Responsibilities

[One-sentence description of what this module does]

## File Inventory

| File | Purpose |
|------|---------|
| `FileName.tsx` | [What it does] |
| `AnotherFile.ts` | [What it does] |

## Public Interface

### Exports

- `ComponentA` — [Purpose]
- `useStoreX` — [Purpose]
- `TypeY` — [Purpose]

### Dependencies

| Dependency | Source | Purpose |
|------------|--------|---------|
| `useXxxStore` | `@/xxx/stores/xxx-store` | [What state it provides] |
| `TypeName` | `@/shared/types` | [Type definition] |
| `cn` | `@/shared/lib/utils` | Merge Tailwind class names |

## Modification Boundaries

### Allowed

- Internal implementation of components within the module (JSX, styles, local state)
- Adding new internal sub-components
- Modifying component Props interfaces (must update all consumers accordingly)
- [Add more based on the specific module]

### Prohibited

- Do not directly modify existing type signatures in shared type definition files (unless the requirement explicitly calls for extension)
- Do not directly manipulate another module's Store from within this module
- Do not introduce third-party dependencies unrelated to this module's responsibilities
- [Add more based on the specific module]

### Cascading Updates

When modifying this module, the following must be checked/updated accordingly:

| Trigger | Files to Update |
|---------|-----------------|
| Modifying exported component Props | All parent components consuming that component |
| Modifying Store action signatures | All components consuming that Store |
| Adding a new file | The "File Inventory" section of this document |

## Data Flow

```
[Describe how data flows in and out]

Example:
User Action → Store Action → State Update → Component Re-render

External Data Sources:
  Mock Data / API → Store → Component Props → Render
```

## Testing Requirements

### Existing Tests

| Test File | Coverage |
|-----------|----------|
| `path/to/__tests__/xxx.test.ts` | [What it covers] |

### Testing Requirements for New Features

- New Store Action → Add Store unit tests
- New utility function → Add utility function unit tests
- New user interaction flow → Add E2E tests
- Bug fix → Add regression tests
```
