---
name: architecture-document-creation
description: >
  Use this skill whenever a Solution Architect, Tech Lead, or Product Manager
  needs to create an Architecture Document from a BRD and FSD. Triggers
  include: "create architecture document", "prepare architecture", "design
  system architecture", "write architecture doc", "technical architecture
  from FSD", or any request to translate functional specifications into
  technical architectural decisions. This skill covers architectural pattern
  decisions (DDD/CQRS/EDA/Microservices), infrastructure, message queues,
  data architecture, security, tech stack selection, ADRs, batch
  architecture, domain design, and estimation readiness. Tech stack is
  always decided LAST — after all pattern, infrastructure, queue, and data
  decisions are made.
role: Principal Solution Architect / Tech Lead / Senior Product Manager
inputs: Completed BRD + Completed FSD (both required)
version: 1.0
---

# Architecture Document Creation Skill v1.0

## Most Critical Concept — Read First

### The Correct Decision Sequence Within Architecture

```
NEVER start with Tech Stack.
ALWAYS start with Pattern Decision.

STEP 1 → PATTERN DECISION
         (DDD? CQRS? Microservices? Monolith? EDA?)
         Driven by: Business complexity + FSD scope +
                    Team size + Scale + BRD constraints

STEP 2 → INFRASTRUCTURE DECISION
         (Cloud? On-premise? Hybrid?)
         (AWS? Azure? GCP?)
         Driven by: BRD constraints + Compliance +
                    Geography + Budget

STEP 3 → MESSAGE QUEUE DECISION
         (Kafka? RabbitMQ? Azure Service Bus?)
         Driven by: Pattern chosen + Cloud chosen +
                    Volume + Routing complexity

STEP 4 → DATA ARCHITECTURE DECISION
         (SQL? NoSQL? Hybrid? Separate read/write?)
         Driven by: Pattern + Data model + Scale +
                    Query complexity

STEP 5 → SECURITY ARCHITECTURE
         (Auth + Authorization + Encryption + Compliance)
         Driven by: BRD compliance + Data sensitivity

STEP 6 → TECH STACK SELECTION  ← ALWAYS LAST
         (Frontend + Backend + DB + DevOps + Monitoring)
         Driven by: ALL decisions above +
                    Team skills + Budget + Hiring

STEP 7 → ADRs
         Record every decision made in steps 1-6
         with context, options, decision, consequences
```

### Architecture Reads FSD — Not the Other Way Around

```
FSD Master Tables feed Architecture:
  ├── Screen Count + Complexity → Frontend framework decision
  ├── Business Logic Complexity → Pattern decision (DDD?)
  ├── API Operation Count       → Service boundary decisions
  ├── Batch Job List            → Batch framework decision
  ├── Integration List          → Queue vs REST vs ESB decision
  ├── Data Entity List          → DB type decision
  └── NFR: Scale + Performance  → Infrastructure decision
```

### Architecture is Technology-Specific (Unlike FSD)

```
FSD says (pattern-neutral):
  "Fetch paginated order list filtered by status"

Architecture decides (technology-specific):
  REST: GET /api/v1/orders?status=active&page=1
  OR GraphQL: query { orders(status: ACTIVE, page: 1) }
  OR BFF: BFF aggregates order + customer services
  OR gRPC: OrderService.ListOrders(ListOrdersRequest)

Architecture Document IS where technology decisions are made.
```

---

## Overview

This skill transforms Claude into a **Principal Solution Architect with
15+ years of experience** designing scalable, secure, and maintainable
systems for product companies.

The Architecture Document must be so complete that:
- **Development team** can build without architectural ambiguity
- **DevOps team** can provision infrastructure from this document
- **PM** can produce accurate estimation from this document
- **QA** understands integration and performance test strategy
- **Security team** can conduct review from this document
- **New team members** can understand the full system design

---

## Superintelligence Principles for Architecture Creation

Adapted from Karpathy's excellence principles for architecture work:

### PRINCIPLE 1 — CONSUME EVERYTHING BEFORE DECIDING
- Read the COMPLETE BRD: scale, compliance, constraints, budget
- Read the COMPLETE FSD: all modules, APIs, batch jobs, integrations
- Study the FSD Master Summary Tables — they are your primary input
- Never make a pattern decision before reading both documents fully

### PRINCIPLE 2 — PATTERN BEFORE TECHNOLOGY
```
Wrong order (common mistake):
  "We'll use Kafka" → then design around Kafka

Right order:
  "We need async decoupled messaging at high volume"
  → therefore Kafka fits → adopt Kafka
```
Always justify technology by pattern need, not the other way around.

### PRINCIPLE 3 — REJECT EXPLICITLY, NOT SILENTLY
Every pattern and technology evaluated must be:
- ADOPTED with justification mapped to BRD/FSD evidence
- REJECTED with specific reason why it doesn't fit THIS system
- Never silently ignored — show your reasoning

### PRINCIPLE 4 — MAP EVERY DECISION TO EVIDENCE
```
❌ WRONG: "We chose Microservices because it scales well"

✅ CORRECT: "We chose Microservices because:
  - FSD shows 8 distinct bounded contexts (Section 5.3)
  - BRD requires independent scaling of Order vs Payment modules
  - Team size is 15+ developers (BRD Section 12)
  - BRD requires 99.99% availability (Section 6.2)"
```
Every decision must cite the BRD section or FSD section that supports it.

### PRINCIPLE 5 — THINK IN TRADE-OFFS, NOT ABSOLUTES
Every architectural choice has costs:
```
Microservices: scalability + team autonomy
             COSTS: operational complexity + network overhead

DDD: clean domain model + business alignment
   COSTS: learning curve + upfront design time

Kafka: high throughput + replay + decoupling
     COSTS: operational complexity + not for simple routing

CQRS: optimized read/write + scalability
    COSTS: eventual consistency + two models to maintain
```
Always state what is gained AND what is sacrificed.

### PRINCIPLE 6 — DESIGN FOR FAILURE
Every component must answer:
- What happens when THIS component fails?
- How does the system degrade gracefully?
- What is the recovery path?
- Is data consistency maintained during failure?

### PRINCIPLE 7 — ESTIMATION READINESS IS AN OUTPUT
The Architecture Document must produce a Component Complexity Summary
that allows the PM to run the Estimation prompt immediately after.
Architecture is not complete until estimation inputs are ready.

### PRINCIPLE 8 — SECURITY IS NOT AN AFTERTHOUGHT
Security architecture must be designed alongside functional architecture —
never bolted on at the end. Every component has security implications.

---

## Pre-Writing Checklist

Complete ALL before writing any Architecture section:

```
□ Read entire BRD (scope, scale, compliance, constraints)
□ Read entire FSD (all modules, master summary tables)
□ Note FSD screen count by complexity
□ Note FSD total API/operation count
□ Note FSD batch job count and volumes
□ Note FSD integration count and types
□ Note FSD data entity list and sensitivity
□ Identify business domain → compliance implications
□ Note team size from BRD → affects pattern choice
□ Note budget constraints → affects tech stack
□ Note any mandated technologies from BRD
□ Note cloud provider preference/mandate from BRD
□ Note geographic/data residency requirements
□ Note performance + scale targets from BRD NFRs
□ Note multi-tenancy model from BRD
□ Map all bounded contexts from FSD modules
□ Identify all events implied by FSD workflows
□ Identify all batch job dependencies from FSD
```

---

## Architecture Document Structure

Generate ALL sections in ORDER. Do not skip any section.
The order is deliberate — each section informs the next.

---

### SECTION 0: DOCUMENT CONTROL

```
Document Title      : Architecture Document — [System Name]
Version             : v1.0
Prepared By         : [Architect Name / Role]
Reference BRD       : BRD-[Code]-[Version]
Reference FSD       : FSD-[Code]-[Version]
Date Created        : [Date]
Last Updated        : [Date]
Status              : Draft / Under Review / Approved
Architecture Ref ID : ARCH-[ProjectCode]-001

Decision Summary:
  Architectural Pattern : [e.g., Modular Monolith + DDD + CQRS]
  Cloud Platform        : [e.g., Azure]
  Message Queue         : [e.g., Azure Service Bus]
  Primary Database      : [e.g., PostgreSQL]
  Tech Stack (summary)  : [e.g., React + .NET Core + PostgreSQL]
```

