# TESTING.md Template

The following template is suitable for testing guidelines in any frontend project. Adapt it to your specific testing framework.

---

```markdown
# Testing Guidelines

## Overview

The project adopts a three-layer progressive verification pipeline:

| Layer | Tools | Speed | Coverage |
|-------|-------|-------|----------|
| Layer 1 | TypeScript + ESLint | Seconds | Type safety + Code standards |
| Layer 2 | [Unit Testing Framework] | Seconds to minutes | Store logic, utility functions, component rendering |
| Layer 3 | [E2E Testing Framework] | Minutes | Critical user flows |

## Test File Organization

### Naming Conventions

- Unit tests: `*.test.ts` or `*.test.tsx`
- E2E tests: `*.spec.ts`

### Directory Structure

```
src/
├── [module]/__tests__/        # Module-level unit tests
└── shared/lib/__tests__/      # Shared utility tests
e2e/
├── [app-name]/                # E2E tests organized by application
└── ...
```

## Unit Testing Guidelines

### Store Test Template

```typescript
import { describe, it, expect, beforeEach } from '[test-framework]'
import { useXxxStore } from '@/path/to/store'

describe('useXxxStore', () => {
  beforeEach(() => {
    // Reset store state (Zustand example)
    useXxxStore.setState(useXxxStore.getInitialState())
  })

  it('has correct initial state', () => {
    const state = useXxxStore.getState()
    expect(state.someField).toBe(expectedValue)
  })

  it('action works correctly', () => {
    useXxxStore.getState().someAction(params)
    expect(useXxxStore.getState().someField).toBe(newValue)
  })
})
```

### Utility Function Test Template

```typescript
import { describe, it, expect } from '[test-framework]'
import { utilFunction } from '@/path/to/utils'

describe('utilFunction', () => {
  it('handles normal input', () => {
    expect(utilFunction(normalInput)).toBe(expectedOutput)
  })

  it('handles edge cases', () => {
    expect(utilFunction(edgeInput)).toBe(edgeOutput)
  })

  it('handles empty/null input', () => {
    expect(utilFunction(null)).toBe(fallbackValue)
  })
})
```

### Component Test Template

```typescript
import { describe, it, expect } from '[test-framework]'
import { render, screen, fireEvent } from '@testing-library/react'
import { ComponentName } from '@/path/to/component'

describe('ComponentName', () => {
  it('renders correctly', () => {
    render(<ComponentName requiredProp="value" />)
    expect(screen.getByText('Expected Text')).toBeInTheDocument()
  })

  it('handles user interaction', async () => {
    render(<ComponentName />)
    await fireEvent.click(screen.getByRole('button'))
    expect(screen.getByText('After Click')).toBeInTheDocument()
  })
})
```

### Test Writing Principles

- Each test must be independent (no reliance on execution order)
- Use `describe` to group tests by functionality
- Cover: happy path + edge cases + error handling
- Mock external dependencies, never mock the module under test

## E2E Testing Guidelines

### Element Locator Strategy (Priority)

1. `data-testid` attribute
2. Text content via `getByText`
3. ARIA role via `getByRole`
4. Avoid using CSS class names or DOM structure

### E2E Test Template

```typescript
import { test, expect } from '@playwright/test'

test.describe('Feature Name', () => {
  test.beforeEach(async ({ page }) => {
    await page.goto('/')
  })

  test('core user flow works', async ({ page }) => {
    // Verify page load
    await expect(page.getByText('Expected Element')).toBeVisible()

    // Perform user action
    await page.click('button:has-text("Action")')

    // Verify result
    await expect(page.getByText('Result')).toBeVisible()
  })
})
```

### Assertion Guidelines

- Use the testing framework's built-in assertions
- Avoid hardcoded wait times; use auto-waiting mechanisms instead
- Assert on user-visible behavior, not internal implementation details

## Coverage Requirements

### MVP Phase

- Store logic: 100% action coverage
- Utility functions: 100% coverage
- Components: Coverage of critical interaction paths
- E2E: Coverage of core user flows

## New Feature Testing Checklist

- [ ] New Store action → Add Store unit tests
- [ ] New utility function → Add utility function unit tests
- [ ] New component → Consider component rendering tests
- [ ] New page/flow → Add E2E tests
- [ ] Modified public interface → Update existing tests
- [ ] Bug fix → Add regression tests
```
