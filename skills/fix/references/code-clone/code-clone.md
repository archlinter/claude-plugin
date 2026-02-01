# Code Clone

**What:** Identical or near-identical code blocks in multiple locations.

**Why it's bad:**
- Maintenance burden: changes must be made in multiple places
- Risk of inconsistent behavior
- Increases codebase size without adding functionality

## Fix Strategies

### Strategy 1: Extract to Shared Function

```typescript
// BEFORE: Duplicated validation in 3 files
// user-form.ts
if (!email.includes('@') || email.length < 5) {
  throw new Error('Invalid email');
}

// signup.ts
if (!email.includes('@') || email.length < 5) {
  throw new Error('Invalid email');
}

// AFTER: Single source of truth
// validators/email.ts
export function validateEmail(email: string): void {
  if (!email.includes('@') || email.length < 5) {
    throw new Error('Invalid email');
  }
}

// All files
import { validateEmail } from '@/validators/email';
validateEmail(email);
```

### Strategy 2: Parameterize Differences

```typescript
// BEFORE: Similar but not identical
async function fetchUser(id: string) {
  const res = await fetch(`/api/users/${id}`);
  if (!res.ok) throw new Error('Failed to fetch user');
  return res.json();
}

async function fetchOrder(id: string) {
  const res = await fetch(`/api/orders/${id}`);
  if (!res.ok) throw new Error('Failed to fetch order');
  return res.json();
}

// AFTER: Generic function with parameter
export async function fetchResource<T>(resource: string, id: string): Promise<T> {
  const res = await fetch(`/api/${resource}/${id}`);
  if (!res.ok) throw new Error(`Failed to fetch ${resource}`);
  return res.json();
}

const user = await fetchResource<User>('users', userId);
const order = await fetchResource<Order>('orders', orderId);
```

### Strategy 3: Higher-Order Functions

```typescript
// BEFORE: Repeated try-catch patterns
async function getUser(id: string) {
  try {
    const user = await api.getUser(id);
    return { data: user, error: null };
  } catch (e) {
    logger.error(e);
    return { data: null, error: e };
  }
}

// AFTER: Higher-order function
function withErrorHandling<T>(fn: () => Promise<T>) {
  return async (): Promise<{ data: T | null; error: Error | null }> => {
    try {
      const data = await fn();
      return { data, error: null };
    } catch (e) {
      logger.error(e);
      return { data: null, error: e as Error };
    }
  };
}

const getUser = withErrorHandling((id: string) => api.getUser(id));
```

### When to Accept Duplication

Sometimes 2-3 copies are acceptable if:
- Differences are significant
- Abstraction would be complex
- Code is likely to diverge

```typescript
// Document intentional duplication
// Intentionally duplicated: different validation rules per context
// See: https://wiki.company.com/validation-strategy
```
