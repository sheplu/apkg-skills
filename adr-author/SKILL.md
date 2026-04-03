---
name: adr-author
description: Draft, update, or review Architecture Decision Records. Use when a technical decision is made or nearly made and needs durable documentation of context, tradeoffs, consequences, and supersession. If the proposal is still open for broad debate, use the RFC skill instead.
argument-hint: <decision-topic-or-adr-file>
paths: "*.md,docs/adr/**/*.md,docs/adrs/**/*.md,adr/**/*.md,adrs/**/*.md"
effort: high
allowed-tools:
  - Read
  - Grep
  - Glob
  - Write
  - Edit
---

# ADR

Use this skill for Architecture Decision Records.
An ADR records a decision durably: why it exists, what alternatives were considered, what tradeoffs were accepted, and what consequences follow.
Use `rfc` instead when the proposal is still under active comparison, needs broad alignment, or must preserve multiple unresolved paths.
Do not use this skill for general project docs such as `README`, setup guides, runbooks, or contributor docs.

## Operating Rules

Before writing or reviewing an ADR:
1. Look for existing ADRs, templates, numbering, and status conventions.
2. Follow local format if one exists.
3. If none exists, use the default structure below.
4. Read enough code, docs, issues, PRs, or incidents to understand the decision context.
5. Keep the ADR focused on one decision unless the repo clearly combines related decisions.

When writing:
- prefer explicit tradeoffs over vague optimism
- prefer concrete consequences over generic statements
- prefer rationale over implementation detail
- preserve uncertainty honestly when the decision is not fully final

## Discover Existing Conventions
Inspect the repo for `docs/adr/`, `docs/adrs/`, `adr/`, `adrs/`, numeric prefixes, status vocabulary, templates, or examples.
If ADRs already exist, mirror file naming, heading names, status formatting, link style, and level of detail.

## Default ADR Structure

Use this unless the repo has a stronger template:
```md
# ADR: <short decision title>

- Status: Proposed | Accepted | Rejected | Superseded | Deprecated
- Date: YYYY-MM-DD
- Deciders: <team or names if known>
- Related: <issue, PR, incident, RFC, previous ADR>

## Context
## Decision
## Options Considered
## Consequences
## Rollout / Migration
## Open Questions
```

You may omit `Rollout / Migration` or `Open Questions` only if they truly do not apply.

## Section Guidance
### Title

The title should name the decision, not the meeting topic.
Good: `ADR: Adopt Vite for the frontend build`
Weak: `ADR: Frontend build`

### Status

Use one of `Proposed`, `Accepted`, `Rejected`, `Superseded`, or `Deprecated`.
Do not mark an ADR `Accepted` if the core choice is still unresolved.

### Context

Explain the problem, the constraints, the decision drivers, and why the decision matters now.
Do not merely restate the solution.

### Decision

State what was chosen in direct language.
Prefer `We will use` or `We will standardize on`.
Avoid `We may move toward` or `One option is`.

### Options Considered

List realistic alternatives, not strawmen.
For each option, capture what it is, why it was considered, and why it won or lost.
Compare the chosen option against at least one viable alternative unless the choice was forced externally.

### Consequences

Include both upside and downside.
Capture things like operational impact, developer workflow impact, migration cost, lock-in risk, performance implications, security implications, and support burden.
Do not write only benefits.

### Rollout / Migration

Include this when adoption is not immediate or trivial.
Capture sequencing, compatibility concerns, migration steps, rollback or escape hatch if relevant, and coexistence of old and new approaches when applicable.

### Open Questions

Include only unresolved questions that materially affect implementation or follow-up.

## Writing Rules
Keep the ADR concise, but complete.
Prefer short paragraphs and concrete bullets.
Use present tense for accepted decisions.
Use one ADR per meaningful decision.
Name technologies, protocols, and boundaries precisely.
If numbers matter, include them: scale, latency targets, data volume, cost thresholds, or support window.
Label assumptions as assumptions.

## Anti-Patterns

Avoid:
- implementation steps instead of a decision record
- a conclusion with no context
- options listed without tradeoffs
- only positive outcomes
- unresolved debate presented as an accepted choice
- migration risk hidden in one vague sentence
- an ADR that cannot be linked to a real problem

## Update And Supersession

When updating an ADR:
- preserve historical intent unless explicitly asked to rewrite it
- update status rather than silently changing meaning
- note what changed and why

When superseding an ADR:
- create or reference the replacing ADR
- mark the old ADR as superseded
- link both directions when possible
- explain what changed in constraints or direction

Do not overwrite an accepted ADR as if the earlier decision never existed.

## Modes

### Draft

Return a full ADR in repo format or the default structure above.
If key inputs are missing, make the fewest necessary assumptions and label them.
If the core decision is still unresolved, say an RFC is the more appropriate artifact first.

### Update

Preserve structure unless it is clearly broken.
Update status, context, consequences, rollout, or linked records as needed.

### Review

Focus on missing rationale, weak tradeoff analysis, dishonest status, hidden migration risk, and unclear consequences.

## Review Checklist

Check whether the ADR clearly answers:
- what decision was made
- why this decision exists
- what alternatives were considered
- why the chosen option won
- what tradeoffs were accepted
- what risks and downsides remain
- how adoption or migration will work
- whether the status is honest
- whether related ADRs, issues, PRs, or incidents are linked

Also check whether a new engineer could understand the rationale months later without prior meeting context.

## Output Format
### Draft Or Update

Return:
- the full ADR markdown
- a short note listing major assumptions if needed

### Review

Return this structure:
#### Findings

List findings first.
Use one line per finding:

`- [medium] docs/adr/0007-build-system.md:18 — The ADR states the team will adopt Vite but never explains why webpack alternatives were rejected, so future readers cannot evaluate the tradeoff. Add at least one viable alternative and why it lost.`

If there are no findings, say `No findings.`

#### Open Questions

List only questions that materially affect decision quality or adoption planning.
If none, say `None.`

#### Summary

Briefly summarize:
- what decision the ADR records
- whether the rationale is complete
- whether rollout and consequences are clear

#### Verdict

Choose one:
- `ready`
- `needs-revision`
- `needs-decision`

Use:
- `ready` when the ADR is complete and honest
- `needs-revision` when the decision may be valid but the record is incomplete
- `needs-decision` when the document pretends a choice exists but the choice is still unresolved

## Review Style

Be direct and durable.
Optimize for future readers, not meeting participants.
Prefer clarity, tradeoffs, and explicit consequences over polished but low-information prose.
