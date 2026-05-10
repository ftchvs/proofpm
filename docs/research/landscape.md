# Research Landscape: AI Product Management CLI / Agents

_Last researched: 2026-05-10_

## Market signal

AI-native product-management tooling is emerging around three clusters:

1. **Prompt/skill packs** for Claude/Cursor/Gemini that encode PM frameworks.
2. **Planning CLIs** that turn ideas or PRDs into epics/stories/tasks.
3. **Issue/backlog automation** that labels, deduplicates, prioritizes, and syncs with trackers.

The strongest opportunity for ProofPM is not “generate another PRD.” The gap is **evidence-backed judgment**: product recommendations that are traceable, challengeable, and decision-ready.

## References

### nmrtn/nanopm

- URL: https://github.com/nmrtn/nanopm
- Positioning: autonomous product management for Claude Code; audit → strategy → roadmap → PRD → tickets.
- Notable strengths: opinionated product pipeline, persistent markdown artifacts, adversarial tone, methodology adaptation.
- Gap for ProofPM: nanopm is strong on PM workflow generation, but the visible wedge is not a structured evidence graph with confidence/counter-evidence as the primitive.

### google-gemini/gemini-cli PM Agent Extension proposal

- URL: https://github.com/google-gemini/gemini-cli/issues/20503
- Positioning: PM extension for OSS projects; analyze issues, PRs, commits, docs, roadmap; generate PM outputs.
- Notable strengths: validates enterprise need for OSS projects to generate PRDs/roadmaps/risk assessments; proposes MCP tools for issue analysis and commit velocity.
- Gap for ProofPM: extension-specific, MCP/agent focused; ProofPM can be CLI-first and tool-agnostic.

### openplanr/OpenPlanr

- URL: https://github.com/openplanr/OpenPlanr
- Positioning: AI-powered planning CLI; backlog, epics, stories, tasks, sprint planning, GitHub sync, agent rules.
- Notable strengths: concrete CLI workflow, file-based planning artifacts, agent context rules.
- Gap for ProofPM: OpenPlanr is a planning system. ProofPM should be a decision system: signal synthesis, prioritization rationale, confidence, counter-evidence, and assumption testing.

### comerito/cezar

- URL: https://github.com/comerito/cezar
- Positioning: GitHub backlog triage CLI; duplicates, labels, missing info, stale cleanup, priority score, release notes.
- Notable strengths: focused maintainer pain, offline-first JSON store, compact digests, interactive review.
- Gap for ProofPM: Cezar optimizes issue hygiene. ProofPM connects issue data to product strategy and roadmap decisions.

### joa23/linear-cli and schpet/linear-cli

- URLs: https://github.com/joa23/linear-cli, https://github.com/schpet/linear-cli
- Positioning: token-efficient or agent-friendly Linear CLIs.
- Notable strengths: Linear as coordination layer, agent-readable workflows, dependency analysis.
- Gap for ProofPM: These manage Linear well; ProofPM should integrate with Linear but not compete as a Linear client.

### pm-skills / product-manager-agent forks

- URLs: https://github.com/DanielQuanli/pm-skills, https://github.com/abshapsough/product-manager-agent
- Positioning: PM skills marketplace; discovery, strategy, execution, launch, growth workflows.
- Notable strengths: broad PM methodology coverage.
- Gap for ProofPM: prompt/skill-pack orientation; ProofPM should operate over real data and produce auditable decision artifacts.

### aakashg/pm-planning-system

- URL: https://github.com/aakashg/pm-planning-system
- Positioning: structured planning docs that live next to code; PM PR process for the AI era.
- Notable strengths: version-controlled specs and engineering review loop.
- Gap for ProofPM: artifact governance, not automated evidence synthesis.

### GrowthBook / PostHog / Langfuse pattern

- GrowthBook: https://github.com/growthbook/growthbook
- PostHog: https://github.com/PostHog/posthog
- Langfuse: https://github.com/langfuse/langfuse
- Signal: mature OSS tooling increasingly adds AI assistants over product/LLM analytics.
- Implication: ProofPM should not try to become analytics infrastructure. It should ingest exports/API results and focus on PM judgment.

## Strategic whitespace

### What not to build

- Another generic PM prompt pack.
- Another Linear CLI.
- Another PRD-to-ticket converter.
- Another full PM OS that tries to own every workflow.

### What to build

A CLI that answers:

> “Given the evidence we have, what product decision should we make, how confident are we, and what would change our mind?”

## Differentiation thesis

ProofPM should define a new product category: **evidence-backed AI PM**.

The durable wedge is the `Decision Brief`:

```md
Recommendation: Fix onboarding import confusion before building team invites.
Confidence: 0.74
Evidence:
- 11 GitHub issues mention import failures
- 7 support tickets mention first-session confusion
- activation export shows 38% drop after import step
Counter-evidence:
- team invites requested by 2 enterprise prospects
Assumptions:
- import success is a prerequisite to activation
- the observed dropoff is product friction, not poor traffic quality
Next test:
- instrument import error taxonomy and run 5 usability sessions
```

