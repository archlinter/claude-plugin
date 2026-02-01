# Circular Type Dependencies

**What:** Modules have circular imports involving only types (type-only imports).

**Why it's bad:**
- Indicates design flaw even if compiler allows it
- Tight coupling between type definitions

## Fix Strategy: Extract Types to Shared Module

```typescript
// BEFORE
// user.ts
import type { Order } from './order';
export interface User { orders: Order[]; }

// order.ts
import type { User } from './user';
export interface Order { customer: User; }

// AFTER
// types/domain.ts
export interface User { orders: Order[]; }
export interface Order { customer: User; }

// user.ts
import type { User } from './types/domain';

// order.ts
import type { Order } from './types/domain';
```
