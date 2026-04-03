---
name: documentation-review
description: Review project documentation such as README files, setup guides, contributor docs, runbooks, and feature docs for accuracy, completeness, clarity, and drift from the codebase. Excludes ADRs and RFCs, including files under adr/adrs/rfc/rfcs paths, which should use the dedicated skills.
argument-hint: <doc-file-or-directory>
paths: "README*,CHANGELOG.md,CONTRIBUTING.md,DEVELOPMENT.md,OPERATIONS.md,RUNBOOK.md,docs/**/*.md,guides/**/*.md,runbooks/**/*.md"
effort: high
allowed-tools:
  - Read
  - Grep
  - Glob
  - Write
  - Edit
---

# Documentation Review
Use this skill to review project documentation for accuracy, completeness, usability, and maintenance quality.
This skill is for docs such as `README.md`, setup guides, contributor docs, developer workflow docs, feature docs, and runbooks.
Do not use this skill for ADRs or RFCs. Those should use the dedicated `adr` or `rfc` skills.

## Goal
The goal is to answer:
- can the intended reader understand what this project or feature does
- can they successfully follow the documented workflow
- does the documentation match the actual codebase and project structure
- does it explain important caveats, constraints, and failure modes

Documentation review is not copyediting only.
Prioritize correctness, missing guidance, onboarding friction, and drift from reality over minor wording polish.

## Operating Rules

Before reviewing a doc:
1. Identify the document type and intended audience.
2. Read the target document fully.
3. Read enough nearby code, config, and scripts to verify important claims.
4. Check whether the doc is current with the actual repo structure and commands.
5. Treat undocumented required steps as documentation bugs.

When editing or recommending changes:
- prefer concrete instructions over abstract descriptions
- prefer examples over vague references
- prefer commands that work from a clean checkout
- prefer warning about sharp edges before the reader hits them

## Scope And Exclusions
This skill applies to project overview docs, setup docs, testing and release docs, runbooks, operational docs, feature docs, and onboarding docs.

This skill does not apply to ADRs, RFCs, inline code comments, or generated API schemas unless the task is primarily doc quality rather than contract review.

If the document under review is actually an ADR or RFC, say so and switch to the more appropriate skill behavior.

Treat these as hard exclusions:
- `adr/`
- `adrs/`
- `rfc/`
- `rfcs/`
- `docs/adr/`
- `docs/adrs/`
- `docs/rfc/`
- `docs/rfcs/`

## Identify The Audience
First identify who the document is for.
Common audiences: new contributors, existing maintainers, operators, feature developers, and end users.
A good maintainer runbook and a good public README do not need the same content.

## What To Review

### Project orientation

Check whether the doc explains what the project or subsystem is, why it exists, who it is for, and where to start.
Flag docs that assume too much context up front.

### Setup and prerequisites

Check whether required prerequisites are explicit: language or runtime versions, package managers, external services, environment variables, local tooling, and access prerequisites where relevant.
Flag setup guides that hide required dependencies or assume prior tribal knowledge.

### Commands and workflows

Check whether documented commands are complete, ordered sensibly, runnable from a reasonable starting point, and consistent with package scripts and repo layout.
Flag commands that no longer exist, wrong paths, incomplete sequences, or missing cleanup notes.

### Accuracy against the codebase

Verify important claims against the repo: file paths, script names, configuration keys, feature names, service names, test commands, and deploy or build steps.
Flag docs that describe an old architecture, old command names, or deleted directories.

### Examples

Check whether examples are realistic, syntactically valid, aligned with the current project, and sufficient for the reader to apply the concept.
Flag docs that describe a workflow but never show an example command, payload, or file layout where one would help.

### Caveats and failure modes

Check whether the doc warns about common setup failures, platform-specific caveats, environment assumptions, destructive commands, and production or data-risk warnings.
Flag docs that make risky workflows look trivial when there are important sharp edges.

### Navigation and structure

