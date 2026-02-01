# Dead Symbols

**What:** Exported symbol (function, class, type) never imported anywhere.

**Why it's bad:**
- Cognitive load when reading file
- Complicates refactoring
- Confusion about intended API

## Fix Process

### Step 1: Verify Unused

```bash
# Search for imports of the symbol
grep -r "import.*SymbolName" . --include="*.ts"
grep -r "from.*module.*SymbolName" . --include="*.ts"
```

### Step 2: Remove Export or Delete

```typescript
// BEFORE
export function unusedHelper() { }
export function usedFunction() { }

// AFTER Option 1: Remove export (if used internally)
function internalHelper() { }
export function usedFunction() { }

// AFTER Option 2: Delete (if truly unused)
export function usedFunction() { }
```

### Step 3: Handle Special Cases

```typescript
// If it's part of public API (npm package)
// Keep but document

/**
 * @public
 * @deprecated Use `newFunction` instead
 */
export function legacyFunction() { }
```
