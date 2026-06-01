# Central AI Research Wiki Agent Instructions

These instructions govern this repository as an Obsidian-based central AI
research wiki. Treat them as the operating schema for every session in this
project.

## Core Operating Loop

For every request, first classify the task type as exactly one of:

1. setup
2. ingest
3. query
4. lint
5. maintenance
6. experiment-result-ingest
7. synthesis-update
8. refactor-wiki
9. research-gap-analysis
10. output-generation
11. schema-update
12. source-classification

Before making edits:

- Read `AGENTS.md`.
- Read root `index.md`.
- Read recent root `log.md` entries.
- Identify relevant source files, wiki pages, topics, concepts, claims,
  contradictions, and synthesis pages.
- Briefly summarize the planned changes.

After making edits:

- Update `index.md`.
- Append `log.md`.
- Add or update cross-links.
- Add source references.
- Flag contradictions instead of silently resolving them.
- Create TODOs for uncertain claims.
- Show changed files.
- Show what the human should review in `git diff`.
- Recommend a commit message.

## A. Purpose

This repository is a central AI research wiki. It is not a generic notes folder
and not a basic RAG store. It is a persistent, compounding markdown knowledge
base that sits between the human researcher and raw sources.

The wiki should become richer every time a source is ingested, a research
question is answered, a lint pass is run, a synthesis is updated, or a finalized
experiment result is transferred in.

The human curates sources, directs analysis, asks questions, reviews changes,
and decides when broad edits, merges, deletions, or schema changes are
appropriate.

The LLM maintains the wiki layer by summarizing sources, filing pages,
cross-referencing concepts, updating indexes and logs, tracking claims,
recording contradictions, identifying open questions, linting structure, and
keeping the synthesis current.

## B. Architecture

- `raw/` is immutable source material. It contains papers, articles, reports,
  transcripts, datasets, images, and finalized experiment reports. Read from it
  freely, but do not modify it unless explicitly asked.
- `wiki/` is the LLM-maintained synthesis layer. It contains generated pages for
  topics, papers, concepts, entities, methods, models and systems, datasets and
  benchmarks, metrics, claims, comparisons, experiments, literature reviews,
  open questions, outputs, overview, and synthesis.
- `AGENTS.md` is the schema and operating manual for the wiki.
- `index.md` is the content-oriented catalog. It helps the human and the LLM
  navigate the knowledge base before search infrastructure is needed.
- `log.md` is the chronological activity record. It is append-only and
  parseable.
- Experiments are executed outside the wiki. Only finalized experiment results
  are ingested into this repository.

## C. Folder Conventions

Canonical project structure:

```text
AI-Research-Wiki/
  AGENTS.md
  README.md
  index.md
  log.md
  .gitignore
  00_Meta/
  raw/
  wiki/
  tools/

raw/
  inbox/
  sources/
  assets/
  finalized-experiments/

wiki/
  overview.md
  synthesis.md
  topics/
  papers/
  concepts/
  entities/
  methods/
  models-and-systems/
  datasets-and-benchmarks/
  metrics/
  claims/
  comparisons/
  experiments/
  literature-reviews/
  open-questions/
  outputs/

00_Meta/
  Wiki Operating Manual.md
  Source Intake Queue.md
  Research Questions.md
  Claim Register.md
  Contradictions.md
  Lint Reports/
  Maintenance Reports/
  Templates/
```

Do not create top-level topic folders such as `AI Safety/` or `LLM Training/`.
Represent subjects as pages under `wiki/topics/`, plus frontmatter values and
tags.

## D. Page Type Conventions

- Topic page: A durable subject hub that connects papers, concepts, methods,
  claims, comparisons, open questions, and synthesis relevant to an area.
- Paper page: A source-specific page for a paper, article, report, transcript,
  or other source. It records bibliographic metadata, thesis, claims, methods,
  evidence, limitations, and links into the wiki.
- Concept page: A reusable definition and explanation of an idea that appears
  across sources or topics.
- Entity page: A page for an organization, lab, author, company, benchmark
  maintainer, project, or other named actor.
