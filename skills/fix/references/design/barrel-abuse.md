# Barrel File Abuse

**What:** Index file with excessive re-exports.

**Why it's bad:**
- Increased build times (tree-shaking issues)
- Circular dependency risk
- Imports more than needed

## Fix Strategy: Direct Imports or Split

```typescript
// BEFORE: index.ts with 100 exports
export * from './a';
export * from './b';
// ... 98 more

// AFTER Option 1: Import directly
import { specificFunc } from './utils/string-helpers';

// AFTER Option 2: Split by domain
// utils/string/index.ts
export * from './format';
export * from './validate';

// utils/number/index.ts
export * from './math';
export * from './currency';
```
