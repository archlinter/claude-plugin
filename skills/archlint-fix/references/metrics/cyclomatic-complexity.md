# Cyclomatic Complexity

**What:** Function has too many decision points (if, for, while, switch cases, etc.).

**Why it's bad:**
- Higher probability of bugs
- Difficult to test all paths
- Hard to understand

## Fix Strategies

### Strategy 1: Extract Helper Functions

```typescript
// BEFORE: Complexity 15
function processOrder(order: Order) {
  if (order.status === 'pending') {
    if (order.items.length > 0) {
      for (const item of order.items) {
        if (item.inStock) {
          // process
        } else {
          // handle out of stock
        }
      }
    }
  } else if (order.status === 'shipped') {
    // ...
  }
}

// AFTER: Split into focused functions
function processOrder(order: Order) {
  if (order.status === 'pending') {
    return processPendingOrder(order);
  }
  if (order.status === 'shipped') {
    return processShippedOrder(order);
  }
}

function processPendingOrder(order: Order) {
  if (order.items.length === 0) return;
  order.items.forEach(processOrderItem);
}

function processOrderItem(item: Item) {
  if (item.inStock) {
    reserveItem(item);
  } else {
    handleOutOfStock(item);
  }
}
```

### Strategy 2: Replace Conditionals with Polymorphism

```typescript
// BEFORE
function calculatePrice(product: Product) {
  if (product.type === 'book') return product.price * 0.9;
  if (product.type === 'electronics') return product.price * 1.1;
  if (product.type === 'food') return product.price;
}

// AFTER: Strategy pattern
const strategies: Record<string, (p: number) => number> = {
  book: (p) => p * 0.9,
  electronics: (p) => p * 1.1,
  food: (p) => p,
};

function calculatePrice(product: Product) {
  return strategies[product.type](product.price);
}
```

### Strategy 3: Use Lookup Tables

```typescript
// BEFORE
function getStatusColor(status: string) {
  if (status === 'success') return 'green';
  if (status === 'warning') return 'yellow';
  if (status === 'error') return 'red';
  return 'gray';
}

// AFTER
const STATUS_COLORS: Record<string, string> = {
  success: 'green',
  warning: 'yellow',
  error: 'red',
};

function getStatusColor(status: string) {
  return STATUS_COLORS[status] ?? 'gray';
}
```