- Method page: A page for a research method, algorithmic approach, evaluation
  method, training procedure, analysis technique, or engineering practice.
- Model/system page: A page for a model, agent system, framework, product,
  architecture, or deployed AI system.
- Dataset/benchmark page: A page for a dataset, task suite, benchmark, corpus,
  evaluation set, or leaderboard.
- Metric page: A page for a measurement, score, evaluation statistic, cost
  measure, reliability measure, or human-evaluation rubric.
- Claim page: A single important proposition with support, caveats,
  contradictions, confidence, and source grounding.
- Comparison page: A structured comparison between concepts, methods, models,
  systems, benchmarks, claims, or papers.
- Experiment result page: A wiki-level summary of a finalized external
  experiment, including result metadata, metrics, interpretation, and links to
  related claims and synthesis.
- Literature review page: A curated review of a research area across multiple
  sources.
- Open question page: A durable research question, uncertainty, or investigation
  thread.
- Output page: A durable generated artifact such as a briefing, slide outline,
  memo, chart explanation, or answer worth preserving.
- Synthesis page: A high-level, cross-source interpretation of what the wiki
  currently believes, where confidence is strong, and where uncertainty remains.
- Overview page: A concise orientation to the wiki's current coverage, major
  areas, and how to navigate.

## E. Naming Conventions

- Use lowercase kebab-case filenames for generated wiki pages.
- Keep titles human-readable in the H1.
- Use stable filenames. Do not rename pages casually.
- Use dates for reports and experiment results:
  `YYYY-MM-DD-title.md`.
- If duplicates exist, propose a merge rather than silently deleting either
  page.
- Prefer descriptive slugs over vague filenames such as `notes.md` or
  `summary.md`.
- Use Obsidian links with the page title or basename where practical, and keep
  aliases readable when needed.

## F. Frontmatter Conventions

Every generated wiki page should include YAML frontmatter:

```yaml
---
type:
status: draft
created:
updated:
topics: []
source_refs: []
related: []
confidence: low
tags: []
---
```

Allowed status values for normal wiki pages:

- `draft`
- `active`
- `stable`
- `stale`
- `archived`

Allowed confidence values:

- `low`
- `medium`
- `high`

Additional fields for paper pages:

```yaml
title:
authors: []
year:
venue:
url:
raw_path:
```

Additional fields for experiment result pages:

```yaml
experiment_id:
source_repo:
run_path:
code_commit:
dataset:
dataset_hash:
metrics_file:
status: finalized
```

## G. Citation And Source Rules

- Every factual claim should be grounded in a source page, raw source, or
  finalized experiment result.
- Use `source_refs` in frontmatter.
- In body text, link to the relevant paper page, source page, claim page, or
  experiment page.
- Mark unsupported claims with `SOURCE_NEEDED`.
- Mark uncertain claims with explicit uncertainty notes.
- Do not invent citations or paper claims.
- If a source is weak, speculative, outdated, or contradicted, mark confidence
  accordingly.
- Prefer linking to source-specific paper pages rather than repeating source
  metadata everywhere.
- Distinguish what a source claims from what the wiki concludes.

## H. Ingest Workflow

Use this workflow when the classified task type is `ingest`.

1. Confirm the source is placed in `raw/inbox/` or `raw/sources/`.
2. Prefer one-source-at-a-time ingest unless the human explicitly requests batch
   ingest.
3. For batch ingest, summarize the proposed plan first and ask for confirmation
   before broad edits.
4. Read `AGENTS.md`, `index.md`, recent `log.md`, and the source.
5. Identify relevant existing topics, papers, concepts, entities, methods,
   models/systems, datasets/benchmarks, metrics, claims, comparisons,
   contradictions, open questions, and synthesis pages.
6. Discuss or summarize key takeaways for human review when appropriate.
7. Extract thesis, claims, methods, datasets, benchmarks, metrics, limitations,
   assumptions, contradictions, related concepts, and open questions.
8. Create a paper/source summary page under `wiki/papers/`.
9. Update relevant topic pages under `wiki/topics/`.
10. Update relevant concept pages under `wiki/concepts/`.
11. Update relevant entity, method, model/system, dataset/benchmark, metric,
    claim, and comparison pages where useful.
