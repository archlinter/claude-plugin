---
name: archlint-fix
description: "Fix architectural smells detected by archlint. Use when user has archlint output showing smells (cycles, god_module, high_coupling, dead_code, etc.) and needs help understanding or fixing them. Triggers on archlint scan results, smell names, architecture issues, circular dependencies, coupling problems."
---

# Fixing Archlint Smells

## Running Archlint

```bash
# Scan project (enabled detectors only)
npx @archlinter/cli

# Scan with detailed explanations
npx @archlinter/cli --explain

# Run specific detector(s)
npx @archlinter/cli -d cyclic_dependency
npx @archlinter/cli -d god_module,dead_code,large_file

# Run ALL detectors (including disabled)
npx @archlinter/cli -A

# Exclude specific detectors
npx @archlinter/cli -e code_clone,large_file

# Filter by severity
npx @archlinter/cli -s high          # high and critical only
npx @archlinter/cli -s medium        # medium, high, critical

# Output formats
npx @archlinter/cli -f json          # JSON output
npx @archlinter/cli -f markdown      # Markdown report
npx @archlinter/cli -f sarif         # SARIF for IDE integration

# Compare against baseline (for PRs)
npx @archlinter/cli diff .archlint-baseline.json

# Save baseline
npx @archlinter/cli snapshot -o .archlint-baseline.json
```

## Detector IDs

Use these IDs with `-d` (only run) or `-e` (exclude) flags:

### Enabled by Default
| ID | Name |
|----|------|
| `barrel_file` | Barrel File Abuse |
| `code_clone` | Code Clone |
| `cognitive_complexity` | High Cognitive Complexity |
| `cyclic_dependency` | Cyclic Dependency |
| `cyclomatic_complexity` | High Cyclomatic Complexity |
| `dead_code` | Dead Code |
| `dead_symbols` | Dead Symbol |
| `deep_nesting` | Deep Nesting |
| `god_module` | God Module |
| `large_file` | Large File |
| `long_params` | Long Parameter List |
| `orphan_types` | Orphan Type |
| `side_effect_import` | Side-effect Import |

### Disabled by Default (use `-A` or `-d` to enable)
| ID | Name |
|----|------|
| `abstractness` | Abstractness Violation |
| `circular_type_deps` | Circular Type Dependency |
| `feature_envy` | Feature Envy |
| `high_coupling` | High Coupling (CBO) |
| `hub_dependency` | Hub Dependency |
| `hub_module` | Hub Module |
| `layer_violation` | Layer Violation |
| `lcom` | Low Cohesion (LCOM) |
| `module_cohesion` | Scattered Module |
| `package_cycles` | Package Cycle |
| `primitive_obsession` | Primitive Obsession |
| `scattered_config` | Scattered Configuration |
| `sdp_violation` | SDP Violation |
| `shared_mutable_state` | Shared Mutable State |
| `shotgun_surgery` | Shotgun Surgery |
| `test_leakage` | Test Leakage |
| `unstable_interface` | Unstable Interface |
| `vendor_coupling` | Vendor Coupling |

## Workflow

1. **Run archlint** to identify smells
2. **Find the smell reference** in the appropriate category below
3. **Understand the context** in user's codebase
4. **Apply the recommended fix pattern**
5. **Re-run archlint** to verify fix

## Smell References

### Dependency Smells
| Smell | Reference |
|-------|-----------|
| cyclic_dependency | [references/dependency/cycles.md](references/dependency/cycles.md) |
| circular_type_deps | [references/dependency/circular-type-deps.md](references/dependency/circular-type-deps.md) |
| high_coupling | [references/dependency/high-coupling.md](references/dependency/high-coupling.md) |
| hub_dependency | [references/dependency/hub-dependency.md](references/dependency/hub-dependency.md) |
| hub_module | [references/dependency/hub-module.md](references/dependency/hub-module.md) |
| layer_violation | [references/dependency/layer-violation.md](references/dependency/layer-violation.md) |
| package_cycles | [references/dependency/package-cycle.md](references/dependency/package-cycle.md) |
| vendor_coupling | [references/dependency/vendor-coupling.md](references/dependency/vendor-coupling.md) |

### Design Smells
| Smell | Reference |
|-------|-----------|
| god_module | [references/design/god-module.md](references/design/god-module.md) |
| feature_envy | [references/design/feature-envy.md](references/design/feature-envy.md) |
| shotgun_surgery | [references/design/shotgun-surgery.md](references/design/shotgun-surgery.md) |
| unstable_interface | [references/design/unstable-interface.md](references/design/unstable-interface.md) |
| sdp_violation | [references/design/sdp-violation.md](references/design/sdp-violation.md) |
| barrel_file | [references/design/barrel-abuse.md](references/design/barrel-abuse.md) |
| module_cohesion | [references/design/scattered-module.md](references/design/scattered-module.md) |
| orphan_types | [references/design/orphan-type.md](references/design/orphan-type.md) |
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
