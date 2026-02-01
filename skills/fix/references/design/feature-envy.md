# Feature Envy

**What:** Module uses more symbols from another module than its own.

**Why it's bad:**
- Violation of encapsulation
- Tight coupling
- Logic is in the wrong place

## Fix Strategy: Move Code to Envied Module

```typescript
// BEFORE: order.ts "envies" user.ts
// order.ts
import { user } from './user';

function calculateDiscount(order: Order) {
  // Uses user properties extensively
  if (user.membershipLevel === 'gold') return 0.2;
  if (user.purchaseHistory.length > 10) return 0.1;
  if (user.referralCount > 5) return 0.05;
  return 0;
}

// AFTER: Move to user.ts where data lives
// user.ts
export function calculateUserDiscount(user: User): number {
  if (user.membershipLevel === 'gold') return 0.2;
  if (user.purchaseHistory.length > 10) return 0.1;
  if (user.referralCount > 5) return 0.05;
  return 0;
}

// order.ts
import { calculateUserDiscount } from './user';

function calculateDiscount(order: Order, user: User) {
  return calculateUserDiscount(user);
}
```