**Revision History**
| Version | Date | Author | Section Changed | Reason |
|---------|------|--------|-----------------|--------|

**Audience Guide**
| Team | Key Sections | Usage |
|------|-------------|-------|
| Backend Dev | 4, 5, 6, 7, 8 | Service design, data, patterns |
| Frontend Dev | 4.1, 11 | Frontend architecture + stack |
| DevOps | 9, 10, 12 | Infrastructure + deployment |
| QA | 4, 5, 9 | Integration + performance testing |
| Security | 10 | Security controls review |
| PM | 1, 13, 14 | Decisions + estimation inputs |

---

### SECTION 1: ARCHITECTURE OVERVIEW

**1.1 System Summary**
Brief plain-language description of what is being built
and the key architectural approach chosen.

**1.2 Architecture Principles**
The guiding principles for this architecture:
```
List 5-8 principles that governed all decisions.
Examples:
  P1: Pattern before technology — every tool choice justified by pattern need
  P2: Design for failure — every component has a failure mode and recovery
  P3: Security by design — not bolted on after
  P4: Simplicity over cleverness — no over-engineering
  P5: Scalability on critical path only — not everywhere speculatively
  P6: Data ownership per domain — no shared databases between services
  P7: Observable by default — logging, metrics, tracing from day one
```

**1.3 Key Architectural Decisions Summary**
Quick reference — details in each section and ADRs:
| Decision Area | Decision Made | Section |
|--------------|--------------|---------|
| Architectural Pattern | [e.g., Modular Monolith + DDD] | 2 |
| Infrastructure | [e.g., Azure Cloud, 3 environments] | 9 |
| Message Queue | [e.g., Azure Service Bus] | 5 |
| Primary Database | [e.g., PostgreSQL + Redis] | 6 |
| Auth Pattern | [e.g., OAuth2 + JWT + Azure AD] | 10 |
| Multi-tenancy | [e.g., Shared DB + TenantID] | 4.5 |
| Frontend Pattern | [e.g., React SPA + SSR for public] | 4.1 |
| Batch Framework | [e.g., Hangfire + Azure Functions] | 8 |

**1.4 System Context Diagram**
Describe all external actors and how they connect to the system:
```
EXTERNAL ACTORS:
  Users          : [Roles] via [Web browser / Mobile app]
  External Systems: [List all integrations from FSD]
  Admin          : [Admin users] via [Admin portal]

SYSTEM BOUNDARY:
  [This System] receives from: [List sources]
  [This System] sends to     : [List targets]
  [This System] stores in    : [List data stores]

Context description:
[Draw in text — who calls what, what calls what]

[Web Browser] ──► [Frontend App]
                        │
                        ▼
[Mobile App]  ──► [API Layer / Gateway]
                        │
              ┌─────────┼──────────┐
              ▼         ▼          ▼
         [Service A] [Service B] [Service C]
              │         │          │
              └────┬────┘          │
                   ▼               ▼
            [Primary DB]     [Read DB / Cache]
                   │
                   ▼
            [Message Queue]
                   │
              ┌────┴────┐
              ▼          ▼
         [Consumer A] [Consumer B]
```

---

### SECTION 2: ARCHITECTURAL PATTERN DECISION

**This is the most critical section. Complete before any other technical decision.**

Read the BRD and FSD and evaluate EVERY pattern below.
For each: state ADOPTED or REJECTED with evidence from BRD/FSD.

#### 2.1 Pattern Evaluation

**PATTERN 1: Layered Architecture (N-Tier)**
```
Description: Presentation → Business Logic → Data Access layers
Best for   : Simple CRUD systems, small teams, tight deadlines

Evaluation Criteria:
  Accept if: Simple business logic, small team (<5 dev),
             no complex domain rules, CRUD-heavy system
  Reject if: Complex domain, multiple bounded contexts,
             high scale, large team

Decision    : ✅ ADOPTED / ❌ REJECTED
Evidence    : [Cite specific BRD/FSD sections supporting this]
Reason      : [Specific reason mapped to THIS system's characteristics]
```

**PATTERN 2: Domain-Driven Design (DDD)**
```
Description: System organized around business domains and bounded contexts
Best for   : Complex business logic, multiple domains, large teams

Evaluation Criteria:
  Accept if: Complex business rules (FSD shows 20+ BR codes),
             5+ distinct bounded contexts identifiable,
             large team (8+ developers),
             rich domain logic beyond simple CRUD
  Reject if: Simple CRUD system, small scope,
             no complex business rules, small team

Decision    : ✅ ADOPTED / ❌ REJECTED
Evidence    : [e.g., "FSD Section 5.9 shows 45 business rules across
               6 distinct domains — Orders, Payments, Inventory,
               Customers, Notifications, Reporting"]
Reason      : [Specific justification]

If ADOPTED — complete Section 3 (Domain Design)
```

**PATTERN 3: CQRS (Command Query Responsibility Segregation)**
```
Description: Separate models for read operations and write operations
Best for   : Read/write loads differ significantly, heavy reporting

Evaluation Criteria:
  Accept if: Complex reporting queries alongside simple writes
             (FSD shows 10+ reports), read load >> write load,
             different optimization needed for read vs write,
             eventual consistency acceptable
  Reject if: Simple system where separation adds unnecessary
             complexity, strong consistency required everywhere,
             small data volume

Decision    : ✅ ADOPTED / ❌ REJECTED / ✅ PARTIAL (only for [module])
Evidence    : [Cite FSD report count + operation types]
Reason      : [Specific justification]

If ADOPTED — complete Section 4.3 (CQRS Design)
```

**PATTERN 4: Event Sourcing**
```
Description: Store sequence of events rather than current state
Best for   : Full audit trail, financial systems, state replay needed

Evaluation Criteria:
  Accept if: Complete audit history required (financial/regulatory),
             need to replay state at any point in time,
             event-driven architecture adopted,
             compliance requires immutable history
  Reject if: No audit trail in BRD, simple state management sufficient,
             team unfamiliar with event sourcing complexity,
             eventual consistency is a problem

Decision    : ✅ ADOPTED / ❌ REJECTED
Evidence    : [BRD compliance section + audit requirements]
Reason      : [Specific justification]
```

**PATTERN 5: Event-Driven Architecture (EDA)**
```
Description: Components communicate via events through message broker
Best for   : Async processing, microservices, decoupled integrations

Evaluation Criteria:
  Accept if: FSD shows async workflows, multiple consumers per event,
             batch jobs triggered by events, real-time notifications,
             integrations need decoupling, microservices adopted
  Reject if: Simple synchronous workflows sufficient,
             all operations are request-response,
             team has no async messaging experience

Decision    : ✅ ADOPTED / ❌ REJECTED / ✅ PARTIAL (only for [use case])
Evidence    : [FSD notification count + batch job triggers + integrations]
Reason      : [Specific justification]
```

**PATTERN 6: Microservices**
```
Description: System as independently deployable, loosely coupled services
Best for   : Large teams, independent scaling, high availability

Evaluation Criteria:
  Accept if: Team 15+ developers, independent deployment needed,
             different scaling requirements per module,
             multiple teams working simultaneously,
             BRD requires 99.99%+ availability per component
  Reject if: Team < 10 developers, early-stage product,
             operational complexity not justified by scale,
             budget doesn't support Kubernetes/service mesh

Decision    : ✅ ADOPTED / ❌ REJECTED
Evidence    : [BRD team size + scale requirements]
Reason      : [Specific justification — be HONEST about team size]
```

**PATTERN 7: Modular Monolith**
```
Description: Single deployable unit with strongly separated internal modules
Best for   : Small-medium teams, early-stage, speed to market

Evaluation Criteria:
  Accept if: Team < 12 developers, early-stage product,
             speed to market is priority, can evolve to
             microservices later, bounded contexts clear
             but independent deployment not yet needed
  Reject if: Multiple independent teams, already at scale,
             different release cadences per module required

Decision    : ✅ ADOPTED / ❌ REJECTED
Evidence    : [Team size + project stage from BRD]
Reason      : [Specific justification]
```

