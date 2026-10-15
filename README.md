[README.md](https://github.com/user-attachments/files/33489461/README.md)
# Architecture Playbook

Practical guidance for understanding application architectures, making design decisions and planning modernisation.

This playbook helps architects, engineers, technical leads and application owners describe systems consistently, compare options and record the evidence behind decisions. Use it for individual application reviews and portfolio-level assessments.

## Contents

- [Start here](#start-here)
- [Reference library](#reference-library)
- [How to use the playbook](#how-to-use-the-playbook)
- [Working principles](#working-principles)
- [Recording architecture decisions](#recording-architecture-decisions)
- [Contributing](#contributing)

## Start here

Choose the activity that matches your immediate need:

| I need to… | Start with… |
| --- | --- |
| Classify an existing application | The [practical classification workflow](architecture-patterns-reference.md#practical-classification-workflow) |
| Understand a pattern and its trade-offs | The [architecture patterns overview](architecture-patterns-reference.md#architecture-patterns-overview) |
| Compare modernisation options | The [modernisation decision guidance](architecture-patterns-reference.md#modernisation-decision-guidance) |
| Resolve ambiguous classification labels | The [source inconsistencies and editorial decisions](architecture-patterns-reference.md#source-inconsistencies-and-editorial-decisions) |

## Reference library

### Application architecture patterns

The [Application Architecture Patterns reference](architecture-patterns-reference.md) provides definitions, Mermaid diagrams, examples, selection criteria and modernisation considerations for:

- Monolith
- Layered / N-Tier
- Modular Monolith
- Microservices
- Service-Oriented Architecture / Service-Based
- Package / Commercial Off-The-Shelf (COTS)
- Software as a Service (SaaS)
- Low-Code / No-Code
- Mainframe / Legacy Platform

It also explains how to use **Other**, **Unknown** and **Not Assessed** without confusing technical characteristics with assessment status.

The classification is intended for **Level 0 portfolio assessment**: a concise description of an application's predominant structure, implementation model or delivery model. Several categories can apply to the same application. Select a primary category for the decision at hand and record relevant secondary characteristics.

## How to use the playbook

1. **Define the question and scope.** Identify the application, business capabilities, system boundaries and decision being considered.
2. **Gather evidence.** Review deployment diagrams, release processes, interfaces, module boundaries, platform dependencies and operating responsibilities with the people who maintain the system.
3. **Describe the current state.** Select a defensible classification and write a short architecture summary. Record evidence gaps explicitly.
4. **Evaluate options.** Compare business outcomes, quality requirements, operational capability, dependencies, cost and transition risk. Include retaining or improving the current architecture where appropriate.
5. **Record the decision.** Capture the rationale, trade-offs, consequences and evidence required to validate the selected approach.
6. **Review as the system changes.** Revisit the assessment when requirements, deployment boundaries, ownership, vendor arrangements or platform dependencies change.

For portfolio work, apply the same definitions across applications and keep current-state assessments separate from proposed target architectures.

## Working principles

- **Start with the problem.** Choose patterns to meet demonstrated needs and constraints.
- **Use evidence.** Separate observed facts, assumptions, proposals and approved decisions.
- **Make trade-offs explicit.** Every approach has strengths, limitations and operating costs.
- **Keep complexity proportionate.** A modular monolith can be a suitable target; microservices require a reason and the capability to operate them.
- **Describe overlapping dimensions.** Structure, sourcing, delivery model and platform dependency answer different questions.
- **Treat modernisation as a transition.** Consider coexistence, data migration, cutover, rollback and retirement where relevant.
- **Design across concerns.** Address security, privacy, resilience, accessibility, interoperability, data governance and operability throughout the work.
- **Keep guidance usable.** Prefer clear explanations, practical examples and enough detail to support a decision.

## Recording architecture decisions

Use a short Architecture Decision Record (ADR) when a choice has significant consequences or needs a durable explanation.

| Field | What to capture |
| --- | --- |
| Title and status | The decision and whether it is proposed, accepted, superseded or rejected |
| Context | The problem, scope, requirements and constraints |
| Evidence | Relevant observations, measurements, references and assumptions |
| Options | Viable alternatives and their trade-offs |
| Decision | The selected approach and why it was chosen |
| Consequences | Benefits, limitations, risks and operational responsibilities |
| Validation and review | How the decision will be tested and what would trigger reconsideration |

Scale the record to the significance of the decision. Link supporting diagrams and assessments rather than repeating them.

## Contributing

Contributions can clarify guidance, correct technical errors, add examples or introduce a reusable reference.

When proposing a change:

1. Explain the problem the change addresses and its intended audience.
2. Use consistent terminology and distinguish general guidance from organisation-specific conventions.
3. Include practical selection criteria, trade-offs and modernisation implications where relevant.
4. Identify illustrative examples and support externally sourced claims with references.
5. Use fenced `mermaid` blocks for diagrams and check that they render without clipped labels or ambiguous relationships.
6. Check relative links and update the reference library when adding a page.
7. Submit a pull request describing the change and how it was checked.

Avoid including credentials, sensitive system details or confidential application information in contributions.

---

This playbook supports professional judgement. Apply the guidance in the context of the organisation's requirements, standards and delivery capabilities.
