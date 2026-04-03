---
name: rfc-author
description: Draft, update, or review Requests for Comments. Use when proposing a significant technical change that still needs alignment, review, and option comparison before a decision becomes final. If the decision is already made and needs durable recording, use the ADR skill instead.
argument-hint: <proposal-topic-or-rfc-file>
paths: "*.md,docs/rfc/**/*.md,docs/rfcs/**/*.md,rfc/**/*.md,rfcs/**/*.md"
effort: high
allowed-tools:
  - Read
  - Grep
  - Glob
  - Write
  - Edit
---

# RFC
Use this skill for Requests for Comments.
An RFC shapes and stress-tests a proposal before it becomes a settled decision.
Unlike an ADR, an RFC should preserve uncertainty, surface tradeoffs, and invite review.
Use `adr` instead when the decision is already made and mainly needs durable recording.

## Operating Rules
Before writing or reviewing an RFC:
1. Look for existing RFCs, templates, numbering, and status conventions.
2. Follow local format if one exists.
3. If none exists, use the default structure below.
4. Read enough code, docs, issues, incidents, and PRs to understand the problem space.
5. Keep the RFC focused on one proposal or one tightly related change set.

When writing:
- preserve open questions instead of pretending the answer is final
- prefer explicit tradeoffs over promotional language
- distinguish problem statement from proposed solution
- separate confirmed facts from assumptions

## When To Use This Skill
Use this skill when:
- proposing a significant architecture or platform change
- introducing a new framework, service, workflow, or protocol
- changing an important operational or product boundary
- replacing a major dependency or subsystem
- requesting cross-team review on a technical direction
- turning scattered discussion into a structured proposal
- reviewing whether an RFC is ready for wider comment

Do not use an RFC for routine implementation notes, bugfixes, or narrow code changes with no broader design impact.

## Discover Existing Conventions
Inspect the repo for `docs/rfc/`, `docs/rfcs/`, `rfc/`, `rfcs/`, numbered filenames, proposal status vocabulary, review sections, templates, or examples.
If RFCs already exist, mirror file naming, heading names, status wording, level of detail, and linking style.

## Default RFC Structure
Use this unless the repo has a stronger template:

```md
# RFC: <short proposal title>

- Status: Draft | In Review | Accepted | Rejected | Withdrawn | Superseded
- Date: YYYY-MM-DD
- Authors: <name or team if known>
- Reviewers: <optional>
- Related: <issue, ADR, PR, incident, doc>

## Summary
## Problem
## Goals
## Non-Goals
## Proposal
## Alternatives Considered
## Risks and Tradeoffs
## Rollout Plan
## Review Ask
## Open Questions
```

You may add sections like `Success Metrics`, `Security Considerations`, or `Migration Plan` when they materially matter.

## Section Guidance

### Title
The title should describe the proposal clearly.
Good: `RFC: Migrate the frontend build from webpack to Vite`
Weak: `RFC: Frontend tooling`

### Status
Use one of `Draft`, `In Review`, `Accepted`, `Rejected`, `Withdrawn`, or `Superseded`.
Do not mark an RFC `Accepted` if key tradeoffs are still unresolved.

### Summary, Problem, Goals, Non-Goals
The summary should explain the proposal in a few sentences.
The problem should explain what is wrong, who is affected, what constraints exist, and why it matters now.
Do not start with the solution.
Goals should be concrete enough that reviewers can tell whether the proposal addresses them.
Non-goals should state clearly what the RFC is not trying to solve.

### Proposal
Describe what will change, how the system boundary changes, what teams or services are affected, and what new dependencies, interfaces, or workflows are introduced.
Be specific enough for review, but do not drown the RFC in implementation trivia.

