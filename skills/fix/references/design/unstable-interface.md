# Unstable Interface

**What:** Module changes frequently AND is used by many other modules.

**Why it's bad:**
- Frequent regressions in dependents
- High maintenance cost
- Hard to stabilize architecture

## Fix Strategies

### Strategy 1: Extract Stable Interface

```typescript
// BEFORE: api-client.ts changes frequently, used by 20 files

// AFTER: Stable interface
// interfaces/api.ts (stable - rarely changes)
export interface ApiClient {
  get<T>(url: string): Promise<T>;
  post<T>(url: string, data: unknown): Promise<T>;
}

// api-client.ts (implementation - can change freely)
export class HttpApiClient implements ApiClient { }

// Consumers depend on interface, not implementation
```

### Strategy 2: Version the API

```typescript
// For internal APIs
// api/v1/users.ts - stable, legacy
// api/v2/users.ts - evolving
```
