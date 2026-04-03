---
name: openapi-author
description: Author or update OpenAPI specs with strong governance. Use for writing OpenAPI 3.x documents, improving spec quality, and validating with Spectral plus IBM and OWASP-oriented rulesets before treating a spec as ready.
argument-hint: <spec-file-or-api-scope>
paths: "openapi*.yml,openapi*.yaml,openapi*.json,swagger*.yml,swagger*.yaml,swagger*.json,docs/api/**/*.md"
effort: high
allowed-tools:
  - Read
  - Grep
  - Glob
  - Write
  - Edit
  - Bash(npx *)
  - Bash(npm *)
---

# OpenAPI Author

Use this skill to author or update OpenAPI specs with strong linting and governance.
Treat the spec as a contract, not just documentation.
Prefer OpenAPI `3.1.x` for new specs unless the repo is already committed to `3.0.x`.
Do not consider a spec finished until it passes linting and design checks.

## Validation First

Prefer this toolchain:
- `@stoplight/spectral-cli`
- `@ibm-cloud/openapi-ruleset`
- `ibm-openapi-validator`
- `@stoplight/spectral-owasp-ruleset`

If the repo does not already have them, suggest installing:

```bash
npm install --save-dev @stoplight/spectral-cli @stoplight/spectral-owasp-ruleset @ibm-cloud/openapi-ruleset ibm-openapi-validator
```

Prefer scripts like:

```json
{
  "scripts": {
    "lint:openapi": "spectral lint openapi.yaml",
    "lint:openapi:ibm": "lint-openapi openapi.yaml"
  }
}
```

Recommend a `.spectral.yaml` such as:

```yaml
extends:
  - spectral:oas
  - "@ibm-cloud/openapi-ruleset"
  - "@stoplight/spectral-owasp-ruleset"
```

If the repo already has linting, extend it instead of replacing it blindly.

## Operating Rules

Before authoring or editing a spec:
1. Check whether the repo already has an OpenAPI file, ruleset, or validator config.
2. Follow the repo’s current OpenAPI version unless there is a reason to upgrade.
3. Reuse existing component schemas, security schemes, and naming conventions.
4. If implementation exists, align the spec with real behavior rather than inventing endpoints.
5. Run or recommend linting before treating the spec as complete.

When writing:
- prefer explicit schemas over prose-only descriptions
- prefer reusable components over repeated inline objects
- prefer consistent naming and response patterns
- prefer complete error and auth documentation over happy-path-only specs

## Baseline Standards

At minimum, a good OpenAPI spec should include:
- `openapi`
- `info.title`
- `info.version`
- meaningful `servers`
- tagged operations
- stable `operationId`
- request parameters and request bodies
- success and error responses
- reusable component schemas where repetition exists
- security schemes when auth exists

Do not ship specs that document only `200` or `201` and ignore realistic error paths.

## What To Enforce

### Metadata and operations

Require:
- meaningful API title and real version
- coherent tags
- operation summaries
- parameters with schema and required flags
- request body schema when applicable
- success responses
- realistic error responses
- security declarations when protected

### Schemas and examples

Require:
- explicit types
- clear required vs optional fields
- enums when values are constrained
- formats for UUIDs, timestamps, emails, and URIs where relevant
- examples for non-trivial payloads

Prefer component reuse for shared objects such as error payloads, pagination metadata, common resources, and security error responses.

Flag:
- `type: object` with no meaningful properties
- inconsistent nullability
- duplicated inline schemas drifting over time
- examples that contradict the schema

### Responses, security, pagination, lifecycle

Document:
- validation errors, auth failures, not found or conflict cases when realistic
- bearer, API key, cookie, or OAuth security schemes when used
- per-operation auth or scopes when they differ
- pagination mechanism, limits, sort fields, filters, and response metadata for list endpoints
- versioning strategy, deprecations, replacement guidance, and sunset or migration notes when relevant

Prefer explicit `security` declarations over assuming auth in prose.

## IBM And OWASP Expectations

Use the IBM ruleset to enforce stronger API design consistency and completeness.
Use the OWASP ruleset to surface security-oriented gaps visible at the contract layer.
Do not claim OWASP compliance from spec linting alone.

Useful OWASP-oriented findings include:
- missing or weak security declarations
- insecure server URL patterns
- missing inventory or audience metadata
- absent rate-limiting headers or related protections in documented responses

Treat those as design feedback that should be resolved in the spec and validated in implementation.

## Authoring And Linting Workflow

Work in this order:
1. Identify spec scope and version.
2. Define `info`, `servers`, tags, and security schemes.
3. Add paths and operations.
4. Add parameters, request bodies, and responses.
5. Extract shared schemas into `components`.
6. Add examples, error shapes, and pagination details.
7. Run Spectral and IBM validation.
8. Fix findings before calling the spec done.

After editing, prefer commands like:

```bash
npx spectral lint openapi.yaml
npx lint-openapi openapi.yaml
```

If the repo standardizes on scripts, prefer those scripts instead.
If linting is not installed, say so explicitly and recommend the install rather than pretending the spec is validated.

## Anti-Patterns

Avoid:
- paths with no `operationId`
- documenting only happy-path responses
- prose-only field semantics without schema constraints
- repeating the same object inline across many operations
- vague top-level names like `data` or `result` without context
- examples that contradict actual field types
- unsecured operations that should clearly declare auth
- disabling IBM or OWASP findings without a documented reason
- treating a passing linter run as proof the implementation is secure

## Output Expectations

When authoring or updating:
- produce valid OpenAPI YAML or JSON
- mention whether linting was run
- mention which validator setup is expected if it was not run
- call out low-confidence assumptions

When reviewing:
- lead with findings
- include lint or ruleset gaps
- separate contract-quality issues from implementation assumptions

## Review Format

### Findings

List findings first, highest severity first.
Use one line per finding:

`- [high] openapi.yaml:142 — POST /users documents only a 201 response and omits validation and auth failure responses, so consumers cannot implement reliable error handling. Add at least 400 and 401 responses with schemas.`

If there are no findings, say `No findings.`

### Linting Gaps

List missing validation setup such as missing `.spectral.yaml`, missing IBM ruleset, missing OWASP ruleset, or missing `lint-openapi` integration.
If the validation setup looks adequate, say `No significant linting gaps.`

### Summary

Briefly summarize:
- what spec or scope was reviewed
- whether the spec looks contract-complete
- whether linting and governance look strong enough

### Verdict

Choose one:
- `ready`
- `needs-revision`
- `not-validated`

Use:
- `ready` when the spec is strong and validated
- `needs-revision` when the spec has contract or quality gaps
- `not-validated` when the main blocker is missing linting or governance setup
