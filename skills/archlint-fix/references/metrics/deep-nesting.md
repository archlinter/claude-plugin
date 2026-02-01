# Deep Nesting

**What:** Too many levels of nested control structures.

**Why it's bad:**
- Hard to follow logic
- Difficult to test all branches
- High cognitive load

## Fix Strategies

### Strategy 1: Flatten with Early Returns

```typescript
// BEFORE: 5 levels deep
function process(data) {
  if (data) {
    if (data.valid) {
      if (data.items) {
        for (const item of data.items) {
          if (item.active) {
            // finally, the logic
          }
        }
      }
    }
  }
}

// AFTER: Flat
function process(data) {
  if (!data?.valid) return;
  if (!data.items) return;

  const activeItems = data.items.filter(item => item.active);
  activeItems.forEach(processItem);
}
```

### Strategy 2: Extract Nested Blocks

```typescript
// BEFORE
for (const order of orders) {
  for (const item of order.items) {
    for (const variant of item.variants) {
      if (variant.inStock) {
        // deep logic
      }
    }
  }
}

// AFTER
function getInStockVariants(orders: Order[]): Variant[] {
  return orders
    .flatMap(order => order.items)
    .flatMap(item => item.variants)
    .filter(variant => variant.inStock);
}

for (const variant of getInStockVariants(orders)) {
  // flat logic
}
```
