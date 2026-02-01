# Ignore Patterns

## Global Ignore

Applies to ALL detectors:

```yaml
ignore:
  - '**/node_modules/**'
  - '**/dist/**'
  - '**/build/**'
  - '**/coverage/**'
  - '**/*.d.ts'
  - '**/__generated__/**'
```

---

## Per-Rule Exclude

Applies to specific detector:

```yaml
rules:
  cyclomatic_complexity:
    exclude:
      - '**/*.test.ts'
      - '**/legacy/**'

  god_module:
    exclude:
      - '**/controllers/**'
      - '**/generated/**'

  dead_code:
    exclude:
      - '**/scripts/**'
      - '**/migrations/**'
```

---

## Path Overrides

Apply different rules to specific paths:

```yaml
overrides:
  # Disable rules for tests
  - files: ['**/*.test.ts', '**/*.spec.ts', '**/__tests__/**']
    rules:
      cyclomatic_complexity: off
      cognitive_complexity: off
      long_params: off

  # Relax rules for legacy code
  - files: ['**/legacy/**', '**/deprecated/**']
    rules:
      god_module: low
      cyclic_dependency: medium
      high_coupling: off

  # Stricter rules for core domain
  - files: ['**/domain/**', '**/core/**']
    rules:
      cyclomatic_complexity: critical
      god_module: critical
```

---

## Glob Pattern Syntax

Archlint uses standard glob patterns:

| Pattern | Matches |
|---------|---------|
| `*` | Any characters except `/` |
| `**` | Any characters including `/` (recursive) |
| `?` | Single character |
| `[abc]` | One of a, b, c |
| `{a,b}` | Either a or b |

**Examples:**

```yaml
ignore:
  # All .test.ts files anywhere
  - '**/*.test.ts'

  # Everything in dist folders
  - '**/dist/**'

  # Specific file types
  - '**/*.{spec,test,stories}.ts'

  # Directories starting with "temp"
  - '**/temp*/**'

  # Files ending with .generated.ts
  - '**/*.generated.ts'
```

---

## Common Patterns

### TypeScript Projects

```yaml
ignore:
  - '**/node_modules/**'
  - '**/dist/**'
  - '**/*.d.ts'
  - '**/coverage/**'
```

### Monorepos

```yaml
ignore:
  - '**/node_modules/**'
  - '**/dist/**'
  - 'packages/*/build/**'
  - 'packages/*/lib/**'
```

### Frontend Projects

```yaml
ignore:
  - '**/node_modules/**'
  - '**/build/**'
  - '**/public/**'
  - '**/.next/**'        # Next.js
  - '**/.nuxt/**'        # Nuxt.js
  - '**/dist/**'
```

### Backend Projects

```yaml
ignore:
  - '**/node_modules/**'
  - '**/dist/**'
  - '**/migrations/**'
  - '**/seeds/**'
  - '**/generated/**'
```

---

## Test Files

```yaml
# Globally ignore
ignore:
  - '**/*.test.ts'
  - '**/*.spec.ts'
  - '**/__tests__/**'
  - '**/__mocks__/**'

# Or use overrides for flexibility
overrides:
  - files: ['**/*.{test,spec}.ts', '**/__tests__/**']
    rules:
      cyclomatic_complexity: off
      cognitive_complexity: off
      god_module: off
```

---

## Generated Code

```yaml
ignore:
  - '**/__generated__/**'
  - '**/*.generated.ts'
  - '**/generated/**'
  - '**/.graphql/**'    # GraphQL codegen
  - '**/prisma/generated/**'
```

---

## Build Artifacts

```yaml
ignore:
  - '**/dist/**'
  - '**/build/**'
  - '**/lib/**'
  - '**/.next/**'
  - '**/.nuxt/**'
  - '**/out/**'
  - '**/target/**'      # Rust/Java
```

---

## IDE and OS Files

```yaml
ignore:
  - '**/.idea/**'
  - '**/.vscode/**'
  - '**/.DS_Store'
  - '**/Thumbs.db'
```

---

## GitIgnore Integration

Archlint automatically respects `.gitignore` by default.

**Disable:**
```yaml
git:
  enabled: false
```

---

## Inline Ignores

For one-off cases:

```typescript
// archlint-disable-line cyclomatic_complexity
function complexFunction() { }

// archlint-disable-next-line god_module, high_coupling
class LargeClass { }

import './side-effect'; // archlint-disable-line side_effect_import

/* archlint-disable cyclomatic_complexity */
function legacy() { }
/* archlint-enable cyclomatic_complexity */

// archlint-disable * - Entire file is legacy
```

---

## Priority Order

When multiple ignore mechanisms apply:

1. **Inline comments** (highest priority)
2. **Per-rule exclude**
3. **Path overrides**
4. **Global ignore**
5. **.gitignore** (lowest priority)

---

## Debugging Ignore Issues

**Check what's being analyzed:**

```bash
npx @archlinter/cli --verbose
```

**Test a specific pattern:**

```bash
# See if file is ignored
npx @archlinter/cli -d cyclomatic_complexity --verbose | grep "my-file.ts"
```

---

## Best Practices

1. **Start broad, refine** - Begin with global ignore, use per-rule as needed
2. **Use path overrides for tests** - More flexible than global ignore
3. **Document why** - Add comments explaining ignore patterns
4. **Review periodically** - Check if ignored paths are still relevant
5. **Prefer specific patterns** - `**/dist/**` over `**/d*/**`
