# Initial Project Setup

## Step 1: Generate Config

```bash
# Interactive setup (recommended)
npx @archlinter/cli init

# Non-interactive with preset
npx @archlinter/cli init --no-interactive --presets nestjs

# Force overwrite existing config
npx @archlinter/cli init --force
```

---

## Step 2: Run Initial Scan

```bash
# See what archlint detects
npx @archlinter/cli
```

**Expected output:**
- Many smells detected (hundreds possible)
- Don't panic! This is normal for existing projects

---

## Step 3: Create Baseline

```bash
# Save current state as baseline
npx @archlinter/cli snapshot -o .archlint-baseline.json
```

**Why:**
- Prevents flagging existing issues
- Focus on *new* architectural drift
- Team doesn't need to fix everything immediately

---

## Step 4: Configure for Your Project

### Start with Framework Preset

```yaml
# .archlint.yaml
extends:
  - nestjs       # or nextjs, express, react, etc.
```

### Add Basic Ignores

```yaml
ignore:
  - '**/node_modules/**'
  - '**/dist/**'
  - '**/*.d.ts'
  - '**/coverage/**'
  - '**/__generated__/**'
```

### Set Entry Points

```yaml
entry_points:
  - 'src/main.ts'           # NestJS
  # or
  - 'src/index.ts'          # Express
  # or
  - 'pages/_app.tsx'        # Next.js
```

---

## Step 5: Adjust Severity

Start permissive, tighten over time:

```yaml
rules:
  # Critical issues only initially
  cyclic_dependency: high
  layer_violation: high

  # Informational for now
  god_module: low
  large_file: low
  dead_code: low
```

---

## Step 6: Set Up CI

### GitHub Actions

```yaml
# .github/workflows/archlint.yml
name: Architecture

on: [pull_request]

jobs:
  archlint:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      pull-requests: write
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: archlint
        uses: archlinter/action@v1
        with:
          baseline: origin/${{ github.base_ref }}
          fail-on: high
          github-token: ${{ github.token }}
```

### GitLab CI

```yaml
# .gitlab-ci.yml
architecture:
  script:
    - npx @archlinter/cli diff origin/$CI_MERGE_REQUEST_TARGET_BRANCH_NAME --fail-on high
  rules:
    - if: $CI_PIPELINE_SOURCE == "merge_request_event"
```

---

## Step 7: Team Onboarding

### Add to package.json

```json
{
  "scripts": {
    "lint:arch": "archlint",
    "lint:arch:ci": "archlint diff .archlint-baseline.json"
  }
}
```

### Document in README

```markdown
## Architecture Linting

We use archlint to prevent architectural drift.

Run locally:
```bash
npm run lint:arch
```

The CI will fail if you introduce new architectural issues.
```

---

## Common Initial Configurations

### NestJS Project

```yaml
extends:
  - nestjs

ignore:
  - '**/node_modules/**'
  - '**/dist/**'
  - '**/*.spec.ts'

entry_points:
  - 'src/main.ts'

rules:
  cyclic_dependency: high
  layer_violation: high
  dead_code: medium

  god_module:
    severity: medium
    fan_in: 20
    fan_out: 20
```

### Next.js Project

```yaml
extends:
  - nextjs

ignore:
  - '**/node_modules/**'
  - '**/.next/**'
  - '**/out/**'

entry_points:
  - 'pages/_app.tsx'
  - 'pages/api/**/*.ts'

rules:
  cyclic_dependency: high
  large_file:
    severity: medium
    max_lines: 500
```

### Express API

```yaml
extends:
  - express

ignore:
  - '**/node_modules/**'
  - '**/dist/**'

entry_points:
  - 'src/index.ts'
  - 'src/server.ts'

rules:
  cyclic_dependency: high
  god_module: medium
  vendor_coupling:
    severity: medium
    ignore_packages:
      - express
      - '@types/*'
```

### React Library

```yaml
extends:
  - react

ignore:
  - '**/node_modules/**'
  - '**/dist/**'
  - '**/lib/**'
  - '**/*.stories.tsx'

entry_points:
  - 'src/index.ts'

rules:
  cyclic_dependency: high
  dead_code: medium
  orphan_types: low
```

---

## Gradual Adoption Strategy

### Month 1: Observation

```yaml
# Just observe, don't fail CI
rules:
  cyclic_dependency: low
  god_module: low
  dead_code: low

# In CI: --fail-on critical
```

### Month 2: Block New Issues

```yaml
# Fail on new architectural debt
rules:
  cyclic_dependency: high
  god_module: medium
  dead_code: low

# In CI: --fail-on medium
```

### Month 3: Tighten Rules

```yaml
# Stricter thresholds
rules:
  god_module:
    severity: high
    fan_in: 15  # Was 20
    fan_out: 15

  large_file:
    severity: high
    max_lines: 500  # Was 1000
```

---

## Troubleshooting Initial Setup

### Too Many Violations

**Solution:** Start with baseline
```bash
npx @archlinter/cli snapshot -o .archlint-baseline.json
```

Then use `diff` mode:
```bash
npx @archlinter/cli diff .archlint-baseline.json
```

---

### Framework Not Detected

**Solution:** Use explicit preset
```yaml
extends:
  - nestjs  # or your framework
```

---

### Entry Points Not Working

**Check:** Run with verbose
```bash
npx @archlinter/cli -d dead_code --verbose
```

**Fix:** Ensure entry points are correctly specified
```yaml
entry_points:
  - 'src/main.ts'     # Correct
  # NOT:
  # - '/src/main.ts'  # Wrong - absolute path
  # - 'main.ts'       # Wrong - missing directory
```

---

## Next Steps

1. **Review violations** - Understand what archlint found
2. **Configure false positives** - See [false-positives.md](false-positives.md)
3. **Set up layers** - If using layered architecture, see [layers.md](layers.md)
4. **Adjust thresholds** - Fine-tune detector options, see [detector-options.md](detector-options.md)
5. **Monitor in CI** - Track regressions over time
