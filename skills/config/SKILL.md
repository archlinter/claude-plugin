---
name: config
description: "Configure archlint for your project. Use for initial setup, adjusting detector settings, fixing false positives, configuring layer architecture, setting up ignore patterns, and customizing severity levels. Triggers on configuration questions, false positive reports, setup requests."
---

# Configuring Archlint

## Quick Start

```bash
# Generate interactive config
npx @archlinter/cli init

# Create config manually
touch .archlint.yaml
```

## Configuration Structure

```yaml
# Global ignore patterns
ignore:
  - '**/node_modules/**'
  - '**/dist/**'

# Path aliases (from tsconfig.json)
aliases:
  '@/*': 'src/*'

# TypeScript integration
tsconfig: true

# Extend from presets
extends:
  - nestjs

# Entry points for dead code detection
entry_points:
  - 'src/main.ts'

# Detector rules
rules:
  cyclic_dependency: high
  dead_code: medium

  god_module:
    severity: high
    fan_in: 15
    fan_out: 15

# Path-specific overrides
overrides:
  - files: ['**/legacy/**']
    rules:
      cyclomatic_complexity: off
```

## Common Tasks

### Fix False Positives
See [false-positives.md](references/false-positives.md)

### Configure Layer Architecture
See [layers.md](references/layers.md)

### Adjust Detector Settings
See [detector-options.md](references/detector-options.md)

### Setup Ignore Patterns
See [ignore-patterns.md](references/ignore-patterns.md)

### Initial Project Setup
See [initial-setup.md](references/initial-setup.md)

## Framework Presets

Built-in presets:
- `nestjs` - NestJS applications
- `nextjs` - Next.js applications
- `express` - Express.js servers
- `react` - React applications
- `angular` - Angular applications
- `vue` - Vue.js applications

```yaml
extends:
  - nestjs
```
