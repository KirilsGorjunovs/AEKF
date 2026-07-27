# RFC-001 — Architectural Principles

- **Status:** Draft
- **Version:** 0.1
- **Authors:** AEKF Working Group
- **Created:** 2026-07-27
- **Updated:** 2026-07-27
- **Discussion:** TBD
- **Supersedes:** None
- **Superseded by:** None

## Abstract

RFC-000 establishes why the AI Engineering Knowledge Framework exists.

This RFC defines the architectural principles that constrain how AEKF may represent, connect, select, evaluate, evolve, and deliver software engineering knowledge.

These principles are intentionally more stable than any particular knowledge schema, storage technology, renderer, agent integration, or repository layout. Future RFCs may define the concrete AEKF Knowledge Model, knowledge units, decision graph, discovery mechanisms, evaluation metrics, and rendering architecture, but those proposals must remain consistent with the principles defined here or explicitly justify a deviation.

This RFC does not define the final AEKF entity model.

## Context

AEKF aims to make software engineering knowledge reusable across AI agents, projects, technologies, and future generations of engineering tools.

Without explicit architectural principles, the project risks reproducing the same problems identified in RFC-000:

- coupling knowledge to a particular AI vendor;
- treating prompts, skills, rules, or files as fundamental engineering concepts;
- duplicating the same knowledge across delivery formats;
- loading excessive context for narrow tasks;
- losing the source, authority, and status of engineering guidance;
- optimising for one implementation before the knowledge model is understood.

Architectural principles provide stable constraints before the concrete model is designed.

They define what future solutions must preserve, while leaving room to test alternative schemas and implementations.

## Normative Language

The key words **MUST**, **MUST NOT**, **REQUIRED**, **SHALL**, **SHALL NOT**, **SHOULD**, **SHOULD NOT**, **RECOMMENDED**, **MAY**, and **OPTIONAL** in this document are to be interpreted as described in BCP 14 when, and only when, they appear in uppercase.

Because this RFC is currently in Draft status, its normative statements are proposed requirements rather than accepted AEKF requirements.

## Scope

This RFC defines principles for:

- the independence and durability of engineering knowledge;
- separation between source knowledge and delivery artefacts;
- relationships to established bodies of knowledge;
- decision support;
- knowledge connectivity;
- discovery and context loading;
- provenance and epistemic transparency;
- composition and reuse;
- measurement and validation;
- human authority;
- evolution and traceability.

This RFC does not define:

- the final set of AEKF entities;
- whether an Engineering Decision is the primary knowledge unit;
- a canonical storage format;
- a graph database or document database requirement;
- a renderer interface;
- a discovery algorithm;
- exact quality metrics or acceptance thresholds;
- vendor-specific integrations.

## Architectural Principles

### AP-01 — Engineering Knowledge Must Be Durable

#### Normative statement

> AEKF knowledge MUST remain meaningful independently of the AI agent, model, vendor, IDE, repository host, or delivery mechanism that consumes it.

#### Rationale

Engineering principles, trade-offs, constraints, and decision criteria often remain valid longer than the tools used to communicate them.

A security rule should not become obsolete merely because a project moves from one coding assistant to another. An architectural rationale should not need to be rewritten because a vendor renames a skill, rule, hook, or memory feature.

#### Consequences

- Vendor-specific instructions MUST NOT be the sole source of engineering knowledge.
- Canonical knowledge SHOULD be understandable by both humans and machines.
- Consumer-specific optimisation MAY occur downstream, provided the underlying meaning remains traceable.
- A change of agent or renderer SHOULD NOT require rewriting the source engineering knowledge.

### AP-02 — The Core Model Must Be Vendor-Neutral

#### Normative statement

> The AEKF core model MUST NOT depend on vendor-specific concepts as fundamental entities.

#### Rationale

Terms such as Skill, Rule, Hook, Memory, Subagent, System Prompt, and `AGENTS.md` describe current implementation mechanisms. They are not stable concepts of software engineering knowledge.

If these mechanisms become first-class entities in the core model, the model inherits the lifecycle and limitations of the vendor that introduced them.

#### Consequences