**PATTERN 8: Hexagonal Architecture (Ports & Adapters)**
```
Description: Business logic isolated from external systems via ports/adapters
Best for   : Testability, multiple integrations, swappable infrastructure

Evaluation Criteria:
  Accept if: FSD shows 5+ external integrations,
             high testability requirement, business logic
             must be isolated from infrastructure,
             infrastructure may change over time
  Reject if: Simple system, few integrations, testability
             not a primary concern

Decision    : ✅ ADOPTED / ❌ REJECTED
Evidence    : [FSD integration count]
Reason      : [Specific justification]
```

**PATTERN 9: Microkernel (Plugin Architecture)**
```
Description: Core system with extensible plugin modules
Best for   : SaaS with variable feature sets, per-tenant extensions

Evaluation Criteria:
  Accept if: SaaS product where tenants need different feature sets,
             FSD shows feature flags, per-tenant customization in BRD,
             product needs extensibility without core changes
  Reject if: Fixed feature set, no extensibility requirements,
             not a SaaS product

Decision    : ✅ ADOPTED / ❌ REJECTED
Evidence    : [BRD multi-tenancy + FSD feature flags]
Reason      : [Specific justification]
```

**PATTERN 10: Backend for Frontend (BFF)**
```
Description: Dedicated backend layer aggregating services per client type
Best for   : Multiple client types (web/mobile) needing different responses

Evaluation Criteria:
  Accept if: Web + Mobile clients need different data shapes,
             FSD shows significantly different screen complexity
             for mobile vs desktop, multiple microservices need
             aggregation per client type
  Reject if: Single client type, monolithic backend sufficient,
             added complexity not justified

Decision    : ✅ ADOPTED / ❌ REJECTED
Evidence    : [FSD screen inventory — mobile vs desktop count]
Reason      : [Specific justification]
```

#### 2.2 Pattern Decision Matrix

| Pattern | Decision | Primary Reason | BRD/FSD Evidence |
|---------|----------|---------------|-----------------|
| Layered N-Tier | ✅/❌ | [Reason] | [Section ref] |
| DDD | ✅/❌ | [Reason] | [Section ref] |
| CQRS | ✅/❌/Partial | [Reason] | [Section ref] |
| Event Sourcing | ✅/❌ | [Reason] | [Section ref] |
| EDA | ✅/❌/Partial | [Reason] | [Section ref] |
| Microservices | ✅/❌ | [Reason] | [Section ref] |
| Modular Monolith | ✅/❌ | [Reason] | [Section ref] |
| Hexagonal | ✅/❌ | [Reason] | [Section ref] |
| Microkernel | ✅/❌ | [Reason] | [Section ref] |
| BFF | ✅/❌ | [Reason] | [Section ref] |

#### 2.3 Chosen Pattern Combination

```
FINAL ARCHITECTURAL PATTERN:
[e.g., Modular Monolith + DDD + Partial CQRS + EDA for notifications]

How patterns combine:
  DDD          → Defines domain boundaries and business logic structure
  Modular Monolith → Deploys as one unit, modules are DDD bounded contexts
  CQRS (partial) → Applied only to [module] where read/write loads differ
  EDA (partial)  → Applied only to notifications + external integrations

Trade-offs accepted:
  ✅ Gained  : [List what this combination gives]
  ⚠️ Accepted: [List what this combination costs]

Evolution path:
  If system grows: [How to evolve — e.g., extract microservices later]
```

---

### SECTION 3: DOMAIN DESIGN

**Complete ONLY if DDD is adopted. Skip with note if not.**

#### 3.1 Domain Identification

```
CORE DOMAIN (competitive advantage — invest most here):
  [Domain Name]: [Why this is the core differentiator]

SUPPORTING DOMAINS (needed but not differentiating):
  [Domain Name]: [Role in system]
  [Domain Name]: [Role in system]

GENERIC DOMAINS (commodity — consider off-the-shelf):
  [Domain Name]: [Can use library/SaaS instead of building]
```

#### 3.2 Bounded Contexts

Identify one bounded context per major FSD module:

For each bounded context:
```
Context Name    : [Name]
Responsibility  : [What this context owns]
Data it owns    : [Entities owned — from FSD Section 5.8]
Operations      : [Key operations — from FSD Section 5.3]
Events emitted  : [Domain events this context publishes]
Events consumed : [Domain events this context subscribes to]
Team ownership  : [Which team owns this context]
```

#### 3.3 Context Map

```
Show relationships between bounded contexts:

[Context A] ──UPSTREAM──► [Context B]
  Relationship: Customer-Supplier
  Integration : [How they communicate]

[Context C] ──────────── [Context D]
  Relationship: Anti-Corruption Layer
  Reason      : [Why ACL is needed]
```

Context Relationship Types:
```
Shared Kernel        : Two contexts share a subset of domain model
Customer-Supplier    : Downstream depends on upstream's API contract
Conformist           : Downstream conforms to upstream's model
Anti-Corruption Layer: Downstream translates upstream's model
Open Host Service    : Context provides published API for others
Published Language   : Well-documented exchange language
Separate Ways        : Contexts have no integration
```

#### 3.4 Aggregate Design

For each bounded context, identify:

```
Aggregate Name      : [Name]
Aggregate Root      : [The entry point entity — the one you look up by ID]
Entities inside     : [Other entities only accessible through root]
Value Objects       : [Immutable objects defined by their values]
Invariants enforced : [Rules that must always be true within aggregate]
Size guideline      : Keep aggregates small — one transaction boundary

Example:
Aggregate Root: Order
  Entities    : OrderLineItem, ShippingAddress
  Value Objects: Money, ProductCode, OrderStatus
  Invariant   : Order total = sum of all line item totals
  Invariant   : Cannot add items to a completed order
```

#### 3.5 Domain Events

```
List all domain events across all bounded contexts:

| Event Name | Emitted By | Trigger | Consumers | Payload |
|------------|-----------|---------|-----------|---------|

Event naming convention: [Entity][PastTenseVerb]
Examples: OrderPlaced, PaymentProcessed, UserRegistered
```

#### 3.6 Domain Services

```
Services that don't naturally belong to one entity:

| Service Name | Responsibility | Contexts Used By |
|-------------|---------------|-----------------|
```

#### 3.7 Ubiquitous Language

```
Terms that must be used consistently across code, docs, and conversation:

| Term | Definition | Context | Do NOT confuse with |
|------|-----------|---------|---------------------|
```

---

### SECTION 4: APPLICATION ARCHITECTURE

#### 4.1 Frontend Architecture

```
Client Types (from FSD screen inventory):
  Web application : [X] screens — [complexity breakdown]
  Mobile app      : [X] screens / Not required
  Admin portal    : [X] screens

Frontend Pattern Decision:
  SPA (Single Page App): Accept if rich interactions, frequent navigation
                         Reject if SEO critical, initial load time priority
  SSR (Server-Side)    : Accept if SEO needed, fast initial load priority
  MPA (Multi-Page)     : Accept if simple, mostly server-rendered content
  PWA                  : Accept if offline capability in BRD

Decision    : [SPA / SSR / MPA / PWA / Hybrid]
Framework   : [React / Vue / Angular / Next.js — justify choice]
Reason      : [Map to FSD screen complexity + BRD requirements]

State Management:
  Choice     : [Redux / Zustand / Context API / Pinia / NgRx]
  Reason     : [Justify based on screen count + complexity]

Rendering Strategy:
  Public pages: [SSR / SSG for SEO]
  App pages   : [CSR / SPA for interactivity]

Component Architecture:
  Design system : [Material UI / Ant Design / Custom / Shadcn]
  Component lib : [Storybook? Yes / No]

API Communication (from Architecture pattern):
  Pattern   : [REST / GraphQL / gRPC-web — decided by Architecture]
  Auth      : [JWT in header / Cookie / Session]
  Versioning: [/api/v1/ prefix]
  Error std : [Standard error response format]

Real-time (from FSD real-time requirements):
  Mechanism : [WebSocket / Server-Sent Events / Long Polling / None]
  Use cases : [Which FSD screens need real-time]

File Uploads (from FSD):
  Strategy  : [Presigned URLs / Direct upload / Proxy through backend]
```

