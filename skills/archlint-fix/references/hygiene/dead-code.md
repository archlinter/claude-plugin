# Dead Code

**What:** File not imported by any other module.

**Why it's bad:**
- Increases codebase size and maintenance burden
- Causes confusion about what's actually used
- May contain outdated patterns or vulnerabilities

## Fix Process

### Step 1: Verify It's Truly Unused

```bash
# Check for dynamic imports
grep -r "import(" . --include="*.ts" | grep "filename"

# Check for string-based requires
grep -r "require(" . --include="*.ts" | grep "filename"

# Check test files
grep -r "from.*filename" . --include="*.test.ts"

# Check config files (webpack, jest, etc.)
grep -r "filename" *.config.*
```

### Step 2: Check if Intentional Entry Point

```yaml
# archlint.config.yaml
detectors:
  dead_code:
    entry_points:
      - "src/workers/*.ts"      # Web workers
      - "src/scripts/*.ts"      # CLI scripts
      - "src/lambdas/*.ts"      # Serverless functions
```

### Step 3: Remove or Archive

```bash
# If confirmed dead:
# Option 1: Delete the file
git rm src/unused-module.ts

# Option 2: Move to archive (if keeping for reference)
mkdir -p .archive
git mv src/unused-module.ts .archive/
```
