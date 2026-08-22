# Business Analysis Pipeline — Storage

Local storage backing the `/ba-analyze` and `/ba-ask` commands defined in the `CLAUDE` project
(`.claude/agents/ba-*.md`, `.claude/commands/ba-*.md`).

Git history in this repo is the record of intermediate documentation versions — each pipeline
stage transition (draft saved, moved to evaluation, finalized) is its own commit. Pushing to the
`origin` remote is never automatic; it happens only when the user explicitly asks the pipeline to
push.

## Structure

```
projects/
└── <project-slug>/
    ├── 00-request.md              ← original request, source doc references, full Q&A transcript
    ├── working/                   ← in-progress drafts as agents iterate
    │   ├── vision-and-scope.md
    │   └── architecture-specification.md
    ├── ready-for-evaluation/      ← snapshot handed to ba-reasoning-evaluator
    │   ├── vision-and-scope.md
    │   └── architecture-specification.md
    ├── evaluation/
    │   └── gap-report_YYYYMMDD_HHMMSS.md   ← one per evaluation round
    └── ready_for_dev_docs/        ← final, user-facing output
        ├── vision-and-scope.md
        └── architecture-specification.md
```

`<project-slug>` is a short kebab-case identifier derived from the request (e.g.
`vacation-request-tracker`), created fresh per `/ba-analyze` run.

## Agents

- **ba-business-analyst** — scope, use cases, user flows, Vision & Scope document.
- **ba-system-architect** — architecture proposal with embedded Mermaid diagrams.
- **ba-reasoning-evaluator** — checks both documents against the original request and the
  confirmed Q&A transcript, flags unconfirmed additions or missing requirements, and routes
  rework back to the other two agents when a document actually needs to change.