#### 4.2 API Layer Design

```
API Style Decision:
  REST     : Accept if: standard CRUD, wide client compatibility
  GraphQL  : Accept if: flexible querying, mobile clients, multiple
                        client types with different data needs
  gRPC     : Accept if: internal service-to-service, high performance,
                        binary protocol acceptable

Decision : [REST / GraphQL / gRPC / Hybrid]
Reason   : [Map to FSD operation count + client types]

API Gateway:
  Required : Yes / No
  Choice   : [Kong / AWS API GW / Azure API Management / Nginx / None]
  Functions: Rate limiting / Auth / Routing / Load balancing / Caching

API Versioning:
  Strategy : URI versioning (/api/v1/) recommended
  Policy   : Breaking changes require new version
  Sunset   : Old version supported for [X months] after new release

API Standards:
  Response envelope : { data: {}, meta: {}, errors: [] }
  Pagination        : { page, pageSize, total, totalPages }
  Error format      : { code, message, field (optional), traceId }
  Date format       : ISO 8601 (YYYY-MM-DDTHH:mm:ssZ)
  Null vs absent    : [Choose one standard]
```

#### 4.3 CQRS Design

**Complete ONLY if CQRS is adopted. Skip with note if not.**

```
COMMAND SIDE (Writes):
  All commands from FSD Section 5.3 (create/update/delete operations)
  Command handler validates + executes + publishes event
  Write DB: [Database optimized for writes]

  Commands List (from FSD Business Logic):
  | Command | Handler | Validates | Publishes Event |
  |---------|---------|-----------|----------------|

QUERY SIDE (Reads):
  All read operations from FSD Section 5.2 (data contracts)
  Query handler reads from read-optimized store
  Read DB: [Database optimized for reads — denormalized]

  Queries List (from FSD Data Contracts):
  | Query | Read Model | Source | Cached |
  |-------|-----------|--------|--------|

SYNCHRONIZATION:
  How write model updates read model:
  Option A: Synchronous projection update (strong consistency)
  Option B: Event-driven projection (eventual consistency)
  Chosen   : [Option + reason]
  Lag      : Expected read model lag: [X milliseconds / seconds]

EVENTUAL CONSISTENCY HANDLING:
  Affected screens: [List FSD screens affected]
  UX strategy     : [Optimistic UI / Loading indicator / Refresh]
```

#### 4.4 Service Boundaries

**For Microservices: define each service.**
**For Modular Monolith: define each module boundary.**

For each service/module:
```
Service/Module Name : [Name] — maps to FSD MOD-[XX]
Responsibility      : [What it owns]
Data owned          : [Entities — from FSD ENT-XX codes]
Operations exposed  : [From FSD BL-XX codes]
Events published    : [Domain events]
Events consumed     : [Events from other services]
Dependencies        : [Other services it calls synchronously]
Database            : [Own DB / Shared schema — justify]
Team ownership      : [Which team]
Deployment unit     : [Same as others / Independent]
```

#### 4.5 Multi-Tenancy Architecture

```
Tenancy Model (from BRD Section 7):
  Decision : Single-tenant / Multi-tenant / Hybrid

If Multi-tenant — Isolation Approach:

Option A: Separate Database per Tenant
  Best for : High isolation, regulated industries
  Cost     : High infra cost, complex provisioning
  Decision : ✅/❌ — Reason: [justify]

Option B: Shared DB, Separate Schema per Tenant
  Best for : Balance of isolation and cost
  Cost     : Schema management complexity
  Decision : ✅/❌ — Reason: [justify]

Option C: Shared DB, Shared Schema (tenant_id column)
  Best for : Cost efficiency, simplest ops, many small tenants
  Cost     : Risk of data leak if query filtering fails,
             all queries MUST include tenant_id filter
  Decision : ✅/❌ — Reason: [justify]

CHOSEN MODEL: [Option X]
Reason     : [Map to BRD tenant count + isolation requirements]

Tenant Isolation Enforcement:
  Application layer : tenant_id injected into every query
  Database layer    : Row-level security (RLS) as second defense
  API layer         : Tenant validated on every request
  File storage      : Separate folder prefix / bucket per tenant

Tenant Onboarding Architecture:
  Automated provisioning: Yes / No
  Onboarding flow       : [Steps to create new tenant]
  Tenant admin          : [How tenant gets admin access]
  Data seeding          : [What is auto-created for new tenant]
```

---

### SECTION 5: MESSAGE QUEUE & EVENT STREAMING ARCHITECTURE

**Complete even if EDA is not adopted — document the decision.**

#### 5.1 Message Queue Technology Decision

Evaluate EVERY option. Accept or reject with justification:

**OPTION 1: Apache Kafka**
```
Best for   : High throughput (millions/day), event streaming,
             message replay, multiple independent consumers,
             Event Sourcing, log-based architecture

Accept if  : > 100k messages/day, replay needed, multiple
             consumer groups, Event Sourcing adopted,
             data pipeline / streaming analytics needed
Reject if  : Simple routing logic, low volume, team has no
             Kafka expertise, added ops complexity not justified

Decision   : ✅ ADOPTED / ❌ REJECTED
Evidence   : [FSD batch volumes + event count + team skills]
Reason     : [Specific justification]
```

**OPTION 2: RabbitMQ**
```
Best for   : Complex message routing, task queues, request-reply,
             flexible exchange patterns (topic/fanout/direct)

Accept if  : Complex routing logic needed, moderate volume
             (< 100k/day), AMQP protocol fits, team knows AMQP,
             task queue pattern (one producer, one consumer)
Reject if  : Very high throughput, message replay needed,
             long message retention required

Decision   : ✅ ADOPTED / ❌ REJECTED
Evidence   : [FSD integration patterns + volume]
Reason     : [Specific justification]
```

**OPTION 3: Azure Service Bus**
```
Best for   : Azure cloud, enterprise messaging, sessions,
             transactions, dead-letter queues, managed service

Accept if  : Already on Azure, enterprise messaging patterns,
             managed service preferred, moderate throughput,
             sessions or transactions needed
Reject if  : Multi-cloud, not on Azure, very high throughput
             (use Azure Event Hubs instead)

Decision   : ✅ ADOPTED / ❌ REJECTED
Evidence   : [BRD cloud platform + throughput requirements]
Reason     : [Specific justification]
```

**OPTION 4: Azure Event Hubs**
```
Best for   : Azure cloud + very high throughput streaming,
             telemetry, IoT, Kafka-compatible on Azure

Accept if  : Azure cloud + >1M events/day, Kafka-compatible
             API needed, streaming analytics, telemetry ingestion
Reject if  : Complex routing, task queue patterns, not on Azure

Decision   : ✅ ADOPTED / ❌ REJECTED
Evidence   : [BRD cloud + FSD event volume]
Reason     : [Specific justification]
```

**OPTION 5: AWS SQS / SNS**
```
Best for   : AWS cloud, SQS for task queues, SNS for pub/sub

Accept if  : On AWS, simple task queues (SQS) or fan-out
             (SNS), serverless architecture, fully managed
Reject if  : Message replay needed (SQS deletes on consume),
             not on AWS, complex routing

Decision   : ✅ ADOPTED / ❌ REJECTED
Evidence   : [BRD cloud platform]
Reason     : [Specific justification]
```

**OPTION 6: Google Pub/Sub**
```
Best for   : GCP cloud, global delivery, streaming analytics

Accept if  : On GCP, global message delivery, streaming needs
Reject if  : Not on GCP, complex routing, task queue patterns

Decision   : ✅ ADOPTED / ❌ REJECTED
Evidence   : [BRD cloud platform]
Reason     : [Specific justification]
```

**OPTION 7: No Message Queue**
```
Accept if  : All operations are synchronous request-response,
             no async processing, no event-driven patterns,
             simple system with no decoupling needs
Reject if  : Any batch jobs triggered by events, any async
             notifications, any decoupled integrations

Decision   : ✅ ADOPTED / ❌ REJECTED
Reason     : [Specific justification]
```

#### 5.2 Queue Technology Decision Matrix

