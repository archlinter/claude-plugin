# Scattered Module

**What:** Module exports unrelated things (low cohesion).

**Why it's bad:**
- Hard to understand and reuse
- Unrelated changes affect same file

## Fix Strategy: Split by Cohesion

```typescript
// BEFORE: helpers.ts (scattered)
export function formatDate() { }
export function sendEmail() { }
export function calculateTax() { }
export function compressImage() { }

// AFTER: Group by cohesion
// date-utils.ts
export function formatDate() { }

// email-service.ts
export function sendEmail() { }

// tax-calculator.ts
export function calculateTax() { }

// image-processor.ts
export function compressImage() { }
```
