# ProofPM

> Evidence-backed product judgment from your terminal.

ProofPM is an open-source AI product manager CLI that reads product signals from GitHub, Linear, customer feedback, analytics exports, and competitive research, then produces prioritized decisions with citations, confidence, assumptions, and counter-evidence.

Most AI PM tools generate artifacts. ProofPM is different: it builds an **evidence graph** first, then generates PRDs, roadmaps, experiment briefs, and executive updates from traceable evidence.

## Why ProofPM

Product teams do not need more AI-generated documents. They need better judgment:

- Which problems are actually supported by evidence?
- Which roadmap items are high-conviction vs. loud but weakly supported?
- What assumptions must be tested before committing engineering time?
- What counter-evidence would make us change our mind?
- What should go into Linear/GitHub as execution-ready work?

ProofPM turns scattered product signals into decision-ready artifacts.

## Example CLI

```bash
proofpm init
proofpm ingest github ftchvs/proofpm
proofpm ingest linear --project "ProofPM"
proofpm ingest feedback ./examples/support-tickets.csv
proofpm signals
proofpm decide "what should we build next?"
proofpm prd "reduce onboarding dropoff"
proofpm roadmap --horizon 6w
proofpm export linear --dry-run
```

## Differentiation

| Category | Existing tools | ProofPM |
| --- | --- | --- |
| AI planning CLIs | Generate epics/stories/tasks | Builds evidence-backed decisions first |
| PM prompt/skill packs | Framework guidance | Runs as a CLI over real product data |
| Issue triage tools | Label/prioritize issues | Connects issues to user impact, strategy, assumptions, and confidence |
| Roadmap tools | Track chosen work | Helps decide what deserves to be chosen |
| MCP-first tools | Assistant-dependent | CLI-first, MCP later |

## Core principle

Every recommendation must include:

1. **Evidence** — source citations and excerpts
2. **Confidence** — how strong the signal is
3. **Counter-evidence** — what argues against the recommendation
4. **Assumptions** — what must be true
5. **Next test** — cheapest way to reduce uncertainty

## Status

Pre-MVP planning repo. See:

- [`docs/prd/PRD.md`](docs/prd/PRD.md)
- [`docs/research/landscape.md`](docs/research/landscape.md)
- [`docs/architecture/system-design.md`](docs/architecture/system-design.md)
- [`docs/roadmap.md`](docs/roadmap.md)

## License

MIT