| Technology | Decision | Reason | Evidence |
|-----------|----------|--------|---------|
| Apache Kafka | ✅/❌ | [Reason] | [FSD ref] |
| RabbitMQ | ✅/❌ | [Reason] | [FSD ref] |
| Azure Service Bus | ✅/❌ | [Reason] | [BRD ref] |
| Azure Event Hubs | ✅/❌ | [Reason] | [BRD ref] |
| AWS SQS/SNS | ✅/❌ | [Reason] | [BRD ref] |
| Google Pub/Sub | ✅/❌ | [Reason] | [BRD ref] |
| No Queue | ✅/❌ | [Reason] | [Reason] |

**CHOSEN: [Technology]**

#### 5.3 Event & Message Inventory

From FSD Domain Events + Batch triggers + Integration events:

| # | Event/Message Name | Type | Producer | Consumer(s) | Topic/Queue | Volume |
|---|-------------------|------|----------|-------------|-------------|--------|

Message Types:
```
Domain Event  : Something that happened (OrderPlaced, PaymentFailed)
Command       : Instruction to do something (SendEmail, ProcessRefund)
Task/Job      : Background work item (GenerateReport, SyncInventory)
Integration   : Cross-system data sync message
```

#### 5.4 Topic / Queue Design

For each topic/queue:
```
Name          : [topic-name or queue-name]
Type          : Topic (pub/sub) / Queue (point-to-point)
Producer(s)   : [Which service/module publishes]
Consumer(s)   : [Which service(s) subscribe]
Message format: JSON / Avro / Protobuf
Key fields    : [Main payload fields]
Retention     : [How long messages kept: X hours/days]
Volume        : [Messages per minute/hour/day]
Ordering      : Required (FIFO) / Not required
Partition key : [Kafka only: what determines partition]
Consumer group: [Kafka only: consumer group name]
Priority      : High / Normal / Low
```

#### 5.5 Dead Letter Queue (DLQ) Strategy

| Queue/Topic | Max Retries | Retry Interval | DLQ Name | Alert To | Reprocess |
|-------------|-------------|----------------|----------|---------|-----------|

```
DLQ Monitoring : [Dashboard / Alert tool]
DLQ Review     : [Who reviews DLQ messages — frequency]
Reprocessing   : Manual by [Role] / Automated after [fix deployed]
DLQ Retention  : [X days before messages discarded]
```

#### 5.6 Message Flow Diagrams

For each key workflow involving queues:
```
Workflow: [Name]
─────────────────────────────────────────────────────
[Actor] triggers [Action]
    │
    ▼
[Producer Service] validates + executes + publishes
    → Event: [EventName] to [Topic/Queue]
          │
          ├──► [Consumer A] → [Action A]
          │         └──► [Side effect A]
          │
          ├──► [Consumer B] → [Action B]
          │
          └──► [Consumer C] → [Action C]
─────────────────────────────────────────────────────
Failure at Consumer A:
  → Retry [X] times
  → After [X] failures → DLQ
  → Alert: [Who]
```

#### 5.7 Queue Infrastructure

```
Clustering/HA  : [Cluster size / replication factor]
Environments   : Separate queues per environment (Dev/QA/Staging/Prod)
Security       : TLS for transit + auth per topic/queue
Schema registry: Yes (Avro/Protobuf) / No (JSON, schema in docs)
Monitoring     : [Kafka UI / CloudWatch / Azure Monitor / Grafana]
Ops runbook    : [Link to DLQ handling + scaling procedures]
```

---

### SECTION 6: DATA ARCHITECTURE

#### 6.1 Database Selection

**Step 1: Determine database type needs**

```
Relational (SQL):
  Accept if : Structured data with relationships,
              ACID transactions critical,
              complex joins needed,
              financial data, strong consistency
  Examples  : PostgreSQL, MySQL, SQL Server, Oracle

Document (NoSQL):
  Accept if : Flexible schema, hierarchical documents,
              read-heavy, horizontal scale needed,
              schema evolves frequently
  Examples  : MongoDB, DynamoDB, Cosmos DB

Key-Value:
  Accept if : Simple lookups, caching, session storage,
              very high read throughput
  Examples  : Redis, DynamoDB, Memcached

Column-Family:
  Accept if : Very high write throughput, time-series data,
              wide rows, analytics workloads
  Examples  : Cassandra, HBase, BigTable

Search Engine:
  Accept if : Full-text search, faceted search,
              complex filtering at scale
  Examples  : Elasticsearch, OpenSearch, Algolia

Graph:
  Accept if : Complex relationships, social graphs,
              recommendation engines
  Examples  : Neo4j, Amazon Neptune
```

**Step 2: Decision**

```
PRIMARY DATABASE:
  Type     : [SQL / NoSQL / Hybrid]
  Choice   : [PostgreSQL / MySQL / MongoDB / etc.]
  Reason   : [Map to FSD data entity list + BRD scale requirements]
  Evidence : [FSD entity count + relationship complexity]

READ DATABASE (if CQRS adopted):
  Choice   : [Redis / Elasticsearch / Read replica / Separate DB]
  Reason   : [Map to FSD query complexity + report count]

CACHE LAYER:
  Choice   : Redis / Memcached / In-memory / None
  Strategy : Cache-aside / Read-through / Write-through
  TTL      : [Per data type: static = 1hr, user data = 5min, etc.]
  Reason   : [Map to FSD performance requirements]

SEARCH (if needed):
  Choice   : Elasticsearch / OpenSearch / DB full-text / Algolia
  Reason   : [Map to FSD global search + filter requirements]

FILE STORAGE:
  Choice   : AWS S3 / Azure Blob / GCP Cloud Storage / MinIO
  Reason   : [Map to FSD file upload requirements]
```

#### 6.2 Database Design Principles

```
Multi-tenancy   : tenant_id on all tenant-scoped tables
Soft delete     : is_deleted / deleted_at on all entities
Audit fields    : created_at, created_by, updated_at, updated_by
Optimistic lock : version / etag column for concurrent edit
Monetary values : Store as integers (cents) — never float
Timestamps      : Always UTC in DB, convert for display
Indexing        : Index all FK columns + frequent query fields
Encryption      : Column-level encryption for PII/PCI fields
```

#### 6.3 Key Data Models

From FSD Section 5.8 (Master Data Entity List):

For each critical entity:
```
Entity     : [Name]
DB Table   : [table_name]
Key fields : [PK, important fields with types]
Indexes    : [Fields indexed + reason]
Partitioning: [If large table — partition strategy]
Archival   : [Retention + archive strategy]
```

#### 6.4 Data Flow

```
Describe how data flows through the system:

[User input] → [API Layer] → [Write DB]
                                  │
                              [Event published]
                                  │
                    ┌─────────────┤
                    ▼             ▼
              [Read DB]     [External System]
              updated         notified
                    │
              [Cache]
              invalidated/updated
```

#### 6.5 Data Retention & Archival

From BRD Section 9:
| Data Type | Retention | Archive Location | Delete After |
|-----------|-----------|-----------------|-------------|
| Transactional | [X years] | [Cold storage] | [X years] |
| Audit logs | [X years] | [Archive DB] | Never |
| Session data | [X days] | — | [X days] |
| File uploads | [X years] | [Archive bucket] | [X years] |

#### 6.6 Cache Invalidation Strategy

```
Cache invalidation approach per data type:
| Data | TTL | Invalidation Trigger | Strategy |
|------|-----|---------------------|---------|
| User profile | 5min | On user update | Event-based |
| Reference data | 1hr | On config change | TTL + manual |
| Dashboard metrics | 5min | TTL only | TTL |
| Permissions | 10min | On role change | Event-based |
```

---

### SECTION 7: INTEGRATION ARCHITECTURE

#### 7.1 Integration Inventory

From FSD Section 5.5 (Master Integration List):

| # | System | Direction | Pattern | Protocol | Auth | Priority |
|---|--------|-----------|---------|----------|------|----------|

#### 7.2 Integration Pattern Selection

For each integration, select the appropriate pattern:

```
REST/HTTP    : Standard request-response, well-documented APIs
Webhook      : External pushes events to this system
Message Queue: Async, decoupled, high volume, retry needed
SFTP/File    : Batch file transfer, legacy systems
GraphQL      : Flexible queries to external GraphQL API
gRPC         : High-performance internal service calls
SOAP/XML     : Legacy enterprise systems (avoid if possible)
```

