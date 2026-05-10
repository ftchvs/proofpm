# PRD: ProofPM — Evidence-Backed Product Manager CLI

## 1. Summary

ProofPM is a CLI-first, open-source AI product manager that helps founders, PMs, and engineering-led teams make better product decisions from scattered evidence.

It ingests product signals from GitHub, Linear, feedback files, analytics exports, and competitor notes; builds a local evidence graph; and generates decision artifacts such as prioritized opportunities, PRDs, roadmap proposals, experiment briefs, and stakeholder summaries.

The core product promise:

> ProofPM does not just write product docs. It explains why a product decision is worth making — with citations, confidence, counter-evidence, assumptions, and next tests.

## 2. Problem

Small teams increasingly rely on AI tools to write PRDs, tickets, and roadmaps. But AI-generated product artifacts often have three problems:

1. **They are ungrounded** — outputs sound plausible but do not cite real product signals.
2. **They hide uncertainty** — recommendations rarely expose assumptions, counter-evidence, or confidence.
3. **They do not compound** — decisions, learnings, rejected ideas, and past assumptions are scattered across docs, Linear, GitHub, Slack, analytics, and memory.

As a result, teams may produce more PM artifacts while still making weak prioritization decisions.

## 3. Target users

### Primary: founder / engineer acting as PM

- Has GitHub and maybe Linear.
- Has messy feedback and feature requests.
- Wants to decide what to build next without installing a heavy PM platform.
- Values speed, clarity, and artifact quality.

### Secondary: PM at a small B2B SaaS team

- Needs to synthesize issues, feedback, analytics, and sales signals.
- Writes PRDs and roadmap updates.
- Wants evidence-backed prioritization and stakeholder-ready summaries.

### Tertiary: OSS maintainer

- Has issues, discussions, PRs, commits, and roadmap docs.
- Needs PM-quality planning artifacts for contributors or enterprise users.

## 4. Goals

### User goals

- Understand what product opportunities are supported by evidence.
- Prioritize backlog items with transparent rationale.
- Generate PRDs that cite source evidence.
- Create roadmap proposals with confidence and risks.
- Identify assumptions and cheapest next tests.

### Business / portfolio goals

- Demonstrate strong product judgment, AI tooling taste, and CLI craftsmanship.
- Build a useful OSS project with a clear niche and demoable workflow.
- Create a future path to MCP, GitHub Action, and hosted dashboard without needing them for v1.

## 5. Non-goals

For v1, ProofPM will not:

- Replace Linear, GitHub Issues, Jira, Productboard, PostHog, or GrowthBook.
- Automatically write external comments, update issues, or change roadmap state without explicit user action.
- Claim statistical truth from weak data.
- Run autonomous coding agents.
- Become a full project-management UI.

## 6. Core product concept

ProofPM operates through five stages:

```text
ingest → normalize → evidence graph → decision engine → artifacts
```

### 6.1 Ingest

Sources:

- GitHub issues, PRs, commits, labels, milestones.
- Linear projects/issues/cycles.
- CSV/Markdown/JSON feedback files.
- Analytics exports from PostHog, Mixpanel, Amplitude, GrowthBook, or custom CSV.
- Competitor notes or scraped page snapshots in Markdown.

### 6.2 Normalize

Convert source records into a canonical `Signal` format:

```ts
type Signal = {
  id: string;
  source: 'github' | 'linear' | 'feedback' | 'analytics' | 'competitor' | 'manual';
  sourceUrl?: string;
  title: string;
  excerpt: string;
  actor?: string;
  date?: string;
  tags: string[];
  strength: number;
  sentiment?: 'positive' | 'negative' | 'neutral';
  metadata: Record<string, unknown>;
}
```

### 6.3 Evidence graph

Cluster signals into `Opportunities`, link them to `Assumptions`, `Risks`, and `Artifacts`.

```ts
type Opportunity = {
  id: string;
  problem: string;
  userSegment?: string;
  evidenceIds: string[];
  counterEvidenceIds: string[];
  assumptions: Assumption[];
  confidence: number;
  impactEstimate?: number;
  effortEstimate?: number;
}
```

### 6.4 Decision engine

Scores opportunities with a transparent model:

- Evidence volume and diversity
- Recency
- User/business impact
- Strategic alignment
- Effort / uncertainty
- Counter-evidence weight
- Assumption risk

