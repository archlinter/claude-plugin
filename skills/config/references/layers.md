# Layer Architecture Configuration

## Overview

Layer architecture enforcement ensures dependencies flow in one direction (e.g., domain doesn't depend on infrastructure).

## Basic Setup

```yaml
rules:
  layer_violation:
    severity: high
    layers:
      - name: domain
        path: '**/domain/**'
        allowed_imports: []

      - name: application
        path: '**/application/**'
        allowed_imports:
          - domain

      - name: infrastructure
        path: '**/infrastructure/**'
        allowed_imports:
          - domain
          - application
```

**How it works:**
- Files matched by `path` belong to that layer
- Imports are checked against `allowed_imports`
- Violation = importing from disallowed layer

---

## Clean Architecture

```yaml
rules:
  layer_violation:
    severity: high
    layers:
      # Core - no dependencies
      - name: domain
        path: '**/domain/**'
        allowed_imports: []

      # Use cases - depends on domain
      - name: application
        path: '**/application/**'
        allowed_imports:
          - domain

      # Adapters - depend on application and domain
      - name: infrastructure
        path: '**/infrastructure/**'
        allowed_imports:
          - domain
          - application

      # UI/API - outermost layer
      - name: presentation
        path: '**/presentation/**'
        allowed_imports:
          - domain
          - application
          - infrastructure
```

---

## Three-Tier Architecture

```yaml
rules:
  layer_violation:
    severity: high
    layers:
      # Data layer
      - name: data
        path: '**/data/**'
        allowed_imports: []

      # Business logic layer
      - name: business
        path: '**/business/**'
        allowed_imports:
          - data

      # Presentation layer
      - name: presentation
        path: '**/presentation/**'
        allowed_imports:
          - business
          - data
```

---

## Hexagonal Architecture

```yaml
rules:
  layer_violation:
    severity: high
    layers:
      # Core domain
      - name: core
        path: '**/core/**'
        allowed_imports: []

      # Ports (interfaces)
      - name: ports
        path: '**/ports/**'
        allowed_imports:
          - core

      # Adapters
      - name: adapters
        path: '**/adapters/**'
        allowed_imports:
          - core
          - ports
```

---

## Feature-Based Architecture

```yaml
rules:
  layer_violation:
    severity: high
    layers:
      # Shared utilities
      - name: shared
        path: '**/shared/**'
        allowed_imports: []

      # Feature modules (can depend on shared)
      - name: features
        path: '**/features/**'
        allowed_imports:
          - shared
          # Note: features should NOT import from other features
```

To prevent cross-feature imports, use additional configuration or rely on `cyclic_dependency` detector.

---

## NestJS Module Architecture

```yaml
rules:
  layer_violation:
    severity: high
    layers:
      # Common/shared module
      - name: common
        path: '**/common/**'
        allowed_imports: []

      # Domain modules
      - name: users
        path: '**/users/**'
        allowed_imports:
          - common

      - name: orders
        path: '**/orders/**'
        allowed_imports:
          - common
          - users  # Orders can depend on users

      - name: payments
        path: '**/payments/**'
        allowed_imports:
          - common
          - orders  # Payments depend on orders
```

---

## Frontend Layer Separation

```yaml
rules:
  layer_violation:
    severity: high
    layers:
      # Core business logic
      - name: domain
        path: '**/domain/**'
        allowed_imports: []

      # UI components (pure presentation)
      - name: components
        path: '**/components/**'
        allowed_imports:
          - domain

      # Pages/containers (orchestration)
      - name: pages
        path: '**/pages/**'
        allowed_imports:
          - domain
          - components

      # API/services layer
      - name: services
        path: '**/services/**'
        allowed_imports:
          - domain
```

---

## Multiple Path Patterns

```yaml
rules:
  layer_violation:
    layers:
      - name: domain
        path:
          - '**/core/**'
          - '**/domain/**'
          - '**/entities/**'
        allowed_imports: []

      - name: infrastructure
        path:
          - '**/infra/**'
          - '**/infrastructure/**'
          - '**/adapters/**'
        allowed_imports:
          - domain
```

---

## Excluding Files from Layer Checks

```yaml
rules:
  layer_violation:
    severity: high
    exclude:
      - '**/*.test.ts'
      - '**/*.spec.ts'
      - '**/migrations/**'
    layers:
      # ... layer definitions
```

---

## Advanced: Conditional Imports

Sometimes you need more flexibility:

```yaml
rules:
  layer_violation:
    layers:
      - name: domain
        path: '**/domain/**'
        allowed_imports: []

      - name: application
        path: '**/application/**'
        allowed_imports:
          - domain

      # Special case: allow infrastructure to import from application
      # for repositories implementing interfaces
      - name: infrastructure
        path: '**/infrastructure/**'
        allowed_imports:
          - domain
          - application
```

---

## Troubleshooting

### Issue: Layer not detected

**Problem:** Files not matching any layer.

**Check:**
```bash
npx @archlinter/cli -d layer_violation --verbose
```

**Solution:** Verify path patterns match your structure:
```yaml
# Too specific
path: 'src/domain/**'  # Only matches src/domain

# Better
path: '**/domain/**'   # Matches anywhere
```

---

### Issue: Too many violations

**Start permissive, tighten gradually:**

```yaml
# Step 1: Allow everything, identify structure
rules:
  layer_violation:
    severity: low  # Just log violations
    layers:
      # Define layers loosely

# Step 2: Document current state
# Run scan, analyze violations

# Step 3: Tighten rules incrementally
# Fix violations or adjust allowed_imports

# Step 4: Increase severity
rules:
  layer_violation:
    severity: high  # Enforce strictly
```

---

## Best Practices

1. **Start with 2-3 layers** - Don't over-engineer initially
2. **Use framework presets** - `extends: nestjs` handles common cases
3. **Document your architecture** - Add comments explaining layer boundaries
4. **Review violations carefully** - Some may indicate design issues
5. **Use path overrides** - Relax rules for legacy code temporarily