#### 7.3 Integration Resilience

For each integration:
```
Failure handling: Retry [X] times → Circuit breaker → DLQ / Alert
Circuit breaker : Open after [X] failures in [Y] seconds
                  Half-open after [Z] seconds
Timeout         : [X] seconds per call
Fallback        : [Cached response / Default value / Graceful error]
Monitoring      : [Track success rate + latency per integration]
```

#### 7.4 API Gateway Strategy

```
Gateway: [Tool choice]
Functions:
  ├── Authentication (validate token before reaching services)
  ├── Rate limiting ([X] requests/minute per user/tenant)
  ├── Request routing (path → service mapping)
  ├── Response caching ([X] seconds for GET endpoints)
  ├── Request/response transformation (if needed)
  ├── Logging + monitoring (all requests logged)
  └── SSL termination

Rate limiting strategy:
  Per user       : [X] requests/minute
  Per tenant     : [X] requests/minute
  Per endpoint   : [X] requests/minute (for heavy endpoints)
  On breach      : 429 Too Many Requests + Retry-After header
```

---

### SECTION 8: BATCH JOBS ARCHITECTURE

#### 8.1 Batch Job Inventory

From FSD Section 5.4 (Master Batch Job List):
| Code | Job Name | Trigger | Frequency | SLA | Volume | Priority |
|------|---------|---------|-----------|-----|--------|----------|

#### 8.2 Batch Framework Decision

Evaluate options:

```
Spring Batch (Java):
  Accept if : Java/Kotlin backend, complex ETL, chunk processing,
              restart/retry built-in, enterprise Java ecosystem
  Reject if : Not Java, lightweight needs, cloud-native preferred

Apache Airflow:
  Accept if : Complex job dependencies (DAGs), ML pipelines,
              Python ecosystem, visual workflow management,
              team knows Python + Airflow
  Reject if : Simple jobs, small team, heavy ops overhead unwanted

AWS Glue:
  Accept if : On AWS, ETL to/from AWS data stores,
              serverless ETL, Spark-based processing
  Reject if : Not on AWS, not ETL-focused

Azure Data Factory:
  Accept if : On Azure, visual ETL pipeline, Azure data ecosystem
  Reject if : Not on Azure, code-first preferred

Hangfire (.NET):
  Accept if : .NET backend, background jobs, dashboard needed,
              simple to moderate complexity jobs
  Reject if : Not .NET, very high volume, complex DAG dependencies

Quartz.NET / Quartz (Java):
  Accept if : Simple scheduled jobs, existing .NET or Java app,
              no separate scheduling infrastructure wanted
  Reject if : Complex job dependencies, need visual monitoring

Azure Functions / AWS Lambda (Serverless):
  Accept if : Event-triggered jobs, cloud-native, auto-scale,
              jobs with variable/unpredictable frequency
  Reject if : Long-running jobs (>15min), complex state, warm-up needed

Custom Scheduler:
  Accept if : Very simple needs, no framework overhead justified
  Reject if : More than 3-4 jobs — use a real framework

CHOSEN    : [Framework]
Reason    : [Map to language + job count + complexity + cloud platform]
```

#### 8.3 Batch Architecture Design

```
Execution model   : Sequential / Parallel / DAG-based
Parallelism       : [Max parallel jobs] / [Partitioning strategy]
Partitioning      : [How large datasets are split for parallel processing]
Checkpoint/restart: [How job resumes from failure point]
Job store         : [Where job state is persisted]
Monitoring        : [Dashboard for job status + history]
Alerting          : [PagerDuty / Email / Slack on failure]
```

#### 8.4 Batch Job Dependency Map

```
Show which jobs depend on other jobs:

BJ-RPT-001 (Nightly Report)
    depends on: BJ-ETL-001 (Data Aggregation)
        depends on: BJ-SYNC-001 (Data Sync from ERP)

Parallel execution possible:
    BJ-NOTIF-001 ──┐
    BJ-AUDIT-001 ──┤── Can run in parallel (no dependencies)
    BJ-CLEAN-001 ──┘

Sequential required:
    BJ-SYNC-001 → BJ-ETL-001 → BJ-RPT-001
    (each must complete before next starts)
```

#### 8.5 Batch + Queue Interaction

| Batch Job | Queue Interaction | Topic/Queue | Direction |
|-----------|-----------------|-------------|-----------|

---

### SECTION 9: INFRASTRUCTURE & DEPLOYMENT

#### 9.1 Cloud / Infrastructure Decision

```
Decision: Cloud / On-Premise / Hybrid
Provider: AWS / Azure / GCP / [Other]

Reasons (map to BRD):
  ✅ [Reason 1 — e.g., "BRD Section 12 mandates Azure"]
  ✅ [Reason 2 — e.g., "Team has Azure expertise"]
  ✅ [Reason 3 — e.g., "GDPR compliance — EU region available"]

Region(s): [Primary] + [DR region if applicable]
Data residency: [Confirm data stays within required geography]
```

#### 9.2 Environment Strategy

```
| Environment | Purpose | Data | Scale | Access |
|-------------|---------|------|-------|--------|
| Development | Active dev | Synthetic | Minimal | Dev team |
| QA/Testing | QA testing | Synthetic | Moderate | Dev + QA |
| Staging/UAT | Pre-prod testing | Anonymized prod | Near-prod | All teams |
| Production | Live system | Real | Full | Ops + automated |

Environment parity:
  Staging must mirror Production (same config, scaled down)
  Config differences: [Only env-specific secrets/endpoints differ]
```

#### 9.3 CI/CD Pipeline

```
Source Control : Git — [GitHub / GitLab / Azure DevOps / Bitbucket]
Branching      : [GitFlow / Trunk-based / Feature branches]
CI Tool        : [GitHub Actions / Azure DevOps / Jenkins / GitLab CI]
CD Tool        : [ArgoCD / Spinnaker / Azure DevOps Release / Same as CI]

Pipeline Stages:
  1. Code commit → Trigger pipeline
  2. Build        → Compile + unit tests
  3. SAST         → Static code analysis / security scan
  4. Test         → Integration tests
  5. Artifact     → Docker image build + push to registry
  6. Deploy Dev   → Automatic on merge to develop
  7. Deploy QA    → Automatic on merge to main
  8. Deploy Staging→ Manual approval gate
  9. Deploy Prod  → Manual approval gate + change record

Rollback strategy: [Blue-green / Canary / Rollback to previous image]
```

#### 9.4 Containerisation & Orchestration

```
Containerisation : Docker — Yes / No
Orchestration    : Kubernetes / Docker Compose / ECS / None
Registry         : [ECR / ACR / GCR / DockerHub]

If Kubernetes:
  Cluster  : [Managed: AKS / EKS / GKE] vs Self-managed
  Namespaces: [One per environment]
  Ingress  : [Nginx Ingress / Traefik / ALB Ingress]
  Secrets  : [Kubernetes Secrets / Vault / Cloud KMS]
  HPA      : Horizontal Pod Autoscaler — Yes / No
```

#### 9.5 Scalability Design

```
Scaling strategy: Horizontal (add instances) / Vertical (bigger instance)

Auto-scaling triggers:
  Scale out when: CPU > [X]% OR Memory > [X]% OR Request queue > [X]
  Scale in when : CPU < [X]% for [Y] minutes
  Min instances : [X] (always running)
  Max instances : [X] (cost ceiling)

Load balancing:
  Type    : [Application LB / Network LB]
  Strategy: [Round-robin / Least connections / Sticky sessions]
  Health  : [Health check endpoint + interval]

Database scaling:
  Read replicas: [X] replicas for read load
  Connection pool: [X] connections per instance
  Sharding: Not needed / Yes — [strategy]

Caching scaling:
  Redis cluster: [X] nodes
  Cache hit rate target: > [X]%

Peak load handling (from BRD):
  Peak scenario: [e.g., Month-end: 5x normal load]
  Strategy     : [Pre-warm instances / Scheduled scale-out]
```

---

### SECTION 10: SECURITY ARCHITECTURE

