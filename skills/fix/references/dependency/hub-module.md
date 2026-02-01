# Hub Module

**What:** Internal module acts as pass-through hub with many connections but no logic.

**Why it's bad:**
- Unnecessary abstraction layer
- Fragile bridge that can break many consumers

## Fix Strategy: Direct Imports or Consolidate

```typescript
// BEFORE: hub.ts re-exports everything
// hub.ts
export * from './a';
export * from './b';
export * from './c';

// consumer.ts
import { funcA, funcB } from './hub';

// AFTER Option 1: Import directly
// consumer.ts
import { funcA } from './a';
import { funcB } from './b';

// AFTER Option 2: Consolidate if hub has purpose
// hub.ts - add actual logic/orchestration
export function orchestrate() {
  const a = funcA();
  const b = funcB(a);
  return b;
}
```
