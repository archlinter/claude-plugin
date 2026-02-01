# Cyclic Dependency

**What:** Files form a dependency cycle: A → B → C → A.

**Why it's bad:**
- Difficult to reason about initialization order
- Changes cascade unpredictably
- Testing becomes difficult due to interdependencies
- May cause runtime initialization issues

## Fix Strategies

### Strategy 1: Extract Shared Code

```typescript
// BEFORE: A imports B, B imports A
// a.ts
import { helperFromB } from './b';
export const helperForB = () => { /* ... */ };

// b.ts
import { helperForB } from './a';
export const helperFromB = () => { /* ... */ };

// AFTER: Extract to shared module
// shared.ts
export const helperForB = () => { /* ... */ };
export const helperFromB = () => { /* ... */ };

// a.ts
import { helperFromB } from './shared';

// b.ts
import { helperForB } from './shared';
```

### Strategy 2: Dependency Injection

```typescript
// BEFORE: UserService needs AuthService, AuthService needs UserService
// user.service.ts
import { AuthService } from './auth.service';
export class UserService {
  constructor(private auth: AuthService) {}
}

// auth.service.ts
import { UserService } from './user.service'; // CYCLE!
export class AuthService {
  constructor(private users: UserService) {}
}

// AFTER: Inject via interface
// interfaces/user-provider.ts
export interface IUserProvider {
  getUser(id: string): User;
}

// auth.service.ts
import { IUserProvider } from './interfaces/user-provider';
export class AuthService {
  constructor(private users: IUserProvider) {} // No cycle!
}
```

### Strategy 3: Event-Driven Decoupling

```typescript
// BEFORE: Direct circular calls
// AFTER: Use events
// events.ts
export const eventBus = new EventEmitter();

// a.ts
import { eventBus } from './events';
eventBus.emit('user-created', user);

// b.ts
import { eventBus } from './events';
eventBus.on('user-created', (user) => { /* react */ });
```
