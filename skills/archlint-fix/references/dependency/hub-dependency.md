# Hub Dependency

**What:** An external package is used by many files (e.g., lodash in 50+ files).

**Why it's bad:**
- Vendor lock-in
- Difficult to upgrade or replace
- High impact if package is deprecated

## Fix Strategy: Create Wrapper

```typescript
// BEFORE: Direct lodash usage everywhere
// file1.ts
import { debounce } from 'lodash';
// file2.ts
import { throttle } from 'lodash';

// AFTER: Wrapper module
// lib/timing.ts
import { debounce as _debounce, throttle as _throttle } from 'lodash';

export const debounce = _debounce;
export const throttle = _throttle;
// Can swap implementation without touching consumers

// file1.ts
import { debounce } from '@/lib/timing';
```
