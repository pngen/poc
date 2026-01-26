# POC Limitations

## Current Implementation Constraints

### Clause Parsing
- **Delimiter**: Only period (`.`) is recognized as clause terminator
- **Nested Structures**: No support for hierarchical policy structures
- **Cross-References**: Clauses cannot reference other clauses by ID

### Principal Detection
- **Fixed Set**: Only SYSTEM, USER, SERVICE are recognized principals
- **Case Sensitivity**: Principals must appear in uppercase in source text
- **Word Boundaries**: Detection uses whitespace tokenization; embedded matches (e.g., "SYSTEMWIDE") are rejected

### Measurement Units
- **Fixed Set**: USD, EUR, GBP, tokens, bytes, requests, hours
- **Currency Symbols**: $, €, £, ¥ are explicitly rejected (require spelled-out units)
- **Custom Units**: Not supported; extension requires code modification

### Modal Language Detection
- **Phrase Matching**: Uses substring containment, not grammatical analysis
- **False Positives**: Possible with words like "assembly" containing "as"
- **Fixed List**: should, may, where reasonable, as appropriate, could, might, possibly

### Action Verb Detection
- **Fixed Set**: must, shall, require, log, audit, record, deny, allow, enforce, track, exceed
- **No Negation Handling**: "must not" treated same as "must"
- **No Tense Analysis**: Past/future tense verbs not distinguished

### Multi-Action Ordering
- **Keywords**: Only "then", "before", "after" indicate explicit ordering
- **Conjunctions**: "and"/"or" without ordering keywords cause rejection
- **Sequence Notation**: No support for numbered steps or flow notation

### Cost Constraints
- **Ceiling Values**: Not parsed from text (always None)
- **Complex Expressions**: No support for formulas or conditional costs
- **Aggregation**: No support for "total", "per-unit", or time-windowed costs

### Traceability
- **Granularity**: One-to-many clause-to-artifact mapping only
- **No Versioning**: No support for policy version tracking
- **No Diff**: Cannot compare traceability between policy versions

## Thread Safety Guarantees

- **Stateless Design**: Compiler holds no mutable state; compile() is pure function
- **Inherent Thread Safety**: No locks required; all state is local to each compile() call
- **Clone Semantics**: Clone is trivial (no shared state to synchronize)

## Performance Characteristics

- **Time Complexity**: O(n * m) where n = clauses, m = average clause length
- **Space Complexity**: O(n) for artifacts plus O(n * m) for traceability
- **No Streaming**: Entire policy must fit in memory

## Future Considerations

1. **Extensible Principal Registry**: Plugin-based principal definitions
2. **Custom Unit Types**: Configuration-driven measurement units
3. **Policy DSL**: Structured policy language with formal grammar
4. **Incremental Compilation**: Recompile only changed clauses
5. **Semantic Analysis**: NLP-based intent extraction