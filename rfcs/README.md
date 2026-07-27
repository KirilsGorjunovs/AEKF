# AEKF RFC Process

RFCs are the primary record of architectural intent, major hypotheses, and proposed changes to AEKF.

The conversation that produces an idea is not the source of truth. The corresponding RFC is.

## RFC lifecycle

```text
Draft → Review → Accepted → Implemented → Deprecated
                    ↘ Rejected
                    ↘ Superseded
```

### Draft

The proposal is incomplete or still being explored. Open questions and alternatives are expected.

### Review

The proposal is considered coherent enough for structured criticism. Review should test assumptions, identify alternatives, and make trade-offs explicit.

### Accepted

The proposal has been approved as architectural direction. Acceptance does not necessarily mean that implementation is complete.

### Implemented

The accepted proposal has a corresponding implementation, renderer, schema, specification change, or other verifiable result.

### Rejected

The proposal was considered but will not be adopted. Rejected RFCs remain in the repository to preserve reasoning and avoid repeating the same discussion.

### Superseded

A newer RFC replaces all or part of the proposal. Both documents remain available and link to each other.

### Deprecated

The proposal was once accepted or implemented but is no longer recommended.

## Required metadata

Every RFC should include:

- Status;
- Version;
- Authors;
- Created date;
- Updated date;
- Discussion location;
- Supersedes;
- Superseded by.

## Naming

Use a three-digit identifier and a descriptive kebab-case title:

```text
000-why-aekf-exists.md
001-architectural-principles.md
002-knowledge-model.md
```

RFC identifiers are never reused.

## Expected structure

An RFC should normally contain:

1. Abstract;
2. Context or Problem Statement;
3. Proposal;
4. Rationale;
5. Alternatives Considered;
6. Risks and Trade-offs;
7. Compatibility or Migration Impact, when relevant;
8. Validation Strategy;
9. Open Questions;
10. Decision or Closing Statement.

RFC-000 is intentionally vision-oriented and does not need to follow every section mechanically.

## Decision policy

No new idea becomes normative merely because it appears in an RFC.

Before acceptance, the proposal should be challenged through:

- counterexamples;
- competing models;
- implementation constraints;
- context and discovery cost;
- maintainability concerns;
- measurable evaluation where possible.

Unresolved questions remain in the RFC. They must not be silently converted into specification requirements.

## Relationship to the specification

RFCs preserve the history and rationale of decisions.

The AEKF specification will contain the current normative model derived from accepted RFCs. It must link back to the RFCs that justify its requirements.

Specification text must not be treated as automatically generated from any Draft RFC.

## Relationship to implementations

Codex skills, Claude skills, prompts, rules, schemas, renderers, and reference implementations are downstream artefacts.

They may implement accepted AEKF decisions, but they do not define the framework's source knowledge model.

## Changes to accepted RFCs

Minor editorial corrections may update an accepted RFC without changing its meaning.

A material semantic change requires either:

- a new RFC that supersedes the previous one; or
- reopening the RFC for review when the project has not yet stabilised the decision.