12. Update `wiki/synthesis.md` if the source changes the overall understanding.
13. Update `00_Meta/Contradictions.md` if the source conflicts with prior
    claims.
14. Update `00_Meta/Research Questions.md` if the source suggests new
    investigations.
15. Update `index.md`.
16. Append `log.md`.

One source may touch 10-15 wiki pages when integration is useful. That is
expected. Do not merely summarize a source and stop.

## I. Query Workflow

Use this workflow when the classified task type is `query`.

1. Read `index.md` first.
2. Identify relevant pages.
3. Read the relevant pages before answering.
4. Synthesize an answer with citations to wiki pages, paper pages, source pages,
   or experiment result pages.
5. Return the answer in the requested format when specified. Acceptable formats
   include markdown, comparison table, literature review, chart, Marp slide
   deck, canvas outline, or another requested format.
6. If the answer produces a valuable comparison, analysis, connection, or
   synthesis, offer to save it back into the wiki.
7. If the human asks to save the answer, create the appropriate page, update
   cross-links, update `index.md`, and append `log.md`.

Do not let useful research disappear into chat history if it should become part
of the wiki.

## J. Lint Workflow

Use this workflow when the classified task type is `lint`.

Periodically health-check the wiki. Check mechanical issues:

- Broken links.
- Missing frontmatter.
- Pages missing from `index.md`.
- `index.md` entries pointing to missing pages.
- Duplicate titles.
- Empty pages.
- Raw files accidentally modified.

Check structural issues:

- Orphan pages.
- Missing cross-references.
- Repeated concepts.
- Concepts mentioned often but lacking their own page.
- Paper pages not linked to concepts or methods.
- Experiment pages not linked to claims or synthesis.

Check semantic issues:

- Contradictions.
- Stale claims superseded by newer sources.
- Unsupported claims.
- Overconfident claims.
- Unclear definitions.
- Missing caveats.
- Missing limitations.
- Research gaps.

Write lint reports under `00_Meta/Lint Reports/`.

Make safe fixes only:

- Fix links.
- Update `index.md`.
- Add missing cross-links.
- Add TODO markers.
- Add `SOURCE_NEEDED` markers.

Do not delete, merge, or heavily rewrite pages without confirmation. Append
`log.md` after every lint pass.

## K. Maintenance Workflow

Use this workflow when the classified task type is `maintenance`.

1. Read recent `log.md` entries.
2. Review `00_Meta/Contradictions.md`.
3. Review `00_Meta/Claim Register.md`.
4. Review unresolved TODOs and `SOURCE_NEEDED` markers.
5. Identify stale pages and weakly supported claims.
6. Identify missing concepts, missing topic pages, and orphan pages.
7. Suggest a maintenance plan before broad edits.
8. Apply safe maintenance updates.
9. Update `index.md` and `log.md`.
10. Write a maintenance report under `00_Meta/Maintenance Reports/` when changes
    are substantial.

Maintenance should improve navigability, evidence quality, and synthesis
freshness without hiding uncertainty.

## L. Indexing Rules

- `index.md` is content-oriented.
- It catalogs wiki pages by category.
- Each entry should include a wiki link, one-line summary, and useful metadata
  such as status, date, source count, or topic.
- `index.md` must be updated after every ingest, saved query, lint pass,
  refactor, experiment-result ingest, and major maintenance update.
- When answering a query, read `index.md` first, then drill into relevant pages.
- Keep `index.md` useful at moderate scale before adding search
  infrastructure.
- Prefer concise entries over exhaustive page excerpts.

## M. Logging Rules

- `log.md` is chronological and append-only.
- It records setup, ingest, query, lint, maintenance, experiment-result ingest,
  synthesis updates, refactors, and schema updates.
- Every entry must use this parseable heading format:

```md
## [YYYY-MM-DD] <type> | <title>
```

Valid log types:

- `setup`
- `ingest`
- `query`
- `lint`
- `maintenance`
- `experiment-result`
- `synthesis`
- `refactor`
- `schema`
- `output`

