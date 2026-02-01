# Shotgun Surgery

**What:** One logical change requires modifications in many files.

**Why it's bad:**
- High maintenance effort
- Easy to miss updating one file
- Knowledge fragmented across codebase

## Fix Strategy: Consolidate Related Logic

```typescript
// BEFORE: Price formatting scattered everywhere
// cart.ts
const formatted = `$${price.toFixed(2)}`;
// invoice.ts
const formatted = `$${price.toFixed(2)}`;
// receipt.ts
const formatted = `$${price.toFixed(2)}`;

// AFTER: Centralize
// formatters/currency.ts
export function formatPrice(price: number): string {
  return `$${price.toFixed(2)}`;
}

// All files import from single source
import { formatPrice } from '@/formatters/currency';
```
