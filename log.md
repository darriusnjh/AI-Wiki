# Central AI Research Wiki Log

## [2026-06-01] setup | Initialize Central AI Research Wiki

- Scope: Initialized the root schema, folder conventions, content map, meta
  registers, templates, and operating rules.
- Sources: No research sources ingested.
- Pages changed: `AGENTS.md`, `README.md`, `index.md`, `log.md`, `.gitignore`,
  `handover.md`, `wiki/overview.md`, `wiki/synthesis.md`, `00_Meta/`,
  `tools/`, `raw/`.
- Key decisions:
  - Use `raw/` as immutable source material and `wiki/` as the generated
    synthesis layer.
  - Keep topics as pages under `wiki/topics/`, not as top-level folders.
  - Require parseable log headings and a content-oriented root index.
  - Initialize Git for repository review.
- Created schema, folder conventions, index, log, templates, and operating
  rules.
- Follow-ups:
  - Add first source to `raw/inbox/`.
  - Run first ingest.
- Recommended commit message: `Initialize central AI research wiki`

## [2026-06-01] setup | Initialize GitHub Repository Remote

- Scope: Connect the local Central AI Research Wiki repository to
  `https://github.com/darriusnjh/AI-Wiki.git` and publish the initial wiki
  setup.
- Sources: No research sources ingested.
- Pages changed: `log.md`, `handover.md`, plus Git repository configuration.
- Key decisions:
  - Use `main` as the primary branch.
  - Use `origin` for the GitHub remote.
  - Leave the pre-existing `.obsidian/` directory unstaged unless vault config
    tracking is requested separately.
  - Configure repo-local Git identity as
    `darriusnjh <darriusnjh@users.noreply.github.com>` because no local identity
    was configured.
- Follow-ups:
  - Review whether any `.obsidian/` vault configuration should be tracked.
  - Add the first source to `raw/inbox/` and run the first ingest.
- Recommended commit message: `Initialize central AI research wiki`