- Vendor concepts MAY exist in renderer or adapter models.
- The core model MUST remain capable of supporting consumers whose mechanisms differ from those known today.
- A vendor integration MUST map to the AEKF model rather than redefining it.
- Vendor-specific features that cannot preserve source semantics MUST document the loss or transformation explicitly.

### AP-03 — Knowledge, Selection, and Delivery Must Be Separated

#### Normative statement

> AEKF MUST distinguish source engineering knowledge from the mechanisms that select, compose, and deliver that knowledge.

#### Rationale

A Markdown instruction file may contain useful engineering knowledge, but the file itself is only one representation. Selection determines which knowledge is relevant. Composition determines how multiple units are combined. Delivery adapts the result to a consumer.

Conflating these concerns makes knowledge difficult to reuse, test, and evolve.

#### Conceptual separation

```text
Source Engineering Knowledge
            ↓
Selection and Composition
            ↓
Consumer-Specific Rendering
            ↓
AI Agent, Tool, or Human Engineer
```

#### Consequences

- Rendered prompts, skills, rules, and instruction files MUST be treated as downstream artefacts.
- Selection logic MUST NOT silently redefine the source knowledge.
- A renderer SHOULD be replaceable without changing the canonical meaning of the knowledge.
- The same source knowledge SHOULD be renderable for multiple consumers.

### AP-04 — Established Bodies of Knowledge Are Baselines, Not Executable Models

#### Normative statement

> AEKF SHALL use SWEBOK as a domain, vocabulary, and coverage baseline, but SHALL NOT treat the SWEBOK taxonomy as an executable model for representing, selecting, or delivering engineering knowledge.

#### Rationale

SWEBOK primarily addresses:

> **What belongs to software engineering?**

AEKF addresses a different class of questions:

> **How should software engineering knowledge be represented, connected, selected, evaluated, and delivered to an AI agent?**

SWEBOK provides an established, consensus-driven map of the software engineering discipline. Its Knowledge Areas describe concepts, activities, practices, methods, and concerns that belong to software engineering.

However, its taxonomy is not a universal execution model for task-specific knowledge discovery, dependency traversal, progressive context loading, renderer generation, or agent decision support.

#### Consequences

- SWEBOK SHOULD be used to test AEKF domain coverage.
- SWEBOK terminology SHOULD be preferred unless AEKF has a documented reason to define a different term.
- AEKF MAY introduce entities and relationships that do not exist in the SWEBOK taxonomy.
- A SWEBOK Knowledge Area MUST NOT automatically be treated as an AEKF Capability, knowledge unit, Skill, or workflow.
- Primary standards and literature SHOULD be used where greater precision or normative authority is required.
- AEKF MUST remain open to knowledge outside SWEBOK when it is relevant to AI-assisted software engineering.

### AP-05 — Knowledge Must Support Engineering Decisions

#### Normative statement

> AEKF knowledge MUST be usable in the context of concrete engineering decisions, actions, or evaluations rather than serving only as passive topic descriptions.

#### Rationale

A topic-oriented document may explain aggregate design, authentication, transaction boundaries, or test design. An engineering task usually requires a more specific outcome:

- whether an object should become an Aggregate Root;
- whether an endpoint requires authorisation;
- whether two modules should communicate synchronously;
- whether a change requires an architectural decision record;
- which tests are necessary to protect a behaviour.

AEKF must help a consumer move from a task and its context toward an informed engineering outcome.

#### Consequences

- Knowledge SHOULD identify the situations in which it is relevant.
- Knowledge SHOULD expose applicable criteria, constraints, trade-offs, and expected outputs.
- Knowledge SHOULD distinguish guidance from the decision reached in a specific project context.
- The model MUST support decision-oriented use without yet requiring Engineering Decision to be the only or primary knowledge entity.
- Descriptive reference material MAY remain part of AEKF when it supports decisions, workflows, interpretation, or validation.

### AP-06 — The Model Must Be Graph-Aware

#### Normative statement

> AEKF MUST support non-hierarchical relationships between knowledge elements.

#### Rationale

Software engineering knowledge does not form a single tree.

A security decision may depend on requirements, architecture, data classification, threat modelling, testing, deployment, and operations. A single knowledge element may belong to several domains and participate in multiple workflows.

A folder hierarchy or one-parent taxonomy cannot represent these relationships without duplication or loss of meaning.

#### Consequences

