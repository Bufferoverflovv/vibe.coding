---
title: "Docs System – Personal Projects"
description: "Structure, naming, and templates for planning solo projects across Discovery, Project Brief (PRD), Technical Design, Implementation, and Validation."
version: "1.1.0"
status: "Approved"
owner: ""
---

# Project documentation system (personal)

This folder standardizes lightweight, consistent docs for solo projects. You can use one of two layouts:

- Single-project (default): the repo represents one project; phase artifacts live directly under docs/.
- Multi-project (optional): the repo hosts multiple projects; each gets its own folder under docs/projects/{project-slug}/.

Set your preference in docs/docs-config.yaml (see below). If no config is present, assume single-project.

## Templates

Templates live in docs/_templates/personal and are the source of truth. Copilot chatmodes load these templates and write phase artifacts to the chosen layout.

Phases (ordered):
- 00 Discovery & scope (Personal)
- 10 Project brief (Personal) – PRD
- 20 Technical design & specifications (Personal)
- 30 Implementation plan (Personal)
- 40 Validation & test plan (Personal)

## Directory layouts

Single-project (recommended for solo repos):
```
docs/
  _templates/
    personal/
      00-discovery-scope-personal.md
      10-project-brief-personal.md
      20-technical-design-personal.md
      30-implementation-plan-personal.md
      40-validation-test-plan-personal.md
  00-discovery-scope/
    index.md
  10-project-brief/
    index.md
  20-technical-design-spec/
    index.md
  30-implementation-plan/
    index.md
  40-validation-test-plan/
    index.md
  decisions/               (optional)
    decision-log.md
    adrs/
      0001-{short-title}.md
  assets/
    diagrams/
    images/
```

Multi-project (use only if this repo contains several projects):
```
docs/
  _templates/
    personal/
      00-discovery-scope-personal.md
      10-project-brief-personal.md
      20-technical-design-personal.md
      30-implementation-plan-personal.md
      40-validation-test-plan-personal.md
  projects/
    {project-slug}/
      00-discovery-scope/
        index.md
      10-project-brief/
        index.md
      20-technical-design-spec/
        index.md
      30-implementation-plan/
        index.md
      40-validation-test-plan/
        index.md
      decisions/
        decision-log.md
        adrs/
          0001-{short-title}.md
      assets/
        diagrams/
        images/
```

Notes:
- Use kebab-case for {project-slug} (e.g., blog-to-notion-sync).
- One index.md per phase is the canonical artifact for that phase.
- Prefer Mermaid diagrams inline; export images to assets/ if needed.

## Frontmatter convention (personal docs)

Each phase artifact should include this minimal frontmatter. Chatmodes populate these fields automatically.

```yaml
---
title: ""
project: "{project-slug or repo name (optional in single-project)}"
phase: "00|10|20|30|40"
version: "0.1.0"
template_version: "<from-template>"
status: "Draft|In review|Approved"
owner: ""
created: "YYYY-MM-DD"
updated: "YYYY-MM-DD"
links:
  discovery: ""
  prd: ""
  tech_spec: ""
  impl_plan: ""
  validation_plan: ""
tags: []
---
```

Status lifecycle: Draft → In review → Approved.

## Chatmodes and output paths

Chatmodes read the template_path and write to the output path that matches your layout.

Single-project (default outputs):
- Discovery & scope (Personal): docs/00-discovery-scope/index.md
- Project brief (Personal): docs/10-project-brief/index.md
- Technical design & specifications (Personal): docs/20-technical-design-spec/index.md
- Implementation plan (Personal): docs/30-implementation-plan/index.md
- Validation & test plan (Personal): docs/40-validation-test-plan/index.md

Multi-project (optional outputs):
- Discovery & scope (Personal): docs/projects/{project-slug}/00-discovery-scope/index.md
- Project brief (Personal): docs/projects/{project-slug}/10-project-brief/index.md
- Technical design & specifications (Personal): docs/projects/{project-slug}/20-technical-design-spec/index.md
- Implementation plan (Personal): docs/projects/{project-slug}/30-implementation-plan/index.md
- Validation & test plan (Personal): docs/projects/{project-slug}/40-validation-test-plan/index.md

All modes include a compact fallback template so they still work if template files are missing. The template_version recorded in each artifact comes from the template frontmatter.

## Layout configuration

Create docs/docs-config.yaml to select layout and (optionally) a default project slug for multi-project repos.

```yaml
layout: single-project  # values: single-project | multi-project
default_project_slug: ""  # optional, used only when layout = multi-project
```

If docs/docs-config.yaml is absent, chatmodes will default to single-project. Some modes may also auto-detect multi-project if docs/projects/ exists.

## Handoffs between phases

- Discovery → Project brief: idea, motivation, learning goals, MVP, constraints, risks
- Project brief → Tech design: PB-### stories and acceptance, constraints, integrations
- Tech design → Implementation: requirements (REQ/NFR/etc.), acceptance (AC-###), interfaces, data model
- Implementation → Validation: phase gates, tasks to test, environment and data needs

## Conventions

- Keep all sections even if empty; write “None” rather than deleting.
- Use explicit IDs consistently (PB-###, REQ-###, AC-###, TASK-###, TEST-###, GATE-##).
- Prefer Mermaid diagrams in tech design where helpful.
- Use decision-log entries with date-stamped lines; ADRs are optional for personal projects.

## Automation notes

- Issue creation: Project brief and Implementation plan modes can open GitHub issues on request.
- File writing: Modes write to the correct output paths based on layout and will offer to append to Decision log/Open questions or create a timestamped copy if the file exists.
- Template updates: Update files in docs/_templates/personal to change structure across all future artifacts.