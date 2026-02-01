# Test Leakage

**What:** Production module imports test file, mock, or test utility.

**Why it's bad:**
- Test code in production bundle
- Increased bundle size
- Potential security risk

## Fix Strategies

### Strategy 1: Move Shared Code Out of Tests

```typescript
// BEFORE
// src/service.ts
import { mockData } from '../tests/mocks'; // LEAKAGE!

// AFTER
// src/fixtures/sample-data.ts (not in tests folder)
export const sampleData = { /* ... */ };

// src/service.ts
import { sampleData } from './fixtures/sample-data';

// tests/service.test.ts
import { sampleData } from '../src/fixtures/sample-data';
```

### Strategy 2: Conditional Import (Development Only)

```typescript
// If mock is needed for dev mode only
let mockServer;
if (process.env.NODE_ENV === 'development') {
  mockServer = require('./mocks/server').mockServer;
}
```

### Strategy 3: Check Import Path

```typescript
// BEFORE: Accidental test import
import { helper } from '../__tests__/helpers'; // Wrong!

// AFTER: Import from correct location
import { helper } from './utils/helpers';
```
