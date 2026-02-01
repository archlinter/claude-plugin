# Vendor Coupling

**What:** Third-party package used directly in many files without abstraction.

**Why it's bad:**
- Vendor lock-in
- Upgrade/replacement requires touching many files

## Fix Strategy: Abstraction Layer

```typescript
// BEFORE: axios used in 30 files
import axios from 'axios';
axios.get('/api/users');

// AFTER: HTTP client abstraction
// lib/http.ts
import axios from 'axios';

export const http = {
  get: <T>(url: string) => axios.get<T>(url).then(r => r.data),
  post: <T>(url: string, data: unknown) => axios.post<T>(url, data).then(r => r.data),
};

// consumers
import { http } from '@/lib/http';
http.get('/api/users'); // Can swap axios for fetch without changes
```
