# Application Architecture Patterns — Technical Reference

A practical reference for classifying applications in an ICT portfolio and understanding how their architecture affects modernisation choices.

**Scope:** Level 0 classification—a concise, portfolio-level description of an application's predominant architecture, implementation model or delivery model. This reference preserves the supplied material and adds explicit classification rules where the source is inconsistent. Examples and diagrams are illustrative, not assessments of actual systems.

## Table of contents

- [Classification principles](#classification-principles)
- [Architecture patterns overview](#architecture-patterns-overview)
- [Monolith](#monolith)
- [Layered / N-Tier](#layered--n-tier)
- [Modular Monolith](#modular-monolith)
- [Microservices](#microservices)
- [SOA / Service-Based](#soa--service-based)
- [Package / COTS](#package--cots)
- [SaaS](#saas)
- [Low-Code / No-Code](#low-code--no-code)
- [Other](#other)
- [Unknown and Not Assessed](#unknown-and-not-assessed)
- [Mainframe / Legacy Platform](#mainframe--legacy-platform)
- [Practical classification workflow](#practical-classification-workflow)
- [Modernisation decision guidance](#modernisation-decision-guidance)
- [Source inconsistencies and editorial decisions](#source-inconsistencies-and-editorial-decisions)

## Classification principles

The source categories serve a practical portfolio purpose, but describe different dimensions:

| Dimension | Relevant categories |
| --- | --- |
| Application structure and deployment | Monolith, Layered / N-Tier, Modular Monolith, Microservices |
| Service reuse and integration | SOA / Service-Based |
| Product and implementation model | Package / COTS, Low-Code / No-Code |
| Service delivery model | SaaS |
| Platform dependency | Mainframe / Legacy Platform |
| Exception or evidence state | Other, Unknown, Not Assessed |

These categories are not mutually exclusive. A commercial product may be layered; a mainframe application may be monolithic; a SaaS product may internally use microservices. Choose a **Primary Pattern** for the portfolio decision and capture known secondary characteristics in **Architecture Summary** or separate fields. Do not infer a vendor's internal architecture from its delivery model.

Classify the **current state** from evidence. Record proposed target architecture separately. An architecture label is not a maturity score or an automatic recommendation to replace a system.

## Architecture patterns overview

| **Architecture pattern** | **What it means** | **Typical strengths** | **Typical concerns / modernization implications** |
|----------------------|---------------|-------------------|-----------------------------------------------|
| **Monolith**         | Most functions are built, deployed, and scaled as one application. Components may be logically separated but remain tightly packaged. | Simple deployment and operations for smaller systems; straightforward transactions. | A small change may require testing and redeploying the whole system. As complexity grows, releases, scaling and technology refresh can become difficult. Consider modularisation, selective refactoring, or replacement—not automatic decomposition. |
| **Layered / N-Tier** | Responsibilities are organised into presentation, business logic and data access layers; physical tiers may be deployed separately. | Clear technical separation; familiar enterprise application structure. | Shared releases and dependencies may limit change and scaling. Assess modularisation and tier-specific improvements before considering decomposition. |
| **Modular Monolith** | One deployable application with deliberately separated internal modules and controlled interfaces. | Lower operational complexity than distributed services; modules can be maintained independently within one codebase. | Weak governance can allow modules to become tightly coupled. Often a viable target architecture rather than merely an interim state. |
| **Service-Oriented Architecture** | Business functions are provided through reusable services, commonly integrated through an enterprise service bus or shared middleware. | Enables reuse and integration across multiple systems; supports enterprise-wide services. | Shared middleware may become a bottleneck or concentration risk. Service ownership, interface lifecycle, orchestration complexity and vendor support need assessment. |
| **Microservices**    | The application is divided into small, independently deployable services aligned to business domains. | Independent releases and scaling; team autonomy; better isolation when properly engineered. | Introduces significant distributed-system and operational complexity. Requires mature automation, observability, security, service ownership, testing and platform engineering. It is not automatically the best target for every system. |
| **Package / COTS**   | Commercial off-the-shelf software is installed or hosted for the organisation and configured to meet its needs. | Faster access to established capabilities; vendor-supported product lifecycle; reduced custom development. | Heavy customisation can make upgrades difficult. Assess vendor roadmap, licensing, integration, data access, support, exit provisions and alignment with standard product processes. |
| **SaaS**             | A vendor operates the application as a subscription-based service, normally shared or logically isolated across customers. | Rapid provisioning, regular upgrades and reduced infrastructure responsibility. | Less control over release timing, hosting design and customisation. Assess data location, security, integration, service continuity, portability, vendor concentration and exit strategy. |
| **Low-Code / No-Code** | Applications are assembled using visual tools, reusable components and platform-managed services, with limited conventional coding. | Rapid delivery, useful for workflow and departmental solutions, and potentially lower development effort. | Can produce uncontrolled application growth, weak lifecycle ownership and platform lock-in. Governance is needed for data, security, testing, integration, reuse and retirement. |
| **Mainframe / Legacy Platform** | The application depends on an older or specialised computing platform, language, database or runtime that materially shapes its architecture. | Often highly reliable and capable of processing large transaction volumes; may contain critical business logic. | Skills scarcity, product lifecycle, high coupling, limited automation and difficult integration may create strategic risk. Modernization could involve encapsulation, rehosting, replatforming, incremental replacement or retirement. |
| **Other**            | A material pattern not adequately represented by the standard choices—for example event-driven, serverless, data-product, embedded or edge-centric architecture. | Preserves accuracy where a standard category would be misleading. | The pattern should be named and briefly explained in the Architecture Summary field. |
| **Unknown**          | Available evidence is insufficient to classify the architecture confidently. | Makes an evidence gap visible rather than forcing an unreliable answer. | Assign follow-up action before making major sequencing, cost, resilience or platform decisions. |
| **Not Assessed** | Architecture discovery has not yet been performed or completed. | Makes assessment coverage visible. | Schedule discovery; distinguish this workflow state from an inconclusive assessment recorded as Unknown. |

## Monolith

Most functions are built, deployed and scaled as one application. Internal components may be logically separated while remaining packaged together.

```text
Users → Single application → Database
        [UI | business logic | data access]
```

**Select this when:** the application is primarily one deployment unit and there is insufficient evidence for a more specific structural classification. If deliberately controlled business-domain modules exist, use **Modular Monolith**. If technical layering is the defining characteristic, record **Layered / N-Tier**, with monolithic deployment noted in the summary.

**Architecture Summary example**

> Single deployable application containing presentation, business logic and data access components, released and scaled together.

A monolith can simplify operations and transactions. As complexity grows, whole-application testing, deployment and scaling may become constraints. Consider modularisation, selective refactoring or replacement based on evidence; decomposition is not automatic.

## Layered / N-Tier

The classic enterprise application.

```text
User Interface
      ↓
Business Logic
      ↓
Data Access
      ↓
Database
```

The concern is separated primarily by **technical layers**.

A traditional Java or .NET enterprise application where the presentation, business and database tiers are distinct but form a single overall application is a typical example.

**Select this when:**

* application is structured principally around technical layers;
* components are relatively tightly associated;
* application is generally built/released as one solution.

**Level 0 example**

> `Layered / N-Tier`

Architecture Summary:

> Three-tier web application comprising presentation, application and database tiers.


---

## Modular Monolith

This is an important distinction from a conventional monolith.

```text
┌───────────────────────────────┐
│      Single Application       │
│                               │
│ ┌────────┐ ┌────────┐ ┌─────┐ │
│ │ Case   │ │ Person │ │Audit│ │
│ │ Module │ │ Module │ │     │ │
│ └────────┘ └────────┘ └─────┘ │
└───────────────────────────────┘
```

It is still **one deployable application**, but internally it has strong modular boundaries.

**Select this when:**

* modules correspond to distinct business capabilities;
* boundaries between modules are intentionally controlled;
* modules are comparatively loosely coupled;
* the application is nevertheless deployed principally as one unit.

A modular monolith can be a viable modernisation target. Microservices should be selected only when their benefits justify the additional complexity.

**Architecture Summary**

> Single deployable application structured into independently maintained business-domain modules.


---

## Microservices

Here the application's capabilities are separated into **independently deployable services**.

```text
                 ┌→ Case Service
API Gateway ─────┼→ Person Service
                 ├→ Document Service
                 └→ Notification Service
```

**Select this when:**

* application consists of multiple services;
* services represent distinct capabilities/domains;
* services have clear interfaces;
* services can generally be deployed independently.

Simply having REST APIs **does not make something microservices**.

A Layered/N-Tier application can expose 100 APIs and still fundamentally be a layered application.

**Architecture Summary**

> Domain-aligned microservices with independently deployable services communicating through APIs and events.


---

## SOA / Service-Based

SOA emphasises **reusable business or enterprise services**.

```text
Application A ─┐
Application B ─┼──→ Shared Services
Application C ─┘
```

For example:

```text
              ┌→ Identity Service
Systems ──────┼→ Document Service
              ├→ Payment Service
              └→ Notification Service
```

**Select this when:**

* major capabilities are implemented as reusable services;
* those services are consumed by multiple functions/applications;
* service boundaries are a fundamental element of the solution architecture.

### SOA vs Microservices

A useful Level 0 distinction:

**SOA:** “How can applications reuse common enterprise/business services?”

**Microservices:** “How is this particular application decomposed into independently deployable services?”

There can naturally be overlap.


---

## Package / COTS

This means **Commercial Off-The-Shelf** software is the application's primary implementation model.

The organisation is principally **configuring/customising a product**, rather than designing the application's internal architecture itself.

```text
Users
  ↓
┌────────────────────┐
│ Commercial Product │
│                    │
│ Configuration      │
│ Extensions         │
│ Workflows          │
└────────────────────┘
        ↓
    Integrations
```

Examples might include commercial:

* ERP;
* CRM;
* case-management;
* document-management;
* security products.

**Why keep this value?**

For portfolio modernisation, distinguishing a bespoke Layered application from COTS is useful because the available modernisation choices are fundamentally different.

For COTS, you may be constrained by the vendor's architecture.


---

## SaaS

SaaS is slightly different again.

```text
Organisation / Consumer
      ↓
Configuration / APIs
      ↓
Vendor-managed SaaS
```

The application is primarily **consumed as a service**, with the provider managing the underlying software.

**Select this when:**

* SaaS is the principal application;
* users consume a vendor-operated service;
* configuration and integration are more important than internal application development.

**SaaS vs COTS**

Use the distinction:

**COTS:** “We operate a commercial product.”

**SaaS:** “We consume the application as a provider-managed service.”

This isn't a perfect academic distinction because SaaS is technically a delivery model rather than an application architecture. For **Level 0 portfolio classification**, however, it is useful because it tells you something important about the available modernisation path.


---

## Low-Code / No-Code

The application's primary implementation is built using a **low-code application platform**.

```text
Low-Code Platform
       │
 ┌─────┼─────┐
 ↓     ↓     ↓
 UI  Workflow Data
```

**Select this when:**

The low-code platform is fundamental to how the application is developed and maintained, rather than being used for one small workflow around an otherwise conventional application.

This distinction matters because:

> **“Uses low-code somewhere” ≠ “Low-Code architecture pattern”.**

For example, a microservices application that happens to use a low-code tool for an administrative workflow should remain **Microservices** in the Primary Pattern column.

Its **Low-Code Development Platform** modernisation capability can separately be marked as implemented/partial.


---

**Assessment and exception values**

## Other

Use this when the application has been assessed, but none of the controlled categories accurately describes its architecture.

Require a short explanation in **Architecture Summary** whenever someone selects Other.

For example:

> `Other`

> Architecture Summary: “Specialised embedded application architecture associated with operational equipment.”

This prevents **Other** becoming the easy default.


---

## Unknown and Not Assessed

The source uses **Unknown** in its overview and **Not Assessed** in its detailed guidance for the same broad condition: insufficient evidence to classify the application. Neither means no architecture, legacy, none or not applicable.

For a more precise portfolio vocabulary, this reference recommends:

| Value | Recommended meaning | Next action |
| --- | --- | --- |
| **Not Assessed** | Discovery has not started or is incomplete. | Assign an assessor and complete discovery. |
| **Unknown** | Discovery has been attempted, but the evidence remains insufficient or contradictory. | Record the missing evidence, owner and follow-up action. |

This distinction is an editorial recommendation, not a distinction established by the source. If the portfolio supports only one value, select a single canonical label and document the mapping from both source labels. Do not infer completed assessments from historical records without checking their evidence.

Prefer storing assessment status separately from the architecture pattern. Until evidence supports a pattern, leave that field unclassified or use the agreed exceptional value required by the portfolio tool.

## Mainframe / Legacy Platform

The application depends on an older or specialised platform, language, database or runtime that materially shapes its architecture. This category appears in the source overview but has no detailed source section; the guidance below expands that entry.

**Select this when:** platform dependency is the most decision-relevant characteristic for the portfolio. Record the application structure separately when known. Age alone does not establish technical risk or justify replacement.

**Architecture Summary example**

> Transaction-processing application on a specialised platform, with critical business rules and tightly coupled data and integration dependencies.

Such systems may be reliable and support large transaction volumes. Assess skills availability, product support, coupling, automation and integration constraints. Options include encapsulation, rehosting, replatforming, incremental replacement and retirement.

## Practical classification workflow

1. **Define the boundary.** Identify the application being assessed, its users, business capabilities, integrations and what is outside scope. Avoid classifying an entire estate as one application.
2. **Gather evidence.** Review deployment diagrams, release pipelines, module boundaries, service interfaces, platform records, product contracts and operating responsibilities. Validate findings with the application owner or technical team.
3. **Record assessment status.** Use Not Assessed while discovery is incomplete. Use Unknown when investigation cannot establish a defensible classification. Record evidence gaps and follow-up ownership.
4. **Identify the implementation and operating model.** Determine whether the application is primarily provider-operated SaaS, an operated commercial package, a low-code platform application or bespoke software. An incidental tool or workflow does not determine the primary category.
5. **Inspect structure and release independence.** For bespoke applications, distinguish technical layers, controlled business modules and independently deployable services. Multiple APIs, containers or repositories alone do not demonstrate microservices.
6. **Assess service reuse and platform dependency.** Determine whether shared enterprise services or a specialised platform materially define the solution. Capture overlapping characteristics explicitly.
7. **Select and explain the primary category.** Use the characteristic that best supports the portfolio's modernisation decision. Apply a consistent local rule across comparable applications; use Other only after assessment and name the actual pattern.
8. **Validate and maintain the record.** Record the evidence, confidence, assessor and assessment date. Review the classification when deployment boundaries, ownership, vendor arrangements or platform dependencies change.

### Suggested portfolio record

| Field | What to record |
| --- | --- |
| Application and scope | Identifier, name and assessed boundary |
| Primary Pattern | One agreed category, or an agreed exceptional value |
| Architecture Summary | Structure, deployment, integration and relevant secondary characteristics |
| Assessment Status | Not Assessed, In Progress, Assessed or Inconclusive, if managed separately |
| Evidence and confidence | Evidence references, confidence and unresolved questions |
| Ownership and review | Application owner, assessor, assessment date and review trigger |
| Current constraints | Release coupling, platform or vendor limits, operational dependencies |
| Modernisation direction | Proposed target, rationale and evidence needed; distinguish proposals from approved decisions |

These fields and status values are suggested additions, not a schema supplied by the source.

### Classification examples

| Observed situation | Primary classification | Supporting explanation |
| --- | --- | --- |
| Java or .NET application with presentation, business and data tiers, released together | Layered / N-Tier | Technical layers define the structure; note monolithic application deployment. |
| Case, person and audit modules with controlled interfaces in one deployable application | Modular Monolith | Business modules have deliberate boundaries; deployment remains unified. |
| Case, person, document and notification services with independent releases | Microservices | Domain boundaries and release independence are evidenced. |
| Several applications reuse shared identity, document, payment and notification services | SOA / Service-Based | Enterprise service reuse is fundamental to the assessed solution. |
| Commercial ERP configured and operated for the organisation | Package / COTS | Product configuration, upgradeability and vendor constraints shape change. |
| Vendor-operated subscription application accessed through configuration and APIs | SaaS | The organisation consumes the provider-managed application. |
| Microservices application using low-code for one administrative workflow | Microservices | Record low-code capability separately as implemented or partial where applicable. |
| Specialised embedded application associated with operational equipment | Other | Name and explain the embedded architecture in Architecture Summary. |
| No discovery has been undertaken | Not Assessed | Assign discovery; do not infer an architecture. |
| Discovery produces insufficient or contradictory evidence | Unknown | Record gaps and the next validation action. |

## Modernisation decision guidance

Start with required outcomes and observed constraints, then evaluate the available options. The architecture label narrows the questions; it does not determine the answer.

| Current category | Practical modernisation focus |
| --- | --- |
| Monolith / Layered / N-Tier | Improve release automation, identify coupling, strengthen module boundaries and assess selective refactoring or replacement. |
| Modular Monolith | Preserve controlled interfaces and ownership; retain as a target where it meets requirements. |
| SOA / Service-Based | Assess shared middleware capacity and concentration risk, service ownership, interface lifecycle, orchestration and vendor support. |
| Microservices | Verify automation, observability, security, testing, ownership and platform engineering can sustain distributed operations. |
| Package / COTS | Assess customisation debt, vendor roadmap, licensing, support, integrations, data access, process fit and exit provisions. |
| SaaS | Assess data location, security, integration, release control, continuity, portability, vendor concentration and exit strategy. |
| Low-Code / No-Code | Establish lifecycle ownership, inventory, data and security controls, testing, reuse, integration and retirement governance. |
| Mainframe / Legacy Platform | Evaluate critical logic, reliability and throughput alongside skills, support, coupling and automation; compare encapsulation, rehosting, replatforming, incremental replacement and retirement. |
| Other | Name the pattern and assess its specific operational and lifecycle constraints. |
| Unknown / Not Assessed | Resolve evidence gaps before major sequencing, cost, resilience or platform decisions. |

For any proposed change, document the current constraint, expected outcome, dependencies, transition approach and validation evidence. Consider coexistence, data migration, cutover, rollback and retirement where relevant. These transition checks extend the source's modernisation guidance.

## Source inconsistencies and editorial decisions

| Source issue | Treatment in this reference |
| --- | --- |
| **Unknown vs Not Assessed** | Preserve both labels and explain that the source gives them overlapping meanings. Recommend separating an unperformed assessment from an inconclusive one; map historic values only after reviewing evidence. |
| **Monolith vs Layered / N-Tier** | Retain both. Monolith describes packaging and deployment; layering describes technical responsibility separation. N-tier can also describe physical distribution, so separate tiers do not by themselves imply independent business services. One application can satisfy both descriptions. |
| **Mainframe / Legacy Platform appears only in the overview** | Retain the category and add a clearly identified detailed explanation based on the overview. |
| **SOA / Service-Based and Service-Oriented Architecture** | Treat these as the same source category for this reference; local controlled vocabularies may choose a single label. |
| **Low-Code vs Low-Code / No-Code** | Use Low-Code / No-Code as the common heading while retaining the source's distinction between primary implementation and incidental use. |
| **“Three exceptional values” introduces only Other and Not Assessed in the detail** | Explain Other, Unknown and Not Assessed together as exception/evidence values, without treating them as technical architectures. |
| **Categories mix architecture and sourcing dimensions** | Preserve the practical Level 0 taxonomy; document overlap and recommend secondary fields instead of presenting the categories as a strict technical hierarchy. |

**Source:** supplied `architecture-patterns.md`. The comparison content, original examples and selection criteria are retained; structure, terminology notes, classification workflow and additional decision guidance are editorial additions. No actual application assessments are implied.
