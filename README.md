# Central AI Research Wiki

This repository is an Obsidian-based central AI research wiki. It is designed as
a persistent, compounding knowledge base that sits between raw research sources
and day-to-day research questions.

The goal is not to collect isolated summaries. When a source is added, the wiki
integrates it into topics, concepts, claims, comparisons, contradictions, open
questions, synthesis, the root index, and the chronological log.

## Repository Layers

- `raw/` is the source-of-truth layer. It contains immutable papers, articles,
  reports, transcripts, assets, datasets, and finalized experiment reports. Read
  from it, but do not edit it unless explicitly requested.
- `wiki/` is the LLM-maintained synthesis layer. It contains generated pages for
  topics, papers, concepts, entities, methods, models and systems, datasets and
  benchmarks, metrics, claims, comparisons, experiments, literature reviews,
  open questions, outputs, overview, and synthesis.
- `AGENTS.md` is the schema and operating manual. It defines task
  classification, ingest, query, lint, maintenance, experiment-result ingest,
  source classification, indexing, logging, and approval rules.
- `index.md` is the content-oriented catalog.
- `log.md` is the chronological activity record.

## Ingesting A Source

1. Place one source in `raw/inbox/` or `raw/sources/`.
2. Ask the agent to ingest it, for example:
   `Ingest raw/inbox/example-ai-paper.md`.
3. The agent reads `AGENTS.md`, `index.md`, recent `log.md`, and the source.
4. The agent creates or updates the paper page, topic pages, concept pages,
   claim pages, comparison pages, open questions, contradictions, synthesis,
   index, and log as needed.

Prefer one-source-at-a-time ingest. Batch ingest requires a plan and explicit
confirmation before broad edits.

## Querying The Wiki

Ask a research question in natural language. The agent reads `index.md` first,
then relevant wiki pages, and answers with citations to wiki pages, paper pages,
source pages, or experiment result pages. If the answer becomes durable
knowledge, ask the agent to save it back into the wiki.

## Linting The Wiki

Ask for a lint pass to check broken links, missing frontmatter, orphan pages,
missing index entries, stale or unsupported claims, repeated concepts,
contradictions, and research gaps. Lint reports live under
`00_Meta/Lint Reports/`.

## Ingesting Finalized Experiment Results

Experiments are executed outside this wiki, usually in an AI-Experiments repo.
Only finalized results should be ingested. Place finalized reports under
`raw/finalized-experiments/` or point the agent at the finalized run. The agent
creates an experiment result page under `wiki/experiments/` and updates related
claims, methods, metrics, comparisons, synthesis, index, and log.

## Git Review

Treat this wiki as a Git repo of markdown files. Before broad edits, use a
topic branch such as `ingest/<source-slug>`, `lint/<scope-date>`,
`maintenance/<scope-date>`, `synthesis/<topic-or-thesis>`,
`experiment/<experiment-id>`, or `schema/<change>`.

After edits, review the changed files and the relevant `git diff`. The agent
should explain what changed, what to review, and recommend a commit message.

