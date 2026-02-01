# Cognitive Complexity

**What:** Function is hard to understand due to nested logic, breaks in linear flow, recursion.

**Why it's bad:**
- Mental overload for readers
- High bug probability during maintenance
- Hard to test all paths

## Fix Strategies

### Strategy 1: Early Returns (Guard Clauses)

```typescript
// BEFORE: Nested structure
function processUser(user: User) {
  if (user) {
    if (user.isActive) {
      if (user.hasPermission) {
        return doSomething(user);
      }
    }
  }
  return null;
}

// AFTER: Flat with guards
function processUser(user: User) {
  if (!user) return null;
  if (!user.isActive) return null;
  if (!user.hasPermission) return null;

  return doSomething(user);
}
```

### Strategy 2: Decompose Boolean Expressions

```typescript
// BEFORE
if (user.age >= 18 && user.country === 'US' && !user.isBanned && user.emailVerified) {
  // ...
}

// AFTER
const isAdult = user.age >= 18;
const isUSResident = user.country === 'US';
const isInGoodStanding = !user.isBanned && user.emailVerified;
const canPurchase = isAdult && isUSResident && isInGoodStanding;

if (canPurchase) {
  // ...
}
```

### Strategy 3: Replace Loops with Declarative Methods

```typescript
// BEFORE
let total = 0;
for (const item of items) {
  if (item.category === 'electronics') {
    if (item.price > 100) {
      total += item.price * 0.9;
    } else {
      total += item.price;
    }
  }
}

// AFTER
const total = items
  .filter(item => item.category === 'electronics')
  .reduce((sum, item) => sum + (item.price > 100 ? item.price * 0.9 : item.price), 0);
```
