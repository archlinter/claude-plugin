# High Coupling (CBO)

**What:** Module has too many incoming + outgoing dependencies (Coupling Between Objects > threshold).

**Why it's bad:**
- Changes ripple through many modules
- Difficult to mock for testing

## Fix Strategies

### Strategy 1: Facade Pattern

```typescript
// BEFORE: Module imports 15 different utilities
import { a } from './utils/a';
import { b } from './utils/b';
// ... 13 more imports

// AFTER: Create facade
// utils/index.ts (facade)
export { a } from './a';
export { b } from './b';

// consumer.ts
import { a, b } from './utils'; // Single import
```

### Strategy 2: Split by Responsibility

```typescript
// BEFORE: One file does formatting, validation, API calls
// helpers.ts - CBO: 25

// AFTER: Split into focused modules
// formatters.ts - CBO: 5
// validators.ts - CBO: 4
// api-client.ts - CBO: 6
```
