# Shared Mutable State

**What:** Exported `let`/`var` that can be modified from multiple modules.

**Why it's bad:**
- Unpredictable side effects
- Race conditions
- Hard to debug state changes

## Fix Strategies

### Strategy 1: Encapsulate in Class/Closure

```typescript
// BEFORE
export let currentUser: User | null = null;

// AFTER: Encapsulate
class UserStore {
  private user: User | null = null;

  getUser() { return this.user; }
  setUser(user: User) { this.user = user; }
}

export const userStore = new UserStore();
```

### Strategy 2: Immutable State Management

```typescript
// Use state management library (Redux, Zustand, etc.)
import { create } from 'zustand';

export const useUserStore = create((set) => ({
  user: null,
  setUser: (user) => set({ user }),
}));
```