Check whether the doc is easy to scan.
Look for a clear opening, meaningful headings, sensible ordering, links to related docs, and minimal duplication.
Flag giant undifferentiated blocks, repeated content, or dead-end references.

### Maintenance quality

Check whether the doc is likely to stay maintainable.
Look for ownership or related references, links to source-of-truth files where appropriate, duplicated instructions that may drift, and version-specific claims with no context.
Flag docs that are likely to rot because they copy information that clearly belongs elsewhere.

## Special Cases

### README

A strong `README` usually covers what the project is, prerequisites, install or setup, how to run or test, where to find deeper docs, and important caveats.

### Runbook

A strong runbook usually covers when to use it, required access, exact steps, safety warnings, rollback or recovery path, and verification steps.

### Contributor docs

A strong contributor guide usually covers setup, formatting and test expectations, common workflows, repo conventions, and how to validate changes.

## Drift Checks
Compare docs with the codebase where possible.

Common drift signals:
- doc references `yarn` but the repo uses `npm` or `pnpm`
- doc references files or directories that no longer exist
- doc references scripts missing from `package.json`
- doc references old environment variable names
- doc describes a feature flow that no longer matches the UI or API

If you cannot verify a claim from the repo, say confidence is limited rather than inventing certainty.

## Writing Rules For Fixes

When drafting or editing docs:
- write for the identified audience
- front-load prerequisites and assumptions
- keep command sequences copyable
- prefer literal commands, paths, and filenames
- use examples when they reduce ambiguity
- link to deeper docs instead of duplicating large sections

Do not add boilerplate text that sounds polished but says nothing.

## Anti-Patterns

Flag:
- docs that restate the repo name but never explain the project
- setup guides missing prerequisites
- commands that cannot work from a clean clone
- references to deleted files or old scripts
- deeply nested docs with no entry point
- duplicated instructions across multiple files with drift risk
- warnings hidden after destructive steps
- examples that are obviously stale
- feature docs with no links to source or ownership context
- docs that assume insider knowledge without saying so

## Review Workflow
Review in this order:
1. Identify the document type and audience.
2. Read the document end to end.
3. Verify core claims against the codebase and config.
4. Check setup, workflow, examples, caveats, and navigation.
5. Identify drift, missing steps, and high-friction points.
6. Recommend or apply edits that make the doc more usable and truthful.

## Findings Rules
Every finding must be:
- specific
- tied to reader impact
- actionable
- linked to a file and line when possible

Prefer findings like:
`The README tells contributors to run \`yarn test\`, but this repo only defines \`npm test\` scripts in package.json. Update the command or new contributors will fail on the first validation step.`

Avoid vague wording like `The documentation could be clearer.`

## Severity

Use:
- `high` for failures that block setup, hide dangerous behavior, mislead operators, or send readers down an invalid workflow
- `medium` for meaningful omissions, stale references, weak examples, or structure problems that create substantial friction
- `low` for smaller clarity issues, minor navigation problems, or weak but non-blocking wording

## Output Format

### Findings

List findings first, highest severity first.

Use one line per finding:

`- [high] README.md:34 — The setup instructions require Redis, but the README never mentions it in prerequisites. Add Redis to the prerequisite list before the install steps.`

If there are no findings, say `No findings.`

### Missing Coverage

List important gaps such as missing prerequisites, troubleshooting, example commands or payloads, rollback steps, validation steps, or links to deeper docs.

If the document looks complete for its audience, say `No significant coverage gaps.`

### Summary

Briefly summarize:
- what doc or docs were reviewed
- whether they appear accurate against the repo
- where the main reader friction is

### Verdict

Choose one:
- `ready`
- `needs-revision`
- `misleading`

Use:
- `ready` when the doc is accurate and usable for its audience
- `needs-revision` when the doc is directionally correct but incomplete or stale
- `misleading` when the doc would cause readers to fail, misunderstand, or take risky actions

## Review Style

Be direct and reader-focused.
Lead with accuracy, missing steps, and documentation drift.
Prefer edits that make the doc more runnable, navigable, and trustworthy.