### Alternatives, Risks, Rollout
List realistic alternatives, not strawmen.
For each option, capture what it is, why it was considered, and what tradeoff made it weaker or stronger.
Compare the proposed option against at least one viable alternative unless the choice was forced externally.
Capture both upside and downside, including migration cost, operational burden, performance impact, security implications, lock-in risk, developer workflow impact, and long-term maintenance cost.
Do not write only benefits.
For rollout, capture sequencing, compatibility plan, migration steps, rollback or escape hatch if relevant, and coexistence strategy if old and new approaches overlap.
If rollout is uncertain, say so explicitly.

### Review Ask And Open Questions
State what feedback is actually needed: validate the boundary, challenge the migration plan, compare against a named alternative, or confirm scope.
An RFC without a clear review ask often turns into passive documentation instead of decision support.
List only unresolved questions that matter to approval, implementation, or adoption.

## Writing Rules
Keep the RFC concise, but decision-useful.
Prefer short paragraphs and concrete bullets over sprawling narrative.
Separate facts, assumptions, and recommendations clearly.
Use one RFC per meaningful proposal.
If multiple proposals are tightly coupled, explain the dependency instead of blending them into an unclear mega-document.
If numbers matter, include them: scale, latency targets, traffic volume, cost estimates, rollout window, or support window.
Label assumptions as assumptions.

## Anti-Patterns
Avoid:
- advocacy copy instead of a reviewable proposal
- presenting the proposal as decided when it is not
- omitting the current problem
- hiding migration risk in vague language
- listing alternatives without tradeoff analysis
- claiming simple rollout without sequencing
- stuffing the RFC with implementation detail that obscures the proposal
- writing an RFC with no clear review ask

## Update And Supersession
When updating an RFC:
- preserve proposal history unless asked to rewrite it
- update status honestly
- note major changes in direction if the proposal evolved materially

When superseding an RFC:
- create or reference the replacing RFC
- mark the old RFC as superseded
- link both directions when possible
- explain why the earlier proposal no longer stands

Do not silently rewrite a controversial proposal as if the earlier version never existed.

## Modes
### Draft
Return a full RFC in repo format or the default structure above.
If key inputs are missing, make the fewest necessary assumptions and label them.
If the decision is already settled, say an ADR may be the better artifact.

### Update
Preserve structure unless it is clearly broken.
Update status, proposal details, risks, rollout, and open questions as needed.

### Review
Focus on missing problem framing, weak tradeoff analysis, dishonest status, hidden rollout risk, unclear non-goals, and unresolved questions that should block approval.

## Review Checklist
Check whether the RFC clearly answers:
- what problem exists
- why this proposal exists now
- what the proposal changes
- what goals and non-goals are
- what alternatives were considered
- what tradeoffs and risks are accepted
- how rollout or migration would work
- what remains unresolved
- whether the status is honest
- whether the review ask is clear

Also check whether a reviewer from another team could understand the proposal without prior meeting context.

## Output Format
### Draft Or Update
Return:
### Draft Or Update

Return:
- the full RFC markdown
- a short note listing major assumptions if needed

### Review
Return this structure:

#### Findings
List findings first.
Use one line per finding:

`- [medium] docs/rfcs/0004-vite-migration.md:27 — The RFC proposes migrating to Vite but never defines non-goals, so reviewers cannot tell whether test runner, SSR, and deploy changes are in scope. Add a Non-Goals section to bound the proposal.`

If there are no findings, say `No findings.`

#### Open Questions
List only questions that materially affect proposal quality or approval readiness.
If none, say `None.`

#### Summary
Briefly summarize:
- what proposal the RFC describes
- whether the problem framing and tradeoffs are complete
- whether rollout and review readiness are clear

#### Verdict
Choose one:
- `ready-for-review`
- `needs-revision`
- `not-ripe`

Use:
- `ready-for-review` when the RFC is complete enough for broad comment
- `needs-revision` when the proposal is plausible but the document is incomplete
- `not-ripe` when the problem, proposal, or review ask is still too vague

## Review Style
Be direct and proposal-focused.
Optimize for reviewers who were not in the room.
Prefer clarity, tradeoffs, scope control, and rollout realism over polished but low-information prose.
