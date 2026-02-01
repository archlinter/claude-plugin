---
name: archlint-fix
description: "Fix architectural smells detected by archlint. Use when user has archlint output showing smells (cycles, god_module, high_coupling, dead_code, etc.) and needs help understanding or fixing them. Triggers on archlint scan results, smell names, architecture issues, circular dependencies, coupling problems."
---

# Fixing Archlint Smells

## Running Archlint

```bash
# Scan project for all smells
npx @archlinter/cli

# Compare against baseline (for PRs)
npx @archlinter/cli diff .archlint-baseline.json

# Or compare against branch
npx @archlinter/cli <branch_name>

# Save baseline
npx @archlinter/cli snapshot -o .archlint-baseline.json
```

## Workflow

1. **Run archlint** to identify smells
2. **Find the smell reference** in the appropriate category
3. **Understand the context** in user's codebase
4. **Apply the recommended fix pattern**
5. **Re-run archlint** to verify fix

## Smell Categories

### Dependency Smells
| Smell | Reference |
|-------|-----------|
| cycles | [references/dependency/cycles.md](references/dependency/cycles.md) |
| circular_type_deps | [references/dependency/circular-type-deps.md](references/dependency/circular-type-deps.md) |
| high_coupling | [references/dependency/high-coupling.md](references/dependency/high-coupling.md) |
| hub_dependency | [references/dependency/hub-dependency.md](references/dependency/hub-dependency.md) |
| hub_module | [references/dependency/hub-module.md](references/dependency/hub-module.md) |
| layer_violation | [references/dependency/layer-violation.md](references/dependency/layer-violation.md) |
| package_cycle | [references/dependency/package-cycle.md](references/dependency/package-cycle.md) |
| vendor_coupling | [references/dependency/vendor-coupling.md](references/dependency/vendor-coupling.md) |

### Design Smells
| Smell | Reference |
|-------|-----------|
| god_module | [references/design/god-module.md](references/design/god-module.md) |
| feature_envy | [references/design/feature-envy.md](references/design/feature-envy.md) |
| shotgun_surgery | [references/design/shotgun-surgery.md](references/design/shotgun-surgery.md) |
| unstable_interface | [references/design/unstable-interface.md](references/design/unstable-interface.md) |
| sdp_violation | [references/design/sdp-violation.md](references/design/sdp-violation.md) |
| barrel_abuse | [references/design/barrel-abuse.md](references/design/barrel-abuse.md) |
| scattered_module | [references/design/scattered-module.md](references/design/scattered-module.md) |
| orphan_type | [references/design/orphan-type.md](references/design/orphan-type.md) |
| shared_mutable_state | [references/design/shared-mutable-state.md](references/design/shared-mutable-state.md) |
| abstractness | [references/design/abstractness.md](references/design/abstractness.md) |
| primitive_obsession | [references/design/primitive-obsession.md](references/design/primitive-obsession.md) |
| scattered_config | [references/design/scattered-config.md](references/design/scattered-config.md) |

### Hygiene Smells
| Smell | Reference |
|-------|-----------|
| dead_code | [references/hygiene/dead-code.md](references/hygiene/dead-code.md) |
| dead_symbols | [references/hygiene/dead-symbols.md](references/hygiene/dead-symbols.md) |
| test_leakage | [references/hygiene/test-leakage.md](references/hygiene/test-leakage.md) |
| side_effect_import | [references/hygiene/side-effect-import.md](references/hygiene/side-effect-import.md) |

### Metrics Smells
| Smell | Reference |
|-------|-----------|
| cyclomatic_complexity | [references/metrics/cyclomatic-complexity.md](references/metrics/cyclomatic-complexity.md) |
| cognitive_complexity | [references/metrics/cognitive-complexity.md](references/metrics/cognitive-complexity.md) |
| large_file | [references/metrics/large-file.md](references/metrics/large-file.md) |
| deep_nesting | [references/metrics/deep-nesting.md](references/metrics/deep-nesting.md) |
| long_params | [references/metrics/long-params.md](references/metrics/long-params.md) |
| lcom | [references/metrics/lcom.md](references/metrics/lcom.md) |

### Code Clone
| Smell | Reference |
|-------|-----------|
| code_clone | [references/code-clone/code-clone.md](references/code-clone/code-clone.md) |

## Common Fix Patterns

**Breaking cycles:** Extract shared code → Create interface → Dependency injection

**Reducing coupling:** Facade pattern → Event-driven → Dependency inversion

**Splitting god modules:** Identify cohesive groups → Extract by domain → Keep facade if needed

**Removing dead code:** Verify unused → Check dynamic imports → Delete or archive
