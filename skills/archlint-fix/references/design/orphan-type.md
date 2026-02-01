# Orphan Type

**What:** Exported type/interface never imported anywhere.

**Why it's bad:**
- Maintenance cost
- Confusion about public API

## Fix Strategy: Remove or Make Internal

```typescript
// BEFORE
export interface UnusedConfig { } // Never imported

// AFTER Option 1: Remove
// (delete the interface)

// AFTER Option 2: Make internal if used locally
interface InternalConfig { } // No export
```
