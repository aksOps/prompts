---
description: 'Web Stack (TS/React) Guidelines, UI/UX Standards, and State Management.'
applyTo: '**.ts, **.tsx, **.js, **.jsx, **.json, **.css, **.scss'
---

# Web Stack (TS/React) Guidelines & Standards

## Your Mission
Enforce strict TypeScript usage, modern React patterns, and a "User First" mentality. You build accessible, performant, and maintainable web applications.

## Introduction
The web ecosystem moves fast. We stick to stable, modern standards: TypeScript for safety, React (Hooks) for UI, and Redux Toolkit for state. We prioritize Accessibility (a11y) and Performance as first-class citizens.

## Web Fundamentals & Architecture

### TypeScript Standards
- **Strict Mode:** Always enabled. `any` is forbidden; use `unknown` if necessary.
- **Interfaces:** Use `interface` for object definitions (better error messages) and `type` for unions/intersections.

### React (Frontend)
- **Functional only:** Class components are deprecated.
- **Hooks:** Use built-in hooks (`useState`, `useEffect`) and custom hooks for logic reuse.
- **Composition:** Break UIs into small, focused components.

### State Management
- **Redux Toolkit (RTK):** Preferred for global state.
- **Context:** Use only for static global data (Themes, User Config) or simple DI.
- **Direct Redux:** ❌ Avoid old-school Redux boilerplate (switch/case reducers).

## Safety & Security

### Frontend Security
- **XSS Prevention:** React handles most content escaping, but be careful with `dangerouslySetInnerHTML`.
- **Input Validation:** Use schema validation (e.g., `zod`) for form inputs.

### Accessibility (a11y)
- **Semantics:** Use semantic HTML (`<button>`, `<nav>`, `<main>`) over `div` soup.
- **ARIA:** Use ARIA attributes only when semantic HTML isn't enough.
- **Keyboard Nav:** Ensure all interactive elements are focusable and usable via keyboard.

## Testing & Quality Assurance

### Frameworks
- **Unit:** Vitest (faster than Jest).
- **Component:** React Testing Library (RTL).
- **E2E:** Playwright (Full Windows support).

### Philosophies
- **Test Behavior:** Test how the user interacts with the component, not implementation details.
- **Queries:** Prefer `getByRole`, `getByLabelText`, `getByText` over CSS selectors.

**Example - Component Test:**
```tsx
import { render, screen, fireEvent } from '@testing-library/react';
import { Button } from './Button';

test('calls onClick when clicked', () => {
  const handleClick = vi.fn();
  render(<Button onClick={handleClick}>Click Me</Button>);
  
  fireEvent.click(screen.getByRole('button', { name: /click me/i }));
  expect(handleClick).toHaveBeenCalledTimes(1);
});
```

## Anti-patterns
- **Prop Drilling:** Passing props down 5 levels. Use Composition or Context/Redux.
- **Huge Components:** Components > 200 lines should likely be split.
- **`useEffect` Dependencies:** Lying about dependency arrays. If linter warns, fix the logic, don't suppress the warning.
