# Detector-Specific Options

## God Module

```yaml
rules:
  god_module:
    severity: high
    fan_in: 15          # Max incoming dependencies (default: 10)
    fan_out: 15         # Max outgoing dependencies (default: 10)
    churn: 20           # Git change frequency threshold (default: 15)
    exclude: ['**/controllers/**']
```

**fan_in**: Number of files that import this module
**fan_out**: Number of files this module imports
**churn**: Number of commits touching this file (requires git)

---

## Cyclomatic Complexity

```yaml
rules:
  cyclomatic_complexity:
    severity: medium
    max_complexity: 15  # Default: 10
```

**max_complexity**: Maximum number of decision points (if, for, while, etc.)

---

## Cognitive Complexity

```yaml
rules:
  cognitive_complexity:
    severity: high
    max_complexity: 15  # Default: 15
```

**max_complexity**: Maximum cognitive load score

---

## Large File

```yaml
rules:
  large_file:
    severity: medium
    max_lines: 1000     # Default: 500
    exclude: ['**/*.config.ts']
```

**max_lines**: Maximum lines of code per file

---

## Deep Nesting

```yaml
rules:
  deep_nesting:
    severity: medium
    max_depth: 4        # Default: 3
```

**max_depth**: Maximum nesting levels

---

## Long Parameter List

```yaml
rules:
  long_params:
    severity: low
    max_params: 5       # Default: 4
```

**max_params**: Maximum function parameters

---

## High Coupling (CBO)

```yaml
rules:
  high_coupling:
    severity: medium
    max_cbo: 20         # Default: 15
```

**max_cbo**: Maximum Coupling Between Objects (fan_in + fan_out)

---

## Vendor Coupling

```yaml
rules:
  vendor_coupling:
    severity: medium
    max_files_per_package: 10    # Default: 8
    ignore_packages:
      - lodash
      - rxjs
      - date-fns
      - '@nestjs/*'
      - 'class-*'
```

**max_files_per_package**: How many files can import a package
**ignore_packages**: Packages to exclude (supports wildcards)

---

## Hub Dependency

```yaml
rules:
  hub_dependency:
    severity: medium
    min_dependents: 10  # Default: 8
    ignore_packages:
      - react
      - '@types/*'
```

**min_dependents**: Minimum dependents to flag as hub

---

## Hub Module

```yaml
rules:
  hub_module:
    severity: medium
    min_dependents: 10  # Default: 8
    min_dependencies: 5 # Default: 5
```

**min_dependents**: Min files importing this module
**min_dependencies**: Min files this module imports

---

## Dead Code

```yaml
rules:
  dead_code:
    severity: low
    exclude:
      - '**/tests/**'
      - '**/scripts/**'
```

**Note:** Uses `entry_points` from root config:
```yaml
entry_points:
  - 'src/main.ts'
  - 'src/index.ts'
  - 'src/workers/**/*.ts'
```

---

## Dead Symbols

```yaml
rules:
  dead_symbols:
    severity: low
    ignore_methods:
      - constructor
      - ngOnInit
      - ngOnDestroy
    contract_methods:
      ValidatorConstraintInterface:
        - validate
        - defaultMessage
      OnInit:
        - ngOnInit
```

**ignore_methods**: Methods to always ignore
**contract_methods**: Interface → required methods mapping

---

## Code Clone

```yaml
rules:
  code_clone:
    severity: medium
    min_lines: 6        # Default: 6
    min_tokens: 50      # Default: 50
```

**min_lines**: Minimum lines to consider as clone
**min_tokens**: Minimum token similarity

---

## LCOM (Low Cohesion)

```yaml
rules:
  lcom:
    severity: medium
    threshold: 0.8      # Default: 0.8 (80%)
```

**threshold**: LCOM4 cohesion threshold (0.0-1.0)

---

## Feature Envy

```yaml
rules:
  feature_envy:
    severity: medium
    ratio: 2.0          # Default: 2.0
```

**ratio**: External refs / internal refs threshold

---

## Shotgun Surgery

```yaml
rules:
  shotgun_surgery:
    severity: medium
    min_coupled_files: 5  # Default: 5
```

**min_coupled_files**: Minimum co-changing files to flag

---

## Unstable Interface

```yaml
rules:
  unstable_interface:
    severity: medium
    min_dependents: 5   # Default: 5
    min_churn: 10       # Default: 10
```

**min_dependents**: Min dependents to check
**min_churn**: Min commits to flag as unstable

---

## SDP Violation

```yaml
rules:
  sdp_violation:
    severity: high
    stability_threshold: 0.5  # Default: 0.5
```

**stability_threshold**: Stability score threshold

---

## Abstractness Violation

```yaml
rules:
  abstractness:
    severity: medium
    distance_threshold: 0.3  # Default: 0.3
```

**distance_threshold**: Distance from main sequence

---

## Barrel File Abuse

```yaml
rules:
  barrel_file:
    severity: medium
    max_exports: 20     # Default: 15
```

**max_exports**: Maximum re-exports in index file

---

## Primitive Obsession

```yaml
rules:
  primitive_obsession:
    severity: low
    max_primitives: 5   # Default: 4
```

**max_primitives**: Max primitive params in function

---

## Scattered Configuration

```yaml
rules:
  scattered_config:
    severity: medium
    max_files: 5        # Default: 5
```

**max_files**: Max files accessing env vars

---

## Shared Mutable State

```yaml
rules:
  shared_mutable_state:
    severity: high
    # No options - detects exported let/var
```

---

## Complete Example

```yaml
rules:
  god_module:
    severity: high
    fan_in: 20
    fan_out: 20
    churn: 30

  cyclomatic_complexity:
    severity: medium
    max_complexity: 15

  large_file:
    severity: medium
    max_lines: 1000

  vendor_coupling:
    severity: medium
    max_files_per_package: 10
    ignore_packages:
      - lodash
      - '@nestjs/*'

  dead_symbols:
    severity: low
    contract_methods:
      ValidatorConstraintInterface: [validate, defaultMessage]

  layer_violation:
    severity: high
    layers:
      - name: domain
        path: '**/domain/**'
        allowed_imports: []
```
