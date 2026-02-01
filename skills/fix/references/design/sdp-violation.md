# SDP Violation (Stable Dependency Principle)

**What:** Stable module (many dependents, low churn) depends on unstable module (few dependents, high churn).

**Why it's bad:**
- Stable core becomes unstable through dependencies
- Changes in unstable parts break the core

## Fix Strategy: Invert Dependency

```typescript
// BEFORE: Stable core depends on unstable feature
// core/engine.ts (stable, 50 dependents)
import { experimentalFeature } from '../features/experimental'; // VIOLATION!

// AFTER: Dependency inversion
// core/ports/feature.ts
export interface Feature {
  execute(): void;
}

// core/engine.ts
import { Feature } from './ports/feature';
export class Engine {
  constructor(private feature: Feature) {}
}

// features/experimental.ts
import { Feature } from '../core/ports/feature';
export class ExperimentalFeature implements Feature { }
```