#### 10.1 Authentication & Authorization

```
Authentication:
  Mechanism     : OAuth2 / OpenID Connect / SAML / Custom JWT
  Identity Provider: [Azure AD / AWS Cognito / Auth0 / Okta / Custom]
  Token type    : JWT (Access token + Refresh token)
  Access token TTL: [X minutes]
  Refresh token TTL: [X days]
  MFA           : [TOTP / SMS / Email OTP / Hardware key]
  SSO           : Yes ([Provider]) / No

Authorization:
  Model         : RBAC / ABAC / ReBAC
  Enforcement   : API Gateway + Service level (defense in depth)
  Permission check: Every API call checks token + role + resource ownership
  Tenant check  : Every call validates tenant scope
```

#### 10.2 Data Encryption

```
In Transit:
  Protocol      : TLS 1.2+ mandatory (1.3 preferred)
  Certificate   : [Managed cert / Let's Encrypt / Custom CA]
  HSTS          : Enabled with [X] max-age

At Rest:
  DB encryption : AES-256 (cloud-managed keys)
  File storage  : AES-256 (cloud-managed keys)
  Column-level  : PII/PCI fields encrypted at application layer
  Backup encryption: Yes — same standard

Key Management:
  Tool          : [AWS KMS / Azure Key Vault / HashiCorp Vault]
  Rotation      : Every [X] months
  Access        : Least privilege — services only get keys they need
```

#### 10.3 Compliance Implementation

From BRD compliance requirements:
| Regulation | Implementation Approach | Evidence Required |
|------------|------------------------|------------------|
| GDPR | Data deletion API, consent tracking, DPA with vendors | Audit log |
| PCI-DSS | No card storage, tokenization, network isolation | Pen test |
| HIPAA | Encryption, BAA with vendors, audit trail | Assessment |
| SOC 2 | Security controls, monitoring, incident response | Audit |

#### 10.4 Network Security

```
Network isolation:
  VPC/VNET      : Private network for all resources
  Subnets       : Public (LB only) / Private (app) / Isolated (DB)
  Security groups: Least privilege — only needed ports
  DB access     : Only from app subnet — never public
  Bastion host  : For emergency DB access

WAF (Web Application Firewall):
  Required      : Yes / No
  Rules         : OWASP Top 10 protection

DDoS protection:
  Cloud-native  : AWS Shield / Azure DDoS / GCP Cloud Armor
  Rate limiting : API Gateway + WAF
```

#### 10.5 Security Monitoring

```
SIEM          : [Tool: Splunk / Azure Sentinel / AWS GuardDuty]
Log centralization: All service logs → central log store
Alerts        : Failed login spike / Unusual data access / Config change
Pen testing   : Before go-live + annually
Vulnerability scan: Every release [automated SAST/DAST]
Incident response: [Runbook location]
```

---

### SECTION 11: TECH STACK SELECTION

**This section comes LAST — informed by all previous sections.**
**Every choice must be justified by decisions made in Sections 2-10.**

#### 11.1 Tech Stack Decision Framework

For each layer, evaluate options against:
| Criteria | Weight |
|----------|--------|
| Fits chosen architectural pattern | 30% |
| Team already knows it | 20% |
| Hiring availability in location | 15% |
| Community maturity + long-term support | 15% |
| Budget fit (open source vs licensed) | 10% |
| Cloud provider native integration | 10% |

#### 11.2 Final Tech Stack

