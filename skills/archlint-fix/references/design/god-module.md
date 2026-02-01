# God Module

**What:** Module with high fan-in (many dependents), high fan-out (many dependencies), and high churn (frequent changes).

**Why it's bad:**
- Single point of failure
- Hard to understand and maintain
- High merge conflict risk
- Difficult to test in isolation

## Fix Strategies

### Strategy 1: Split by Domain

```typescript
// BEFORE: utils.ts (god module)
export function formatDate() { }
export function formatCurrency() { }
export function validateEmail() { }
export function validatePhone() { }
export function fetchUser() { }
export function fetchOrders() { }

// AFTER: Split by domain
// utils/formatters.ts
export function formatDate() { }
export function formatCurrency() { }

// utils/validators.ts
export function validateEmail() { }
export function validatePhone() { }

// services/user.service.ts
export function fetchUser() { }

// services/order.service.ts
export function fetchOrders() { }
```

### Strategy 2: Facade Pattern (if integration point)

```typescript
// If god module is intentional integration point
// Keep facade, move implementation
// api/index.ts (thin facade)
export { fetchUser } from './user.api';
export { fetchOrders } from './order.api';
export { fetchProducts } from './product.api';
```
