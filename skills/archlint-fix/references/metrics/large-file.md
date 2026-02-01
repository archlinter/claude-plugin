# Large File

**What:** File with too many lines of code.

**Why it's bad:**
- Hard to navigate
- Higher merge conflict risk
- Slower code reviews
- Often indicates mixed responsibilities

## Fix Strategies

### Strategy 1: Split by Domain/Feature

```typescript
// BEFORE: user-service.ts (800 lines)
// - User CRUD
// - Authentication
// - Authorization
// - Profile management
// - Notifications

// AFTER: Split by domain
// users/crud.service.ts
// users/auth.service.ts
// users/authz.service.ts
// users/profile.service.ts
// users/notification.service.ts
```

### Strategy 2: Extract Constants and Types

```typescript
// BEFORE: Everything in one file
const STATUS = { ... };
const CONFIG = { ... };
interface User { ... }
interface Order { ... }
function process() { ... }

// AFTER
// constants.ts
export const STATUS = { ... };
export const CONFIG = { ... };

// types.ts
export interface User { ... }
export interface Order { ... }

// service.ts
import { STATUS, CONFIG } from './constants';
import { User, Order } from './types';
```

### Strategy 3: Use Barrel File for Organization

```typescript
// After splitting, create index for clean imports
// users/index.ts
export { UserCrudService } from './crud.service';
export { UserAuthService } from './auth.service';
export type { User, UserProfile } from './types';
```
