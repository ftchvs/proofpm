# ProofPM System Design

## Architecture

```text
CLI commands
   ↓
Core engine
   ├── connectors
   │   ├── GitHub
   │   ├── files
   │   ├── Linear later
   │   └── analytics later
   ├── normalizer
   ├── local store
   ├── clustering / evidence graph
   ├── decision scorer
   └── artifact renderer
        ↓
Markdown / JSON / Linear dry-run / GitHub dry-run
```

## Modules

### `connectors/*`

Fetch raw data and return source records.

### `signals/*`

Normalize records into canonical `Signal` objects.

### `graph/*`

Cluster signals into opportunities and link assumptions/counter-evidence.

### `decision/*`

Score opportunities and generate decision briefs.

### `artifacts/*`

Render PRDs, roadmap proposals, experiment briefs, and stakeholder updates.

### `exports/*`

Export artifacts to Markdown, GitHub Issues, Linear issues, or JSON. External writes should be dry-run by default.

## Future MCP

The CLI should call core engine functions rather than embedding business logic in command handlers. Later MCP tools can wrap the same functions:

- `ingest_project_signals`
- `rank_product_opportunities`
- `generate_decision_brief`
- `generate_prd`
- `draft_linear_issues`

