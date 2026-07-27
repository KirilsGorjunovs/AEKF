# RFC-000 — Why AEKF Exists

- **Status:** Draft
- **Version:** 0.2
- **Authors:** AEKF Working Group
- **Created:** 2026-07-27
- **Updated:** 2026-07-27
- **Discussion:** TBD
- **Supersedes:** None
- **Superseded by:** None

## Abstract

Software engineering knowledge for AI agents is currently organised around the capabilities and limitations of individual tools rather than around the engineering decisions themselves.

Modern coding assistants expose concepts such as prompts, rules, memories, skills, hooks, subagents, and repository instructions. Although useful, these concepts are implementation mechanisms, not engineering concepts.

As a result, engineering knowledge becomes tightly coupled to a particular AI platform and must be rewritten whenever the underlying tooling changes.

This RFC proposes a different approach.

Instead of organising knowledge around AI agents, AEKF organises knowledge around **software engineering decisions**.

Agents become consumers of that knowledge rather than its owners.

## Problem Statement

Today's AI-assisted software engineering suffers from several recurring problems.

### Vendor-specific knowledge

Engineering guidance is written specifically for:

- Codex Skills;
- Claude Skills;
- Cursor Rules;
- Windsurf Rules;
- repository prompts;
- IDE-specific instructions.

Although the engineering knowledge is largely identical, it must be rewritten for every platform.

The knowledge itself becomes fragmented.

### Knowledge duplication

The same architectural guidance frequently appears in multiple places:

- repository instructions;
- skills;
- templates;
- documentation;
- prompts.

Over time, these copies diverge.

Maintaining consistency becomes increasingly difficult.

### Context overload

Large instruction files often contain information that is irrelevant to the current task.

An agent implementing a REST endpoint may unnecessarily load guidance for:

- persistence;
- architecture decisions;
- migrations;
- security reviews;
- reporting.

Most of this information never contributes to the actual engineering decision.

### Tool-driven architecture

Current systems often model the structure of the AI tool rather than the structure of engineering knowledge.

Examples include:

- one large Architecture skill;
- one large Testing skill;
- one large Security skill.

These structures mirror human organisational roles rather than the actual decisions required during software development.

## Vision

AEKF aims to define a vendor-neutral architecture for software engineering knowledge.

Its purpose is not to replace AI coding assistants.

Its purpose is to provide a stable engineering knowledge model that can be consumed by any present or future coding agent.

The primary artefact is therefore not a prompt.

It is a knowledge architecture.

## Core Principle

> Engineering knowledge must outlive the AI agent consuming it.

An engineering principle should remain valid whether it is consumed by:

- Codex;
- Claude Code;
- Gemini;
- Cursor;
- a future coding assistant;
- or a human engineer.

Only the rendering mechanism should change.

The knowledge itself should remain stable.

## From Skills to Engineering Decisions

Traditional agent architectures are organised around skills.

AEKF proposes organising knowledge around engineering decisions instead.

For example, instead of:

> Aggregate Design Skill

AEKF models:

> Should this object become an Aggregate Root?

Instead of:

> Security Skill

AEKF models:

> Should this endpoint require authorisation?

Instead of:

> Persistence Skill

AEKF models:

> Should this data belong to the same transactional boundary?

Engineering decisions become the primary unit of knowledge.

Skills become implementation artefacts generated from that knowledge.

## Knowledge as a Graph

Software engineering is not a tree.

It is a graph of connected decisions.

A single task rarely requires an entire engineering discipline. Instead, it requires a small set of related decisions.

For example:

```text
Implement secured REST endpoint

↓

Does the endpoint require authentication?

↓

Does the endpoint require authorisation?

↓

Which module owns the use case?

↓

Should the operation be transactional?

↓

What tests protect the behaviour?
```

The objective of AEKF is to model these decision paths explicitly.

## Scope

AEKF defines how engineering knowledge should be:

- organised;
- discovered;
- composed;
- maintained;
- evaluated;
- rendered for different consumers.

AEKF does not attempt to prescribe one programming language, framework, architectural style, or development methodology.

## Non-Goals

AEKF is not:

- a prompt library;
- a collection of Markdown files;
- a Codex framework;
- a Claude framework;
- a replacement for software architecture;
- a replacement for engineering judgement.

It defines the structure of engineering knowledge rather than specific engineering solutions.

## Expected Benefits

If successful, AEKF should provide:

- reusable engineering knowledge independent of vendors;
- smaller task-specific context;
- less duplicated guidance;
- measurable knowledge quality;
- composable engineering workflows;
- easier maintenance of engineering instructions;
- improved consistency across AI coding assistants.

## Research Questions

This RFC intentionally leaves several questions unanswered:

- What is the optimal unit of engineering knowledge?
- Should skills exist as first-class concepts?
- How should engineering decisions be connected?
- How should knowledge quality be measured?
- What constitutes an effective discovery mechanism?
- How should knowledge be rendered for different AI agents?
- How should conflicting engineering guidance be resolved?
- How should human judgement interact with generated agent instructions?

These questions will be addressed in subsequent RFCs.

## Proposed Roadmap

```text
RFC-000 — Why AEKF Exists

↓

RFC-001 — Architectural Principles

↓

RFC-002 — Knowledge Model

↓

RFC-003 — Knowledge Units

↓

RFC-004 — Decision Graph

↓

RFC-005 — Discovery and Context Loading

↓

RFC-006 — Evaluation Metrics

↓

RFC-007 — Rendering Architecture

↓

AEKF Specification v1.0
```

## Open Questions

- Is an Engineering Decision the correct primary knowledge unit?
- Which concepts are fundamental and which are merely renderer-specific adapters?
- Should the AEKF source of truth be graph-based, document-based, or schema-based?
- How should accepted RFCs contribute to the normative specification?
- How can knowledge remain vendor-neutral while still generating effective vendor-specific instructions?
- What evidence is required before a hypothesis becomes a normative principle?

## Closing Statement

Software engineering knowledge should not be rewritten every time AI tooling evolves.

Instead, engineering knowledge should exist as an independent, structured, measurable body of knowledge that any capable AI agent can consume.

AEKF is an attempt to define that architecture.

Whether the underlying implementation uses prompts, skills, rules, memories, or mechanisms that do not yet exist should become an implementation detail rather than the foundation of the system.