The score should be explainable, not magical.

### 6.5 Artifacts

Generate:

- Decision brief
- Prioritized opportunity list
- PRD
- Roadmap proposal
- Experiment brief
- Linear/GitHub issue draft
- Stakeholder update

## 7. MVP scope

### 7.1 Commands

#### `proofpm init`

Creates local config and data directories.

Output:

```text
.proofpm/
  config.yml
  signals.jsonl
  opportunities.json
  decisions/
  artifacts/
```

#### `proofpm ingest github <owner/repo>`

Pulls GitHub issues and PR metadata.

Options:

- `--since 90d`
- `--labels bug,feature,customer`
- `--state open|closed|all`
- `--dry-run`

#### `proofpm ingest file <path>`

Imports Markdown, CSV, or JSON feedback.

#### `proofpm signals`

Shows normalized signals by theme, source, and recency.

#### `proofpm decide "<question>"`

Generates a decision brief with recommendation, evidence, counter-evidence, confidence, assumptions, and next test.

#### `proofpm prioritize`

Ranks opportunities with score breakdown.

#### `proofpm prd "<opportunity or question>"`

Generates a PRD grounded in selected evidence.

#### `proofpm roadmap --horizon 6w`

Generates a NOW/NEXT/LATER roadmap proposal.

#### `proofpm export linear --dry-run`

Creates Linear-ready issue/project payloads but does not write by default.

### 7.2 MVP data sources

MVP should support:

1. GitHub Issues/PRs via `gh` or GitHub API.
2. Local Markdown/CSV/JSON files.
3. Manual signals via CLI.

Linear and analytics exports can be phase 2 if needed, but docs should design for them from day one.

## 8. User stories

### Ingest

- As a founder, I can run `proofpm ingest github owner/repo` and get a local product signal cache.
- As a PM, I can import a CSV of support tickets and map columns to signal fields.
- As an OSS maintainer, I can ingest issues without sending private issue content to an LLM unless I opt in.

### Decision making

- As a user, I can ask “what should we build next?” and receive ranked opportunities with citations.
- As a user, I can see counter-evidence and assumptions so I do not overtrust AI output.
- As a user, I can override scoring weights in config.

### PRD generation

- As a PM, I can generate a PRD from an opportunity and see source evidence embedded as citations.
- As an engineer, I can inspect acceptance criteria and know what evidence motivated them.

### Export

- As a user, I can preview Linear/GitHub issues before any write happens.
- As a user, I can export Markdown artifacts into my repo for version control.

## 9. UX principles

1. **CLI-first, artifact-native** — works in terminal and writes useful Markdown.
2. **Dry-run by default for external writes** — user approves writes explicitly.
3. **Citations everywhere** — no recommendation without source references.
4. **Challenge the premise** — always include counter-evidence and assumptions.
5. **Local-first** — cache source data locally and allow offline analysis.
6. **Human-in-control** — ProofPM recommends; humans decide.

## 10. Example output

```md
# Decision Brief: Improve import onboarding before team invites

## Recommendation

Prioritize fixing import onboarding friction before building team invites.

## Confidence

0.74 — moderate/high

## Why

The import step appears to be the highest-confidence activation blocker. Evidence appears across GitHub issues, support tickets, and analytics export.

## Evidence

1. GitHub issue #42: “CSV import fails silently on malformed rows.”
2. Support ticket 2026-04-18: “I could not tell whether my data uploaded.”
3. Activation export: 38% drop from signup → first successful import.

## Counter-evidence

1. Two enterprise prospects requested team invites.
2. Team invites may influence expansion revenue more than import quality.

## Assumptions

- Import success is a prerequisite to activation for target users.
- The observed dropoff is caused by UX/product friction, not low-intent traffic.
- Fixing import feedback/error handling is lower effort than team invites.

## Next test

Run 5 moderated import usability sessions and instrument import error taxonomy.

## Suggested PRD

Create PRD with scope limited to import feedback, error taxonomy, and success-state clarity.
```

## 11. Functional requirements

### FR1: Project initialization

- Create `.proofpm/config.yml`.
- Store data locally in JSONL/SQLite.
- Detect GitHub repo from current directory when possible.

### FR2: Source connectors

