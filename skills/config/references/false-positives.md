# Fixing False Positives

## Method 1: Inline Comments (Recommended)

Use for individual cases where the smell is intentional or unavoidable.

```typescript
// archlint-disable-line cyclomatic_complexity
function complexLegacyFunction() { }

// archlint-disable-next-line god_module
class IntegrationController { }

import { legacy } from './old'; // archlint-disable-line vendor_coupling
```

**Block disable:**
```typescript
/* archlint-disable cyclomatic_complexity, cognitive_complexity */
function unavoidablyComplexLogic() {
  // Complex but necessary code
}
/* archlint-enable cyclomatic_complexity, cognitive_complexity */
```

**File-level disable:**
```typescript
// archlint-disable * - Legacy file, refactoring planned for Q2
```

---

## Method 2: Contract Methods

For interface implementations that appear unused but are required.

```yaml
# .archlint.yaml
rules:
  dead_symbols:
    contract_methods:
      # NestJS validators
      ValidatorConstraintInterface:
        - validate
        - defaultMessage

      # Angular lifecycle
      OnInit:
        - ngOnInit

      # Custom interfaces
      MyPluginInterface:
        - initialize
        - execute
```

**Example:**
```typescript
// Without config: archlint reports 'validate' as dead
class EmailValidator implements ValidatorConstraintInterface {
  validate(email: string) { } // Required by interface
  defaultMessage() { }        // Required by interface
}

// With config above: no false positive
```

---

## Method 3: Exclude Patterns

For entire directories or file patterns.

```yaml
rules:
  god_module:
    exclude:
      - '**/generated/**'     # Auto-generated files
      - '**/migrations/**'    # Database migrations
      - '**/*.entity.ts'      # ORM entities

  cyclomatic_complexity:
    exclude:
      - '**/legacy/**'
      - '**/deprecated/**'
```

---

## Method 4: Path Overrides

For applying different rules to specific paths.

```yaml
overrides:
  # Relax rules for tests
  - files: ['**/*.test.ts', '**/*.spec.ts']
    rules:
      cyclomatic_complexity: off
      cognitive_complexity: off
      long_params: off

  # Disable architecture rules for legacy code
  - files: ['**/legacy/**', '**/deprecated/**']
    rules:
      god_module: off
      cyclic_dependency: low

  # Stricter rules for new feature
  - files: ['**/features/payment/**']
    rules:
      cyclomatic_complexity: high
      god_module: critical
```

---

## Method 5: Adjust Thresholds

Sometimes the default thresholds are too strict for your project.

```yaml
rules:
  # Default max_complexity: 10 → too strict
  cyclomatic_complexity:
    severity: medium
    max_complexity: 15

  # Default fan_in/fan_out: 10 → increase
  god_module:
    severity: high
    fan_in: 20
    fan_out: 20
    churn: 30

  # Default max_lines: 500 → increase
  large_file:
    severity: medium
    max_lines: 1000
```

---

## Method 6: Ignore Packages (Vendor Coupling)

For widely-used utility libraries.

```yaml
rules:
  vendor_coupling:
    severity: medium
    max_files_per_package: 10
    ignore_packages:
      - lodash
      - rxjs
      - date-fns
      - '@nestjs/*'      # Wildcard support
      - 'class-*'        # Glob pattern
```

---

## Common False Positive Scenarios

### Scenario 1: Framework Controllers (God Module)

**Problem:** NestJS/Express controllers flagged as god modules.

**Solution:**
```yaml
overrides:
  - files: ['**/*.controller.ts', '**/controllers/**']
    rules:
      god_module: off
```

Or use preset:
```yaml
extends:
  - nestjs  # Automatically handles this
```

---

### Scenario 2: Test Utilities (Dead Code)

**Problem:** Test helpers not imported by production code.

**Solution:**
```yaml
rules:
  dead_code:
    exclude:
      - '**/test-utils/**'
      - '**/testing/**'
      - '**/__tests__/helpers/**'
```

---

### Scenario 3: Database Entities (Orphan Types)

**Problem:** ORM entity fields reported as unused.

**Solution:**
```yaml
rules:
  dead_symbols:
    exclude:
      - '**/*.entity.ts'
      - '**/entities/**'
      - '**/models/**'
```

---

### Scenario 4: Polyfills (Side-Effect Imports)

**Problem:** `import './polyfills'` flagged.

**Solution:**
```yaml
rules:
  side_effect_import:
    exclude:
      - '**/polyfills.ts'
      - '**/main.ts'  # Entry point
```

Or inline:
```typescript
import './polyfills'; // archlint-disable-line side_effect_import
```

---

### Scenario 5: Migration Scripts (Scattered Config)

**Problem:** Migration files access env vars.

**Solution:**
```yaml
rules:
  scattered_config:
    exclude:
      - '**/migrations/**'
      - '**/scripts/**'
```

---

## Workflow for Handling False Positives

1. **Verify it's truly a false positive**
   - Is the smell legitimate but unavoidable?
   - Is it in legacy code you can't refactor now?
   - Is it a framework pattern archlint doesn't recognize?

2. **Choose suppression method**
   - **One-off case** → Inline comment
   - **Pattern across project** → Exclude pattern or override
   - **Framework requirement** → Contract methods or preset

3. **Document why**
   ```typescript
   // archlint-disable-line god_module - Integration hub required by architecture
   ```

4. **Review periodically**
   - Add to team docs: "We allow X because Y"
   - Revisit when refactoring
