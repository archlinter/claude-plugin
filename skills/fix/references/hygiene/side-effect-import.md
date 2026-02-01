# Side-Effect Import

**What:** Import that executes code on load without binding symbols.

```typescript
import './polyfills';      // Side effect
import './global-styles';  // Side effect
import 'reflect-metadata'; // Side effect
```

**Why it's bad:**
- Unpredictable execution order
- Global state pollution
- Hard to test

## Fix Strategies

### Strategy 1: Make Explicit

```typescript
// BEFORE
import './init';

// AFTER: Export and call explicitly
// init.ts
export function initialize() {
  // setup code
}

// main.ts
import { initialize } from './init';
initialize();
```

### Strategy 2: Use Named Imports

```typescript
// BEFORE
import './register-handlers';

// AFTER
// register-handlers.ts
export function registerHandlers() {
  window.addEventListener('error', handleError);
}

// main.ts
import { registerHandlers } from './register-handlers';
registerHandlers();
```

### Strategy 3: Isolate to Entry Point

```typescript
// Keep side-effect imports only in entry point
// main.ts (entry point)
import './polyfills';
import './global-styles';
import { App } from './app';

// All other files: no side-effect imports
```