- GitHub connector must support issues and PRs.
- File connector must support Markdown, CSV, JSON.
- Connectors must preserve source URL/file path for citations.

### FR3: Signal normalization

- Extract title, excerpt, source, tags, date, and metadata.
- Generate deterministic IDs.
- Preserve raw source references.

### FR4: Theme clustering

- Group similar signals into opportunities.
- MVP may use embeddings when LLM/API key exists, and fallback keyword clustering when absent.

### FR5: Decision scoring

- Provide score and score breakdown.
- Include evidence strength, diversity, recency, counter-evidence, and assumption risk.
- Allow configurable weights.

### FR6: Artifact generation

- Generate Markdown artifacts.
- Every artifact must include source citations.
- PRD must include problem, goals, non-goals, users, requirements, metrics, rollout, risks, and open questions.

### FR7: Privacy and safety

- Default local mode should avoid sending content to external LLMs unless configured.
- Redaction hooks for emails, names, account IDs, and secrets.
- External writes require `--yes`.

## 12. Non-functional requirements

- Installable as a single CLI package.
- Fast enough for repos with 500 issues.
- Works without a hosted backend.
- Human-readable local storage.
- Provider-agnostic LLM layer.
- Good docs and examples from day one.

## 13. Technical approach

Recommended stack for v1:

- TypeScript CLI using `commander` or `oclif`.
- SQLite or JSONL for local store.
- GitHub via `gh` first for auth simplicity, then Octokit.
- LLM provider abstraction: OpenAI-compatible endpoint, Anthropic, local Ollama.
- Markdown artifact renderer.
- Optional MCP server later using the same core engine.

## 14. Differentiators

### 14.1 Evidence graph as the core primitive

Existing tools usually start with artifacts: PRD, roadmap, stories. ProofPM starts with evidence and then generates artifacts.

### 14.2 Counter-evidence by default

Every recommendation must include what argues against it.

### 14.3 Assumption ledger

ProofPM tracks assumptions over time:

- assumption
- linked opportunity
- confidence
- test
- status: untested / testing / validated / invalidated

### 14.4 Decision memory

Past decisions are stored as Markdown/JSON so future runs can compare:

- what we recommended
- what we believed
- what evidence existed
- what happened later

### 14.5 CLI-first, MCP-ready

ProofPM is useful without MCP, but the core engine should expose functions that can later power an MCP server.

## 15. Success metrics

### OSS metrics

- 100 GitHub stars within 60 days of public launch.
- 10 external users try the CLI.
- 3 external issues/feature requests.
- 1 external contributor.

### Product metrics

- User can produce first decision brief in under 5 minutes.
- At least 80% of generated recommendations contain 3+ citations.
- Users report the output helped clarify a real product decision.

### Quality metrics

- Deterministic parser tests for connectors.
- Golden-file tests for artifact output.
- No external writes without `--yes`.

## 16. Risks

| Risk | Severity | Mitigation |
| --- | --- | --- |
| Too similar to nanopm/OpenPlanr | High | Position around evidence graph and decision briefs, not planning pipeline |
| LLM output hallucination | High | Require citations, expose confidence, support deterministic fallback |
| Connector scope creep | Medium | Start with GitHub + files only |
| Users expect full PM platform | Medium | Keep CLI focused on decisions/artifacts |
| Private data leakage | High | Local-first, redaction, explicit LLM opt-in |

## 17. Open questions

1. Should v1 use TypeScript or Python?
2. Should GitHub ingestion use `gh` shell-out first or Octokit OAuth from day one?
3. Should the default local store be JSONL for transparency or SQLite for querying?
4. Should the first artifact be `Decision Brief` or `PRD`?
5. Should the public repo include a fake demo dataset from day one?

## 18. Launch plan

### Pre-launch

- Build MVP with GitHub + file ingestion.
- Include example repo dataset.
- Record terminal GIF.
- Write “AI PM tools are making docs cheaper, not decisions better” launch essay.

### Launch channels

- GitHub README
- Hacker News Show HN
- Product Hunt
- LinkedIn post
- Relevant communities: PM, indie hackers, OSS maintainers, AI agent builders

### Demo narrative

1. Ingest a real GitHub repo.
2. Ask what to build next.
3. Show ranked opportunities with citations and counter-evidence.
4. Generate PRD.
5. Export Linear/GitHub issue draft with dry-run.