Each entry should include scope, sources, pages changed, key decisions,
follow-ups, and recommended commit message.

Keep the heading prefix consistent so commands like this work:

```powershell
Select-String -Path log.md -Pattern '^## \[' | Select-Object -Last 5
```

## N. Experiment-Result Ingest Workflow

Use this workflow when the classified task type is
`experiment-result-ingest`.

- Experiments are run outside this wiki, usually in an AI-Experiments repo.
- Ingest only finalized experiment results.
- Validate `manifest.json` if available.
- Read `final_report.md`, `metrics.json`, selected charts, and representative
  traces only if needed.
- Do not copy full raw traces into the wiki by default.
- Create an experiment result page under `wiki/experiments/`.
- Update related concept, method, metric, claim, comparison, and synthesis
  pages.
- Update `00_Meta/Contradictions.md` if results conflict with prior claims.
- Update `index.md` and `log.md`.

The wiki records conclusions, evidence, and links back to the finalized run; it
does not become the execution environment.

## O. Source Classification And New-Topic Workflow

Use this workflow when the classified task type is `source-classification`, and
also inside ingest when a source does not fit existing topics.

- If a source does not fit existing topics, create a new topic page under
  `wiki/topics/`.
- Do not create new top-level topic folders without confirmation.
- A source may belong to multiple topics.
- Use tags and frontmatter for topic membership.
- Update related concept and method pages across topic boundaries.
- Record classification rationale when it is non-obvious.
- If classification is uncertain, mark it as TODO and ask for human review.

## P. Research Automation Workflow

- Source collection may be automated, but source selection should be reviewed by
  the human unless batch automation is explicitly approved.
- Automated research should place sources in `raw/inbox/` first.
- The LLM should classify sources, propose priority, and ask before ingesting
  large batches.
- The LLM may suggest research gaps, search queries, and new sources during lint
  or maintenance.
- Automated tasks must preserve raw source files and should avoid broad wiki
  edits without a reviewed plan.

## Q. Git Review Workflow

Treat the wiki as a Git repo of markdown files.

Before broad changes, recommend a branch name:

- `ingest/<source-slug>`
- `lint/<scope-date>`
- `maintenance/<scope-date>`
- `synthesis/<topic-or-thesis>`
- `experiment/<experiment-id>`
- `schema/<change>`

After changes:

- Show changed files.
- Explain why each changed.
- Recommend a commit message.
- Never hide broad edits.
- Do not delete, merge, or heavily rewrite pages without confirmation.
- Preserve `raw/` files.

## R. Obsidian Conventions

- Use internal links with `[[page-name]]` where practical.
- Use YAML frontmatter for Dataview-friendly metadata.
- Keep graph view useful by cross-linking concepts, topics, papers, methods,
  claims, and experiments.
- Store local images under `raw/assets/` or a clearly designated assets folder.
- Use Marp-compatible markdown for slide outputs when requested.
- Keep page titles human-readable even when filenames are lowercase kebab-case.
- Prefer wiki links in body text and path references in frontmatter.

## S. Output Generation Rules

- Outputs can include markdown pages, comparison tables, literature reviews,
  Marp slide decks, charts, and canvas outlines.
- If an output should become durable knowledge, save it under `wiki/outputs/`,
  `wiki/comparisons/`, or `wiki/literature-reviews/` as appropriate.
- Update `index.md` and `log.md` when an output is saved.
- Generated outputs should cite the wiki pages, source pages, paper pages, or
  experiment pages they rely on.
- If an output is exploratory and unsupported, mark `SOURCE_NEEDED` and keep
  confidence low.

## T. Approval Rules

Ask before:

- Deleting files.
- Merging pages.
- Moving many pages.
- Rewriting `wiki/synthesis.md` heavily.
- Modifying `raw/`.
- Batch ingesting many sources.

Use judgment for safe, narrow edits such as adding cross-links, fixing broken
links, updating `index.md`, appending `log.md`, and adding TODO or
`SOURCE_NEEDED` markers.

