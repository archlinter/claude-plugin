# Abstractness Violation

**What:** Module falls in "Zone of Pain" (too stable + concrete) or "Zone of Uselessness" (too abstract + unstable).

**Why it's bad:**
- Zone of Pain: Rigid code hard to change
- Zone of Uselessness: Unused abstractions adding complexity

## Fix Strategies

### Fix for Zone of Pain (stable but concrete)

```typescript
// BEFORE: Heavily used concrete class
export class DatabaseConnection {
  query(sql: string) { /* postgres-specific */ }
}

// AFTER: Add abstraction
export interface Database {
  query(sql: string): Promise<Result>;
}

export class PostgresDatabase implements Database { }
```

### Fix for Zone of Uselessness (abstract but unused)

```typescript
// BEFORE: Over-abstracted, rarely used
export interface IFactory<T> { }
export interface IBuilder<T> { }
export interface IStrategy<T> { }

// AFTER: Remove unused abstractions or make concrete
// Delete if not needed, or implement concretely
```
