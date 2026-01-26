# Policy-to-Outcome Compiler (POC)

## One-sentence value proposition
Compiles human or institutional policy into machine-verifiable governance artifacts that ensure structural compliance and traceability.

## Overview
The Policy-to-Outcome Compiler (POC) is a deterministic system that translates policy intent into executable constraints for three external governance systems:
- DIO (Deterministic Input–Output): Ensures execution determinism
- ZT-AAS (Zero-Trust Authority & Scope): Manages authority validation
- ICAE (Immutable Cost Attribution Engine): Tracks economic truth

POC v1.0.0 guarantees that policy is compiled into artifacts with provable backward auditability, where every execution, authorization, or cost artifact can be traced back to exactly one policy clause.

## Architecture diagram
<pre>
+-------------------+
|   Policy Input    |
+-------------------+
          |
          v
+-------------------+
| Intent Normalizer |
+-------------------+
          |
          v
+--------------------+
|  Artifact Compiler |
|  - DIO Invariants  |
|  - ZT Authority    |
|  - ICAE Costs      |
+--------------------+
          |
          v
+--------------------+
|  Traceability Map  |
+--------------------+
          |
          v
+---------------------+
| Compilation Verdict |
+---------------------+
</pre>

## Core Components
- `PolicyCompiler`: Main compilation engine that orchestrates the full pipeline
- `IntentNormalization`: Policy intent parsing and semantic validation
- `DIOInvariant`: Execution constraints for deterministic behavior
- `ZTAuthority`: Authority scopes and delegation rules with explicit principals
- `ICAEConstraint`: Cost attribution and measurement constructs with explicit units
- `TraceabilityEntry`: Clause-to-artifact mapping ensuring exact traceability

## Usage

```rust
use poc::PolicyCompiler;

let compiler = PolicyCompiler::new();
let result = compiler.compile("All actions must be logged by SYSTEM.");
if result.is_success() {
    println!("Compilation successful");
    // Access compiled artifacts:
    // result.dio_invariants, result.zt_authority_graph, result.icae_constraints
} else {
    println!("Compilation failed: {:?}", result.error_messages());
}
```

## Example Policies

### Basic Policy
All actions must be logged by SYSTEM. No unauthorized access allowed by USER.

### Cost-Constrained Policy
All actions must be logged by SYSTEM. Cost of logging cannot exceed 1000 USD per month.

### Complex Policy with Assumptions
Assumes network is secure. All actions must be logged by SYSTEM. No unauthorized access allowed by USER. Except for testing.

## Design Principles
- **Determinism**: All outputs are deterministic from inputs with no implicit behavior
- **Explicitness**: No silent assumptions or inferred authority - every principal must be explicitly named
- **Auditability**: Full traceability from execution to policy clause with exact mapping
- **Separation of Concerns**: Each artifact type is handled independently with strict boundaries
- **Fail Fast**: Semantic violations are detected and reported immediately without partial compilation

## Requirements
- Rust 1.56+
- Standard library only (no external dependencies)
- Policy clauses must be expressed in clear, unambiguous language
- All authority must be explicitly named (SYSTEM, USER, SERVICE)
- Cost clauses must include explicit measurement units (USD, EUR, tokens, etc.)

## Limitations
See LIMITATIONS.md for detailed information about current implementation constraints.

## Release Notes
- v1.0.0 - Initial Release
- Complete policy compilation pipeline with deterministic outputs
- Full backward auditability through exact traceability mapping
- Semantic validation for modal language, multi-actions, and missing verbs
- Explicit authority requirements (SYSTEM, USER, SERVICE)
- Cost constraint validation with explicit unit requirements
- Production-ready implementation with comprehensive test coverage