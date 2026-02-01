# Archlint Claude Plugin

Claude Code plugin for fixing architectural smells detected by [archlint](https://github.com/archlinter/archlint).

## Installation

```bash
# Add marketplace
/plugin marketplace add archlinter/claude-plugin

# Install plugin
/plugin install archlint
```

Or install directly:

```bash
/plugin install archlint@archlinter/claude-plugin
```

## Usage

### Fixing Architectural Smells

1. Run archlint to detect smells:

```bash
npx @archlinter/cli
```

2. Use the fix skill:

```
/archlint:fix
```

The skill will guide you through understanding and fixing detected smells.

### Configuring Archlint

For initial setup, false positives, or adjusting detector settings:

```
/archlint:config
```

Helps with:
- Initial project setup
- Fixing false positives
- Configuring layer architecture
- Adjusting detector thresholds
- Setting up ignore patterns

## Supported Smells

### Dependency (8)
- Cyclic dependencies
- Circular type dependencies
- High coupling (CBO)
- Hub dependency
- Hub module
- Layer violation
- Package cycle
- Vendor coupling

### Design (12)
- God module
- Feature envy
- Shotgun surgery
- Unstable interface
- SDP violation
- Barrel file abuse
- Scattered module
- Orphan type
- Shared mutable state
- Abstractness violation
- Primitive obsession
- Scattered configuration

### Hygiene (4)
- Dead code
- Dead symbols
- Test leakage
- Side-effect import

### Metrics (6)
- Cyclomatic complexity
- Cognitive complexity
- Large file
- Deep nesting
- Long parameter list
- Low cohesion (LCOM)

### Code Clone (1)
- Code clone detection

## License

MIT
