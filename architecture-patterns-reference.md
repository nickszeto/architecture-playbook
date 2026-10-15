[architecture-patterns.md](https://github.com/user-attachments/files/33489405/architecture-patterns.md)
# Architecture Patterns

## Overview

| **Architecture pattern** | **What it means** | **Typical strengths** | **Typical concerns / modernization implications** |
|----------------------|---------------|-------------------|-----------------------------------------------|
| **Monolith**         | Most functions are built, deployed, and scaled as one application. Components may be logically separated but remain tightly packaged. | Simple deployment and operations for smaller systems; straightforward transactions. | A small change may require testing and redeploying the whole system. As complexity grows, releases, scaling and technology refresh can become difficult. Consider modularisation, selective refactoring, or replacement—not automatic decomposition. |
| **Modular Monolith** | One deployable application with deliberately separated internal modules and controlled interfaces. | Lower operational complexity than distributed services; modules can be maintained independently within one codebase. | Weak governance can allow modules to become tightly coupled. Often a viable target architecture rather than merely an interim state. |
| **Service-Oriented Architecture** | Business functions are provided through reusable services, commonly integrated through an enterprise service bus or shared middleware. | Enables reuse and integration across multiple systems; supports enterprise-wide services. | Shared middleware may become a bottleneck or concentration risk. Service ownership, interface lifecycle, orchestration complexity and vendor support need assessment. |
| **Microservices**    | The application is divided into small, independently deployable services aligned to business domains. | Independent releases and scaling; team autonomy; better isolation when properly engineered. | Introduces significant distributed-system and operational complexity. Requires mature automation, observability, security, service ownership, testing and platform engineering. It is not automatically the best target for every system. |
| **Package / COTS**   | Commercial off-the-shelf software is installed or hosted for the organisation and configured to meet its needs. | Faster access to established capabilities; vendor-supported product lifecycle; reduced custom development. | Heavy customisation can make upgrades difficult. Assess vendor roadmap, licensing, integration, data access, support, exit provisions and alignment with standard product processes. |
| **SaaS**             | A vendor operates the application as a subscription-based service, normally shared or logically isolated across customers. | Rapid provisioning, regular upgrades and reduced infrastructure responsibility. | Less control over release timing, hosting design and customisation. Assess data location, security, integration, service continuity, portability, vendor concentration and exit strategy. |
| **Low-Code / No-Code** | Applications are assembled using visual tools, reusable components and platform-managed services, with limited conventional coding. | Rapid delivery, useful for workflow and departmental solutions, and potentially lower development effort. | Can produce uncontrolled application growth, weak lifecycle ownership and platform lock-in. Governance is needed for data, security, testing, integration, reuse and retirement. |
| **Mainframe / Legacy Platform** | The application depends on an older or specialised computing platform, language, database or runtime that materially shapes its architecture. | Often highly reliable and capable of processing large transaction volumes; may contain critical business logic. | Skills scarcity, product lifecycle, high coupling, limited automation and difficult integration may create strategic risk. Modernization could involve encapsulation, rehosting, replatforming, incremental replacement or retirement. |
| **Other**            | A material pattern not adequately represented by the standard choices—for example event-driven, serverless, data-product, embedded or edge-centric architecture. | Preserves accuracy where a standard category would be misleading. | The pattern should be named and briefly explained in the Architecture Summary field. |
| **Unknown**          | Available evidence is insufficient to classify the architecture confidently. | Makes an evidence gap visible rather than forcing an unreliable answer. | Assign follow-up action before making major sequencing, cost, resilience or platform decisions. |


## 1. Layered / N-Tier

The classic enterprise application.

```none
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

## 2. Modular Monolith

This is an important distinction from a conventional monolith.

```none
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

This can be a perfectly valid modernisation target. You don't need to force every system towards microservices.

**Architecture Summary**

> Single deployable application structured into independently maintained business-domain modules.


---

## 3. Microservices

Here the application's capabilities are separated into **independently deployable services**.

```none
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

## 4. SOA / Service-Based

SOA emphasises **reusable business or enterprise services**.

```none
Application A ─┐
Application B ─┼──→ Shared Services
Application C ─┘
```

For example:

```none
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

## 5. Package / COTS

This means **Commercial Off-The-Shelf** software is the application's primary implementation model.

The organisation is principally **configuring/customising a product**, rather than designing the application's internal architecture itself.

```none
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

## 6. SaaS

SaaS is slightly different again.

```none
ICA / Consumer
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

This isn't a perfect academic distinction because SaaS is technically a delivery model rather than an application architecture. For **Level 0 portfolio classification**, however, I think it is useful because it tells you something important about the available modernisation path.


---

## 7. Low-Code

The application's primary implementation is built using a **low-code application platform**.

```none
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

**The three exceptional values**

## 8. Other

Use this when the application has been assessed, but none of the controlled categories accurately describes its architecture.

I would require a short explanation in **Architecture Summary** whenever someone selects Other.

For example:

> `Other`

> Architecture Summary: “Specialised embedded application architecture associated with operational equipment.”

This prevents **Other** becoming the easy default.


---

## 9. Not Assessed

This is important for data quality.

It means:

> **We don't know yet.**

It should *not* mean:

* none;
* legacy;
* no architecture;
* N/A.

It means there is insufficient evidence to classify the application.

This gives you a useful way of identifying systems requiring further architecture discovery.
