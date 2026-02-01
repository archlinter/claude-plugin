# Scattered Configuration

**What:** Config access (env vars, settings) spread across many files.

**Why it's bad:**
- Hard to audit configuration usage
- Inconsistent defaults
- Difficult to mock in tests

## Fix Strategy: Centralize Configuration

```typescript
// BEFORE: Scattered env access
// file1.ts
const apiUrl = process.env.API_URL;
// file2.ts
const apiUrl = process.env.API_URL || 'http://localhost';
// file3.ts
const apiUrl = process.env.API_URL ?? 'default';

// AFTER: Central config
// config/index.ts
export const config = {
  api: {
    url: process.env.API_URL || 'http://localhost:3000',
    timeout: parseInt(process.env.API_TIMEOUT || '5000'),
  },
  db: {
    host: process.env.DB_HOST || 'localhost',
  },
} as const;

// All files
import { config } from '@/config';
const apiUrl = config.api.url;
```