| Layer | Chosen Technology | Version | Justification |
|-------|-----------------|---------|---------------|
| **Frontend Framework** | [e.g., React 18] | [X.X] | [Map to pattern + FSD complexity] |
| **Frontend State** | [e.g., Zustand] | [X.X] | [Map to app complexity] |
| **Frontend Build** | [e.g., Vite] | [X.X] | [Speed + ecosystem] |
| **UI Component Library** | [e.g., Shadcn/UI] | [X.X] | [Design needs] |
| **Mobile** | [React Native / Flutter / None] | [X.X] | [BRD mobile requirement] |
| **Backend Language** | [e.g., C# / Java / Python / Node] | [X.X] | [Pattern + team skills] |
| **Backend Framework** | [e.g., .NET Core / Spring Boot] | [X.X] | [Map to pattern choice] |
| **API Style** | [REST / GraphQL / gRPC] | — | [Map to Section 4.2] |
| **ORM / Data Access** | [e.g., EF Core / Hibernate] | [X.X] | [Backend framework fit] |
| **Primary Database** | [e.g., PostgreSQL 15] | [X.X] | [Map to Section 6.1] |
| **Read Database** | [e.g., Redis / Elasticsearch] | [X.X] | [CQRS need] |
| **Cache** | [e.g., Redis] | [X.X] | [Performance requirements] |
| **Message Queue** | [e.g., Azure Service Bus] | — | [Map to Section 5] |
| **Search** | [e.g., Elasticsearch / None] | [X.X] | [FSD search requirements] |
| **File Storage** | [e.g., Azure Blob Storage] | — | [Cloud choice] |
| **Batch Framework** | [e.g., Hangfire] | [X.X] | [Map to Section 8.2] |
| **API Gateway** | [e.g., Azure APIM / Kong] | [X.X] | [Map to Section 4.2] |
| **Auth / Identity** | [e.g., Azure AD B2C] | — | [Map to Section 10.1] |
| **Cloud Platform** | [e.g., Microsoft Azure] | — | [Map to Section 9.1] |
| **Container** | [e.g., Docker + AKS] | — | [Map to Section 9.4] |
| **CI/CD** | [e.g., Azure DevOps] | — | [Map to Section 9.3] |
| **Infrastructure as Code** | [e.g., Terraform / Bicep] | [X.X] | [Repeatability] |
| **Monitoring/APM** | [e.g., Azure App Insights] | — | [Cloud + ops needs] |
| **Log Management** | [e.g., ELK Stack / Azure Monitor] | — | [Cloud + ops needs] |
| **Error Tracking** | [e.g., Sentry] | — | [Developer experience] |
| **Secret Management** | [e.g., Azure Key Vault] | — | [Security + cloud] |

#### 11.3 Technology Decisions Rejected

For each major technology considered and rejected:
| Technology | Considered For | Reason Rejected |
|-----------|---------------|----------------|

---

### SECTION 12: OBSERVABILITY & MONITORING

#### 12.1 The Three Pillars

```
LOGS (What happened):
  Format        : Structured JSON logs
  Fields        : timestamp, level, service, traceId, userId,
                  tenantId, message, error (if applicable)
  Tool          : [ELK / Azure Monitor / CloudWatch Logs]
  Retention     : [Hot: 30 days / Warm: 90 days / Archive: 1 year]
  Sensitive data: PII MUST NOT appear in logs

METRICS (How system performs):
  Tool          : [Prometheus + Grafana / Azure Monitor / DataDog]
  Key metrics   : Request rate / Error rate / Latency (P50/P95/P99)
                  CPU/Memory / DB connections / Queue depth
                  Cache hit rate / Batch job duration

TRACES (How requests flow):
  Tool          : [Jaeger / Zipkin / Azure App Insights]
  Sampling      : [100% for errors / X% for normal traffic]
  TraceId       : Propagated across all services + logs
```

#### 12.2 Alerting Strategy

```
| Alert | Condition | Severity | Notify | Response |
|-------|-----------|---------|--------|---------|
| High error rate | >1% 5xx in 5min | P1 | On-call | Immediate |
| High latency | P95 >2s for 5min | P2 | Dev team | 30min |
| DB connections | >80% pool used | P2 | Dev team | 30min |
| Queue DLQ | Any message in DLQ | P2 | Dev team | 1hr |
| Batch job failed | Any job failure | P2 | Dev + Ops | 1hr |
| Disk space | >80% full | P3 | Ops | Next day |
| SSL cert expiry | <30 days | P3 | Ops | Next day |
```

#### 12.3 Health Check Strategy

```
Liveness probe  : Is the application running? (simple ping)
Readiness probe : Is the application ready to receive traffic?
                  (checks DB + cache + queue connectivity)
Deep health     : Full dependency check (admin use)

Endpoint        : GET /health/live → 200 OK
                  GET /health/ready → 200 OK / 503 if not ready
Response time   : < 100ms
```

---

### SECTION 13: ARCHITECTURE DECISION RECORDS (ADRs)

**Record EVERY major decision made in this document.**

For each ADR:

```
ADR-[Number]: [Short Decision Title]
Date        : [Decision date]
Status      : Proposed / Accepted / Deprecated / Superseded by ADR-[X]
Deciders    : [Who made this decision]
────────────────────────────────────────────────────────
CONTEXT:
  Why was this decision needed?
  What forces were at play?
  What constraints existed?

OPTIONS CONSIDERED:
  Option A: [Name] — [Brief description]
    Pros: [List]
    Cons: [List]

  Option B: [Name] — [Brief description]
    Pros: [List]
    Cons: [List]

  Option C: [Name] — [Brief description]
    Pros: [List]
    Cons: [List]

DECISION:
  We chose [Option X] because [specific reasons].
  Evidence: [BRD section / FSD section / team constraint]

CONSEQUENCES:
  Positive  : [What this enables]
  Negative  : [What this costs / constrains]
  Risks     : [What could go wrong with this decision]
  Review    : [When to reconsider this decision]
────────────────────────────────────────────────────────
```

**Minimum ADRs to create for every Architecture Document:**
```
ADR-001: Architectural Pattern Selection
ADR-002: Cloud Provider and Region Selection
ADR-003: Message Queue Technology Selection
ADR-004: Primary Database Selection
ADR-005: Frontend Framework and Pattern
ADR-006: Backend Framework and Language
ADR-007: Authentication and Authorization Strategy
ADR-008: Multi-tenancy Model
ADR-009: Batch Processing Framework
ADR-010: Deployment and Container Strategy
```

---

### SECTION 14: ESTIMATION READINESS

**This section is the direct input to the Estimation prompt.**
Architecture Document is not complete without this section.

#### 14.1 Component Complexity Summary

This table allows the PM to run Bottom-Up Estimation immediately:

**Frontend Components:**
| # | Module | Screen Name | Complexity | Notes |
|---|--------|------------|------------|-------|
From FSD Section 5.1 — confirm complexity with Architecture context.

**Backend Components:**
| # | Module | Operation | Complexity | Architecture Impact |
|---|--------|-----------|------------|---------------------|
| | | | SIMPLE/MED/COMPLEX | [DDD aggregate adds X days] |

Backend Complexity Guide (Architecture-adjusted):
```
SIMPLE  : Basic CRUD, no domain logic, 1-2 DB tables
MEDIUM  : Business rules, domain validation, 3-5 DB tables,
          event publishing
COMPLEX : Aggregate logic, CQRS, saga/workflow, multiple
          events, external service calls, transactions
```

**Batch Jobs:**
| # | Job Code | Job Name | Complexity | Architecture Notes |
|---|----------|---------|------------|-------------------|

**Integrations:**
| # | System | Direction | Complexity | Notes |
|---|--------|-----------|------------|-------|
```
SIMPLE  : REST API, well-documented, sandbox available
MEDIUM  : OAuth, data mapping, error handling needed
COMPLEX : Legacy, SOAP, EDI, no sandbox, complex auth
```

**Infrastructure:**
| Component | Complexity | Notes |
|-----------|------------|-------|
| Cloud setup | MEDIUM | [Environment count × setup effort] |
| CI/CD pipeline | MEDIUM | [Tool choice × complexity] |
| Kubernetes setup | HIGH | [If applicable] |
| Monitoring setup | MEDIUM | [Tool stack] |
| Security controls | MEDIUM-HIGH | [Compliance level] |

#### 14.2 Architecture-Driven Effort Adjusters

Additional effort from architectural decisions — add to base estimate:

| Decision | Additional Effort | Reason |
|----------|-----------------|--------|
| DDD adopted | +15-20% backend | Domain model design + aggregate impl |
| CQRS adopted | +10-15% backend | Two models + sync mechanism |
| Event Sourcing | +20-30% backend | Event store + projection design |
| Microservices | +25-35% DevOps | Service mesh + orchestration |
| Multi-tenant | +10-15% overall | Tenant scoping everywhere |
| High compliance | +15-20% overall | Security controls + audit |
| Message queue | +10% backend | Event publishing + DLQ handling |

#### 14.3 Technical Risks Affecting Estimate

| # | Risk | Probability | Impact | Buffer | Mitigation |
|---|------|-------------|--------|--------|-----------|

Risk categories:
- Integration risk (external APIs may differ from docs)
- Team skill gap (new technology adoption curve)
- Scale risk (performance may need more optimization)
- Compliance risk (regulatory approval timeline)
- Vendor risk (third-party SLA dependency)

#### 14.4 Open Architecture Questions

Items that must be resolved before Estimation is finalized:
| Question | Impact on Estimate | Owner | Deadline |
|---------|-------------------|-------|---------|

---

### SECTION 15: ARCHITECTURE SIGN-OFF

| Role | Name | Signature | Date | Status |
|------|------|-----------|------|--------|
| Principal Architect | | | | Approved |
| Tech Lead | | | | Reviewed |
| Product Manager | | | | Approved |
| Security Lead | | | | Reviewed |
| DevOps Lead | | | | Reviewed |
| Customer/Stakeholder | | | | Approved |

---

## Architecture Completion Checklist

**Do not declare Architecture complete until ALL items checked.**

**Pattern Decision:**
- [ ] ALL patterns evaluated (adopted or rejected with evidence)
- [ ] Chosen combination clearly stated with how patterns combine
- [ ] Trade-offs explicitly documented
- [ ] Every decision mapped to specific BRD/FSD evidence

**Domain Design (if DDD):**
- [ ] Core/Supporting/Generic domains identified
- [ ] All bounded contexts mapped to FSD modules
- [ ] Context map with relationship types
- [ ] Aggregates designed per context
- [ ] Domain events listed
- [ ] Ubiquitous language glossary

**Application Architecture:**
- [ ] Frontend pattern decided with justification
- [ ] API style decided (REST/GraphQL/gRPC)
- [ ] CQRS design (if adopted) — commands + queries listed
- [ ] Service/module boundaries defined
- [ ] Multi-tenancy architecture complete

**Message Queue:**
- [ ] ALL queue technologies evaluated
- [ ] Chosen technology with justification
- [ ] All events/messages from FSD inventoried
- [ ] Topic/queue design per event type
- [ ] DLQ strategy defined
- [ ] Message flow diagrams for key workflows

**Data Architecture:**
- [ ] Database type decision with justification
- [ ] Read DB / cache strategy (if CQRS)
- [ ] Key data models
- [ ] Cache invalidation strategy
- [ ] Data retention and archival

**Batch Architecture:**
- [ ] Batch framework decided with justification
- [ ] All FSD batch jobs covered
- [ ] Job dependency map
- [ ] Monitoring and escalation defined

**Infrastructure:**
- [ ] Cloud platform decided
- [ ] All environments defined
- [ ] CI/CD pipeline stages
- [ ] Scaling strategy
- [ ] Containerisation approach

**Security:**
- [ ] Auth mechanism decided
- [ ] Encryption strategy (transit + rest)
- [ ] Compliance controls from BRD
- [ ] Network security design
- [ ] Security monitoring

**Tech Stack (LAST):**
- [ ] Every layer has a technology choice
- [ ] Every choice justified by prior decisions
- [ ] Rejected technologies documented
- [ ] No technology chosen without pattern justification

**ADRs:**
- [ ] Minimum 10 ADRs created
- [ ] Every major decision has an ADR
- [ ] Every ADR has: context + options + decision + consequences

**Estimation Readiness:**
- [ ] Component complexity summary table complete
- [ ] Architecture-driven effort adjusters calculated
- [ ] Technical risks documented with buffers
- [ ] Open questions listed with owners

**Final Quality Test:**
- [ ] Dev team can build without architectural ambiguity?
- [ ] DevOps can provision infrastructure from this document?
- [ ] PM can produce accurate estimation from this document?
- [ ] Security team can conduct review from this document?
- [ ] Tech stack chosen LAST — after all pattern decisions?
- [ ] Every technology choice justified by pattern evidence?