- The model MUST support typed relationships such as dependency, prerequisite, refinement, conflict, evidence, applicability, sequence, and production of an artefact.
- Hierarchies MAY be used for navigation, ownership, or presentation.
- A hierarchical view MUST NOT be assumed to be the complete semantic model.
- This principle does not require a graph database. Documents, relational storage, or other representations MAY be used if they preserve the required relationships.

### AP-07 — Discovery Must Use Progressive Disclosure

#### Normative statement

> AEKF MUST support discovery and selection without requiring the full content of every potentially relevant knowledge element to be loaded.

#### Rationale

AI agents operate within finite context, attention, latency, and cost constraints. Loading complete bodies of knowledge for every task reduces signal quality and may introduce irrelevant or conflicting instructions.

The consumer should first receive enough information to determine relevance, then load the selected knowledge in greater depth.

#### Conceptual stages

```text
Task Context
     ↓
Discovery Metadata
     ↓
Candidate Selection
     ↓
Required Knowledge Content
     ↓
Optional Supporting Resources
```

#### Consequences

- Discovery metadata MUST be distinguishable from full knowledge content.
- Metadata SHOULD communicate purpose, applicability, triggers, dependencies, and expected outputs.
- Selection SHOULD minimise irrelevant context while preserving required dependencies.
- Progressive disclosure MUST NOT conceal mandatory constraints needed to make a safe selection.
- Context cost SHOULD be measurable.

### AP-08 — Provenance and Epistemic Status Must Be Explicit

#### Normative statement

> Material engineering guidance in AEKF MUST identify its origin and the nature of its authority.

#### Rationale

Not all engineering knowledge has the same status.

A legal requirement, an international standard, a project policy, a widely used practice, an academic finding, and an unvalidated AEKF hypothesis must not be presented as equivalent.

Without provenance, an agent cannot reliably distinguish mandatory constraints from recommendations or experiments.

#### Consequences

AEKF SHOULD be able to distinguish at least:

- normative law or regulation;
- normative standard;
- organisational or project policy;
- established professional practice;
- referenced academic or empirical evidence;
- contextual engineering judgement;
- AEKF hypothesis or experiment;
- generated or inferred guidance.

Knowledge SHOULD identify relevant source versions, dates, jurisdictions, project scopes, or other applicability boundaries when they affect interpretation.

Conflicting guidance MUST remain traceable to its sources rather than being silently merged into false consensus.

### AP-09 — Knowledge Must Be Composable Without Uncontrolled Duplication

#### Normative statement

> AEKF knowledge elements MUST be reusable and composable without requiring independent copies of the same engineering rule for every workflow, domain, or consumer.

#### Rationale

Duplication creates semantic drift. When the same guidance is copied into architecture, testing, security, and agent-specific documents, later updates rarely reach every copy consistently.

Composition should allow shared knowledge to participate in multiple contexts while retaining one authoritative identity.

#### Consequences

- Reusable knowledge SHOULD have stable identity.
- Relationships and references SHOULD be preferred over copying complete guidance.
- A composed workflow MAY provide contextual framing, but it MUST preserve traceability to source knowledge.
- Renderers MAY duplicate text in generated artefacts when required by the consumer, but generated duplication MUST NOT become a second source of truth.
- Conflicts created by composition MUST be detectable or explicitly resolved.

### AP-10 — Quality Claims Must Be Measurable and Testable

#### Normative statement

> AEKF architecture MUST permit empirical evaluation of its knowledge model, discovery mechanisms, compositions, and rendered outputs.

#### Rationale

Claims such as “better context,” “more discoverable,” “higher quality,” or “more reusable” are not meaningful unless they can be observed and compared.

AEKF is an engineering framework and should be capable of testing its own architectural hypotheses.

#### Consequences

The architecture SHOULD enable measurement of properties such as:

- domain and decision coverage;
- discoverability;
- selection precision and recall;
- dependency completeness;
- context size and loading cost;
- conflict rate;
- reuse and duplication;
- maintenance effort;
- renderer fidelity;
- decision quality and consistency;
- human correction or override rate.

Future RFCs MUST define specific metrics, datasets, experiments, and acceptance criteria before presenting these properties as validated benefits.

### AP-11 — Human Authority and Accountability Must Be Preserved

