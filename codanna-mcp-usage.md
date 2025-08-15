# Codanna MCP Tools Quick Reference

## Tool Selection

### Initial Exploration
- `get_index_info`: First interaction, understand codebase scope

### Finding Symbols
- `find_symbol`: Exact name lookup (try first)
- `search_symbols`: Fuzzy matching (fallback)

### Concept Search
- `semantic_search_docs`: Initial exploration of features/concepts
- `semantic_search_with_context`: Deep dive with full context

### Code Relationships
- `get_calls`: What a function calls
- `find_callers`: What calls a function
- `analyze_impact`: Change impact analysis

## Common Workflows

### "How is X implemented?"
1. `semantic_search_docs("X feature", limit=5)`
2. `semantic_search_with_context("X", limit=1)` for best match

### "What does function Y do?"
1. `find_symbol("Y")`
2. `get_calls("Y")` + `find_callers("Y")`

### "Where is Z used?"
1. `find_symbol("Z")`
2. `find_callers("Z")`
3. `analyze_impact("Z")` for full dependency tree

### "Understand error/bug"
1. `semantic_search_docs("error message", limit=5)`
2. `find_callers()` on relevant symbols
3. Trace with `get_calls()`

### "Refactor impact"
1. `find_symbol("target")`
2. `analyze_impact("target", max_depth=2)`

## Key Tips
- Start with `semantic_search_docs` for vague queries
- Use `find_symbol` for known names (faster)
- Chain tools: broad → narrow → deep
- Max depth 2-3 for `analyze_impact` (performance)
- `semantic_search_with_context` = comprehensive one-shot analysis