#### Normative statement

> AEKF MUST support human engineering judgement and MUST NOT imply that knowledge delivery to an AI agent transfers professional accountability to the framework or the agent.

#### Rationale

Engineering decisions depend on context, risk, incomplete evidence, stakeholder priorities, legal obligations, and professional responsibility.

AEKF can structure knowledge, expose trade-offs, and improve consistency. It cannot guarantee that a generated recommendation is correct for every project.

#### Consequences

- Consumers SHOULD be able to inspect the knowledge and rationale behind material recommendations.
- Human users MUST be able to reject or override generated guidance.
- Overrides SHOULD be recordable when traceability is required.
- High-risk or regulated decisions MAY require explicit human approval.
- A renderer MUST NOT present uncertain or advisory guidance as an absolute requirement unless the source knowledge justifies that requirement level.

### AP-12 — Knowledge Must Be Evolvable and Traceable

#### Normative statement

> AEKF MUST support controlled evolution of knowledge while preserving stable identity, version history, and traceability of material changes.

#### Rationale

Engineering knowledge changes as standards, laws, technologies, evidence, and professional practices evolve.

A consumer must be able to determine which version of the knowledge produced an instruction, recommendation, workflow, or artefact.

#### Consequences

- Material knowledge elements SHOULD have stable identifiers.
- Changes that alter meaning SHOULD create a new version or explicit supersession relationship.
- Rendered artefacts SHOULD be traceable to their source versions.
- Deprecation MUST NOT silently erase historical rationale.
- Compatibility and migration implications SHOULD be documented when the model evolves.
- A future specification MUST define the exact versioning and identity rules.

## Principle Conformance

Future AEKF RFCs SHOULD include a conformance section that identifies:

- which architectural principles the proposal implements;
- which principles constrain the proposal;
- any known tension or exception;
- how conformance will be validated.

A proposal that conflicts with an Accepted architectural principle MUST either:

1. revise the proposal;
2. propose an amendment to this RFC; or
3. explicitly supersede the affected principle through a new RFC.

A principle SHOULD NOT be weakened merely because a current vendor integration cannot implement it directly.

## Rationale for Principle-Level Architecture

The Knowledge Model defined in later RFCs will be easier to change than the principles that constrain it.

For example, AEKF may eventually represent knowledge through:

- typed documents;
- relational schemas;
- a property graph;
- a hybrid model;
- a compiled intermediate representation.

Any of these may be acceptable if it preserves durability, vendor neutrality, separation of concerns, graph-aware relationships, provenance, progressive disclosure, composition, measurability, and traceability.

This allows AEKF to experiment with implementation while protecting its architectural intent.

## Alternatives Considered

### Adopt the SWEBOK hierarchy as the complete AEKF model

This provides an established taxonomy and strong domain coverage.

It is rejected as the complete model because a disciplinary hierarchy does not by itself represent task context, decision paths, dependencies, progressive loading, provenance conflicts, or consumer-specific rendering.

SWEBOK remains an important baseline under AP-04.

### Model current AI-agent mechanisms directly

This would create entities such as Skill, Rule, Hook, Memory, and Subagent in the core model.

It is rejected because these concepts are vendor-specific and likely to evolve faster than engineering knowledge.

They may still appear in downstream adapters.

### Treat Markdown files as the knowledge model

This would provide a simple and accessible implementation.

It is rejected as an architectural assumption because files do not automatically define semantic identity, typed relationships, provenance, composition, or evaluation.

Markdown may remain one authoring or rendering format.

### Make Engineering Decision the only knowledge entity immediately

This would create a clear decision-oriented structure.

It is rejected at this stage because some required knowledge may be better represented as evidence, constraints, workflows, definitions, examples, policies, or resources.

RFC-002 and RFC-003 must determine the fundamental entity set.

### Require a graph database

This would naturally support graph traversal.

It is rejected as a principle because graph semantics do not require one storage technology. The representation should be selected after the conceptual model is understood.

### Permit fully autonomous application of all knowledge

This would maximise automation.

It is rejected as a general principle because decision authority and accountability vary by risk, regulation, organisational policy, and project context.

## Risks and Trade-offs

### Abstraction overhead

A vendor-neutral model may require more initial design effort than writing direct instructions for one agent.

This cost is justified only if reuse, consistency, and evolvability can be demonstrated.

### Lowest-common-denominator design

Vendor neutrality may tempt the framework to avoid capabilities that exist only in some consumers.

AEKF should instead preserve a neutral semantic core while allowing richer renderer-specific extensions.

### Metadata burden

Provenance, applicability, identity, and discovery metadata increase authoring and maintenance effort.

Future RFCs must identify the minimum metadata necessary for reliable selection and traceability.

### Premature formalisation

Formal schemas can freeze weak assumptions before the knowledge model has been validated.

For this reason, this RFC defines constraints rather than a complete schema.

### Measurement complexity

Decision quality and context effectiveness are difficult to measure objectively.

AEKF must avoid replacing untested intuition with misleading metrics.

### Human overreliance

Traceable and well-structured knowledge may appear more authoritative than the underlying evidence warrants.

Epistemic status and human authority are therefore explicit principles.

## Compatibility and Migration Impact

This RFC introduces no implementation migration because AEKF does not yet have an accepted knowledge specification.

However, existing experimental agent instructions, skills, rules, templates, and knowledge packs should be treated as candidate source material rather than assumed components of the final core model.

Future migrations may need to separate:

- durable engineering knowledge;
- discovery metadata;
- workflow composition;
- renderer-specific instructions;
- vendor-specific extensions.

## Validation Strategy

The proposed principles should be challenged before this RFC becomes Accepted.

Validation should include at least the following activities.

### Cross-domain modelling

Attempt to represent examples from multiple SWEBOK Knowledge Areas, including requirements, architecture, construction, testing, security, quality, maintenance, and operations.

The principles should not work only for architecture or backend development.

### Multi-consumer rendering

Represent one source knowledge set and render it for at least two materially different consumers.

The experiment should identify semantic loss, duplication, and vendor-specific leakage.

### Task-specific discovery

Evaluate whether a narrow engineering task can discover the required knowledge without loading an entire domain.

### Provenance and conflict experiment

Represent two valid but conflicting sources of guidance and test whether AEKF preserves source, scope, authority, and resolution context.

### Evolution experiment

Change or supersede one knowledge element and verify that dependencies and rendered artefacts can identify the affected version.

### Human review

Ask experienced engineers to assess whether delivered knowledge exposes enough rationale, evidence, and uncertainty to support responsible decisions.

## Open Questions

- Which entities are fundamental to the AEKF Knowledge Model?
- Is Engineering Decision a first-class entity, a relationship, an output, or several of these?
- Which relationship types are required in the initial model?
- What is the minimum viable discovery metadata?
- How should epistemic status and authority be represented?
- How should project-specific knowledge override or refine general knowledge?
- Which metrics can reliably evaluate decision quality?
- How should renderer-specific extensions remain traceable to the neutral core?
- What versioning model is appropriate for individual knowledge elements and composed releases?
- Should AEKF define formal conformance levels for implementations?

These questions are intentionally deferred to RFC-002 and later RFCs.

## Proposed Decision

Accept the twelve architectural principles in this RFC as constraints on subsequent AEKF design:

1. durable engineering knowledge;
2. vendor-neutral core model;
3. separation of knowledge, selection, and delivery;
4. SWEBOK as baseline without taxonomy lock-in;
5. decision-oriented use;
6. graph-aware relationships;
7. progressive disclosure;
8. explicit provenance and epistemic status;
9. composability without uncontrolled duplication;
10. measurable and testable quality claims;
11. preserved human authority and accountability;
12. evolvability and traceability.

Acceptance of these principles would not accept any particular entity schema, storage format, renderer design, discovery algorithm, or metric implementation.

## References

- [RFC-000 — Why AEKF Exists](000-why-aekf-exists.md)
- [IEEE Computer Society — Software Engineering Body of Knowledge](https://www.computer.org/education/bodies-of-knowledge/software-engineering)
- [SWEBOK Version 4](https://www.computer.org/education/bodies-of-knowledge/software-engineering/v4)
- [BCP 14](https://www.rfc-editor.org/info/bcp14/)
- [RFC 2119](https://www.rfc-editor.org/info/rfc2119/)
- [RFC 8174](https://www.rfc-editor.org/info/rfc8174/)
