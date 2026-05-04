---
name: adr-creation
description: >
  Use this skill whenever a Solution Architect, Tech Lead, or Product Manager
  needs to create an Architecture Decision Record (ADR). Triggers include:
  "create ADR", "write ADR", "document this decision", "record architecture
  decision", "we decided to use X", "why did we choose X", "document why
  we chose", "create decision record", "architecture decision", or any
  situation where a significant technical or architectural decision has been
  made or needs to be made and must be recorded for future reference.
  ADRs capture the WHAT, WHY, OPTIONS considered, CONSEQUENCES, and
  STATUS of every significant decision so future team members understand
  not just what was decided but why — and what was rejected and why.
role: Solution Architect / Tech Lead / Senior Developer / Product Manager
inputs: Decision context + options considered + decision made
version: 1.0
---

# ADR Creation Skill v1.0

## What is an ADR?

An Architecture Decision Record (ADR) is a document that captures:
- A significant decision that was made
- The context that made the decision necessary
- The options that were considered
- The decision that was chosen
- The reasons why it was chosen
- The consequences (positive AND negative) of the choice
- The current status of the decision

```
WHY ADRs MATTER:
─────────────────────────────────────────────────────────
Without ADRs:
  New developer: "Why are we using Kafka? Can we switch to RabbitMQ?"
  Team: Nobody knows. Original architect left 2 years ago.
  Result: Hours of investigation. Wrong decision made. Costly rework.

With ADRs:
  New developer: Reads ADR-005. Understands: Kafka chosen because
                 of 500k messages/day + event replay requirement.
                 RabbitMQ was considered but rejected because of
                 no replay support.
  Result: 2 minutes. Correct decision maintained. Zero waste.
─────────────────────────────────────────────────────────
```

---

## When to Create an ADR

Create an ADR for EVERY decision that meets ANY of these criteria:

```
DECISION IMPACT TRIGGERS — Create ADR if the decision:
□ Affects the overall system structure or architecture
□ Is difficult or costly to reverse later
□ Involves choosing between two or more viable options
□ Will be questioned by future team members
□ Involves a technology, framework, or tool selection
□ Changes an existing approach or established pattern
□ Has significant cost or timeline implications
□ Affects team workflow or development process
□ Involves a security or compliance approach
□ Resolves a disagreement between team members
□ Is made under constraints that may not be obvious later

DO NOT create ADR for:
✗ Trivial decisions (variable naming, minor code style)
✗ Decisions already fully covered by existing standards
✗ Implementation details that don't affect architecture
✗ Decisions that are easily reversed with no consequences
```

### ADR Trigger Phrases

When you hear these → an ADR is needed:
```
"We decided to use [technology]"
"We chose [approach] over [alternative]"
"We're going to [do something differently]"
"We can't use X because [reason]"
"We need to decide between [A] and [B]"
"Why are we doing it this way?"
"Who decided this and why?"
"We're changing from X to Y"
"We're going to live with [limitation] because [reason]"
"We evaluated [options] and chose [X]"
```

---

## ADR Types

Different decisions need different ADR templates:

```
TYPE 1 — TECHNOLOGY SELECTION ADR
  When: Choosing a technology, framework, library, or tool
  Examples: Database choice, frontend framework, queue technology,
            auth provider, monitoring tool, testing framework

TYPE 2 — ARCHITECTURAL PATTERN ADR
  When: Choosing a design pattern or architectural style
  Examples: DDD vs Layered, CQRS adoption, microservices vs monolith,
            multi-tenancy model, API style (REST vs GraphQL)

TYPE 3 — INFRASTRUCTURE ADR
  When: Choosing infrastructure, cloud, or deployment approach
  Examples: Cloud provider, container strategy, CI/CD tool,
            environment strategy, disaster recovery approach

TYPE 4 — SECURITY ADR
  When: Choosing security approach, auth mechanism, or compliance strategy
  Examples: OAuth2 vs SAML, JWT vs session, encryption approach,
            compliance implementation strategy

TYPE 5 — PROCESS / WORKFLOW ADR
  When: Deciding on team process, branching strategy, or workflow
  Examples: Git branching strategy, code review process,
            deployment frequency, API versioning policy

TYPE 6 — DATA ADR
  When: Deciding on data structure, migration, or retention approach
  Examples: Schema design choice, migration strategy, data retention,
            caching strategy, sharding approach

TYPE 7 — REVERSAL / SUPERSEDING ADR
  When: An existing decision is being changed or reversed
  Examples: Migrating from MongoDB to PostgreSQL,
            switching from REST to GraphQL,
            replacing a library with a better alternative
```

---

## ADR Lifecycle

```
PROPOSED ──► ACCEPTED ──► DEPRECATED ──► SUPERSEDED
    │             │              │              │
    │             │              │              └─► Points to new ADR
    │             │              └─► Decision no longer relevant
    │             └─► Decision confirmed and in use
    └─► Under discussion, not yet decided

STATUSES:
  Proposed   : Decision being considered, not yet finalized
  Accepted   : Decision confirmed and implemented/implementing
  Deprecated : Decision no longer relevant (feature removed)
  Superseded : Decision replaced — see ADR-[new number]
  Rejected   : Option considered but NOT chosen (minor ADRs)
```

---

## Superintelligence Principles for ADR Creation

Adapted from Karpathy's excellence principles for decision documentation:

### PRINCIPLE 1 — CAPTURE CONTEXT FIRST, DECISION SECOND
The most valuable part of an ADR is the CONTEXT — not the decision.
The decision is visible in the code. The context exists only in ADR.
Future readers need to understand the constraints and forces at play
MORE than they need to know what was chosen.

### PRINCIPLE 2 — REJECTED OPTIONS ARE AS VALUABLE AS CHOSEN ONES
```
❌ WRONG ADR: "We chose PostgreSQL. It is a great database."

✅ CORRECT ADR:
  "We evaluated PostgreSQL, MongoDB, and DynamoDB.
   MongoDB was rejected because our data is highly relational
   (15 foreign key relationships across 8 tables).
   DynamoDB was rejected because we are not on AWS and
   the team has no DynamoDB expertise.
   PostgreSQL was chosen because it handles our relational
   model, team knows it well, and it runs on any cloud."

A reader who later suggests MongoDB will read the ADR and
immediately understand why it was rejected without asking.
```

### PRINCIPLE 3 — CONSEQUENCES MUST BE HONEST
State ALL consequences — both positive AND negative.
An ADR that only lists positives is marketing, not documentation.
```
✅ Include:
  + What this decision enables
  + What problems it solves
  - What it costs (complexity, performance, learning curve)
  - What it prevents us from doing
  - What debt it creates
  ~ What it leaves neutral or uncertain
```

### PRINCIPLE 4 — FUTURE READER TEST
Before finalising an ADR, ask:
```
"If a new developer joins in 2 years and reads this ADR,
 will they understand:
  ✅ WHY this decision was necessary?
  ✅ What we considered but rejected — and why?
  ✅ What we gave up to get what we chose?
  ✅ When this decision should be revisited?
  ✅ What has changed since this was written?"

If any answer is NO → ADR is incomplete.
```

### PRINCIPLE 5 — LINK EVERYTHING
ADRs are most powerful when linked:
- Link to BRD/FSD sections that motivated the decision
- Link to other ADRs that influence or are influenced by this one
- Link to the ticket/story that triggered this decision
- Link to the PR where this was implemented

### PRINCIPLE 6 — DATE AND CONTEXT DECAY
Technology landscapes change. Always include:
- Exact date of decision
- Team/project context at time of decision
- When this decision should be reviewed
- What change in circumstances would trigger revisiting

### PRINCIPLE 7 — SHORT IS BETTER THAN LONG (WITHIN REASON)
An ADR should be readable in 5 minutes.
If it takes longer: it's too detailed.
If it takes less than 1 minute: it's too shallow.
Target: 1-2 pages per ADR.

---

## Pre-Writing Checklist

Before writing any ADR:
```
□ Is this decision significant enough to warrant an ADR?
   (Use trigger criteria above)
□ Has this decision already been captured in Architecture Document?
   If yes: Reference that section, don't duplicate
□ What is the exact decision being recorded?
   (Write it in one clear sentence before starting)
□ Who made this decision? Who was consulted?
□ What were ALL the options considered?
   (Minimum 2, ideally 3+)
□ What constraints existed? (Budget, timeline, team skills, tech)
□ What evidence supports the chosen option?
□ What are the honest downsides of the chosen option?
□ What would trigger revisiting this decision?
□ What ADR number should this be? (Sequential within project)
```

---

## ADR Numbering Convention

```
Format: ADR-[NNNN]-[short-title-kebab-case]

Examples:
  ADR-0001-database-selection
  ADR-0002-frontend-framework
  ADR-0015-multi-tenancy-model
  ADR-0023-switch-from-rest-to-graphql

Storage location (recommended):
  /docs/adr/
    ├── ADR-0001-database-selection.md
    ├── ADR-0002-frontend-framework.md
    └── ADR-0003-message-queue-selection.md

Index file: /docs/adr/README.md
  (Lists all ADRs with one-line summary + status)
```

---

## ADR Templates

### TEMPLATE 1 — TECHNOLOGY SELECTION ADR (Standard)

```markdown
# ADR-[NNNN]: [Short Decision Title]

**Date**       : [YYYY-MM-DD]
**Status**     : Proposed / Accepted / Deprecated / Superseded by ADR-[NNNN]
**Deciders**   : [Names and roles of decision makers]
**Consulted**  : [Names of people consulted but not decision makers]
**Informed**   : [Teams/people informed after decision]
**Tags**       : [technology / pattern / infrastructure / security / data / process]

---

## Context

[2-4 paragraphs describing:]
- What problem or need triggered this decision
- What constraints existed (budget, timeline, team skills, existing tech)
- What forces were at play (performance, scalability, compliance, etc.)
- Why this decision could not be postponed

Reference: [BRD Section X.X / FSD Section X.X / ARCH Section X.X]
Ticket/Story: [Link to Jira/GitHub issue]

---

## Decision Drivers

The factors that most influenced this decision:

| Driver | Weight | Description |
|--------|--------|-------------|
| [Factor 1] | HIGH | [Why this mattered most] |
| [Factor 2] | HIGH | [Why this mattered] |
| [Factor 3] | MEDIUM | [Why this was considered] |
| [Factor 4] | LOW | [Minor consideration] |

---

## Options Considered

### Option 1: [Technology/Approach Name]

**Description**: [What this option is and how it works]

**Pros**:
- [Advantage 1 — specific to THIS project's needs]
- [Advantage 2]
- [Advantage 3]

**Cons**:
- [Disadvantage 1 — specific to THIS project's context]
- [Disadvantage 2]
- [Disadvantage 3]

**Evaluation against drivers**:
| Driver | Rating | Reason |
|--------|--------|--------|
| [Factor 1] | ✅ Strong | [Reason] |
| [Factor 2] | ⚠️ Neutral | [Reason] |
| [Factor 3] | ❌ Weak | [Reason] |

---

### Option 2: [Technology/Approach Name]

**Description**: [What this option is]

**Pros**:
- [Advantage 1]
- [Advantage 2]

**Cons**:
- [Disadvantage 1]
- [Disadvantage 2]

**Evaluation against drivers**:
| Driver | Rating | Reason |
|--------|--------|--------|
| [Factor 1] | [Rating] | [Reason] |
| [Factor 2] | [Rating] | [Reason] |

---

### Option 3: [Technology/Approach Name] (if applicable)

[Repeat structure above]

---

## Option Comparison Matrix

| Criterion | Weight | Option 1 | Option 2 | Option 3 |
|-----------|--------|----------|----------|----------|
| [Factor 1] | 30% | ✅ Strong | ⚠️ Neutral | ❌ Weak |
| [Factor 2] | 25% | ⚠️ Neutral | ✅ Strong | ✅ Strong |
| [Factor 3] | 20% | ✅ Strong | ✅ Strong | ⚠️ Neutral |
| [Factor 4] | 15% | ✅ Strong | ❌ Weak | ✅ Strong |
| [Factor 5] | 10% | ⚠️ Neutral | ✅ Strong | ❌ Weak |
| **Weighted Score** | | **[X]/10** | **[X]/10** | **[X]/10** |

---

## Decision

**We choose: Option [N] — [Technology/Approach Name]**

**Because**:
1. [Primary reason — maps to highest-weight driver]
2. [Secondary reason]
3. [Supporting reason]

**Evidence from BRD/FSD**:
- [Specific BRD/FSD reference that supports this decision]
- [Another reference]

**Deciding factor over alternatives**:
- Over Option [X]: [Specific reason Option N wins this comparison]
- Over Option [Y]: [Specific reason Option N wins this comparison]

---

## Consequences

### Positive
- ✅ [What this enables — specific]
- ✅ [Performance/scalability benefit — specific]
- ✅ [Team productivity benefit — specific]

### Negative
- ❌ [Cost — be honest: learning curve, operational burden, etc.]
- ❌ [Limitation — what this prevents us from doing]
- ❌ [Technical debt — what we're accepting]

### Neutral / Uncertain
- ~ [Side effect that is neither good nor bad]
- ~ [Unknown that needs monitoring]

### Actions Required
- [ ] [Action 1 needed as result of this decision — Owner: [Name]]
- [ ] [Action 2 — Owner: [Name]]
- [ ] [Update related ADRs: ADR-[NNNN]]

---

## Compliance & Links

**Related ADRs**:
- ADR-[NNNN]: [Title] — [How it relates]
- ADR-[NNNN]: [Title] — [How it relates]

**Supersedes**: ADR-[NNNN] (if replacing a previous decision)

**Implementation**: [PR/commit link when implemented]

**Review Date**: [When to review this decision — e.g., "After 6 months in production" or "When team size exceeds 15"]

**Revisit Triggers**:
- [Condition that would make us reconsider — e.g., "If throughput exceeds 1M messages/day"]
- [Another condition — e.g., "If cloud provider is changed"]
```

---

### TEMPLATE 2 — LIGHTWEIGHT ADR (Quick Decisions)

For smaller decisions that still need recording:

```markdown
# ADR-[NNNN]: [Short Title]

**Date**   : [YYYY-MM-DD]
**Status** : Accepted
**Author** : [Name]

## Decision
[One clear sentence: "We will use X for Y purpose."]

## Context
[2-3 sentences: Why was this decision needed? What constraints existed?]

## Options Considered
- **Option A (chosen)**: [Brief description — why chosen]
- **Option B**: [Brief description — why rejected]
- **Option C**: [Brief description — why rejected]

## Consequences
**Positive**: [Main benefit]
**Negative**: [Main cost or limitation accepted]

## Review
Revisit if: [Condition]
```

---

### TEMPLATE 3 — REVERSAL / SUPERSEDING ADR

When changing a previous decision:

```markdown
# ADR-[NNNN]: Migrate from [Old] to [New]

**Date**        : [YYYY-MM-DD]
**Status**      : Accepted
**Supersedes**  : ADR-[NNNN] — [Old decision title]
**Deciders**    : [Names]

---

## Original Decision (ADR-[NNNN] Summary)
[Brief summary of what was originally decided and why]

## Why the Original Decision is Being Revisited

[What changed since the original decision? Be specific:]
- [Change 1: e.g., "Scale grew from 10k to 500k users — original assumption no longer holds"]
- [Change 2: e.g., "New team skills acquired since original decision"]
- [Change 3: e.g., "Technology landscape changed — better option now exists"]
- [Change 4: e.g., "Original decision created problem X which is now blocking"]

## Problems with Current Approach
- [Problem 1 — specific, measurable if possible]
- [Problem 2]

## New Decision
**We will migrate from [Old] to [New].**

## Why [New] is Better Now
[Why the new choice addresses the problems above]
[Why this option wasn't chosen originally — what was different then]

## Migration Plan
| Step | Description | Owner | Timeline |
|------|-------------|-------|---------|
| 1 | [Preparation] | | |
| 2 | [Migration step] | | |
| 3 | [Validation] | | |
| 4 | [Cutover] | | |
| 5 | [Old system decommission] | | |

## Consequences
**Positive**: [What improves]
**Negative**: [Migration cost, risk, disruption]
**Risks**: [What could go wrong during migration]

## Rollback Plan
If migration fails: [How to revert to old approach]
```

---

### TEMPLATE 4 — PATTERN / APPROACH ADR

For DDD, CQRS, Microservices, and similar architectural pattern decisions:

```markdown
# ADR-[NNNN]: [Pattern Name] Adoption

**Date**     : [YYYY-MM-DD]
**Status**   : Accepted
**Deciders** : [Names]
**Scope**    : [Entire system / Module X only / New modules only]

---

## Context
[What characteristics of the system make this pattern relevant?]
[What problem does this pattern solve for us specifically?]

Reference to BRD/FSD evidence:
- FSD shows [X] bounded contexts across [X] modules
- FSD shows [X] business rules — domain logic is complex
- BRD requires [specific NFR] that this pattern addresses

---

## Pattern Description
**[Pattern Name]**: [Brief description of what this pattern is]

How it applies to our system:
[Specific explanation of how we will implement this pattern
 in our context — not a generic definition]

---

## Alternatives Considered

### Without this pattern: [Alternative approach]
**Why insufficient**:
- [Specific reason 1 why the simpler approach won't work for us]
- [Specific reason 2]

### Partial adoption: [Limited scope option]
**Considered**: Apply only to [module/area]
**Decision**: [Full adoption / Partial adoption chosen — why]

---

## Implementation Approach

**Scope of adoption**: [What applies this pattern, what doesn't]

**Boundaries**:
- Applied to: [Specific modules/components]
- NOT applied to: [Modules where pattern is overkill]
- Applied from: [Sprint X / Milestone Y]

**Key implementation decisions within this pattern**:
| Sub-decision | Choice | Reason |
|-------------|--------|--------|
| [Aspect 1] | [Choice] | [Reason] |
| [Aspect 2] | [Choice] | [Reason] |

---

## Consequences

### Positive
- ✅ [Benefit 1 — specific to our system]
- ✅ [Benefit 2]

### Negative
- ❌ [Cost 1 — honest: additional complexity, learning curve]
- ❌ [Cost 2 — what we are giving up]

### Team Impact
- Learning required: [What the team needs to learn]
- Estimated ramp-up: [X weeks for team to be productive]
- Reference materials: [Books, courses, examples]

---

## Guard Rails
Rules the team MUST follow when implementing this pattern:
1. [Rule 1 — e.g., "Aggregates must never reference other aggregates by object, only by ID"]
2. [Rule 2]
3. [Rule 3]

Red flags that indicate we're implementing it wrong:
- [Anti-pattern 1 to watch for]
- [Anti-pattern 2]
```

---

### TEMPLATE 5 — SECURITY / COMPLIANCE ADR

```markdown
# ADR-[NNNN]: [Security Decision Title]

**Date**       : [YYYY-MM-DD]
**Status**     : Accepted
**Deciders**   : [Names including Security Lead]
**Sensitivity**: [Public / Internal / Confidential]

---

## Context
[What security requirement or compliance mandate drove this decision?]
[What risk is being mitigated?]

BRD compliance reference: [BRD Section 6.4-6.5]
Regulation: [GDPR / PCI-DSS / HIPAA / SOC2 / etc.]
Risk: [What happens if this is not addressed]

---

## Threat Model (if applicable)
| Threat | Likelihood | Impact | Mitigation |
|--------|-----------|--------|-----------|
| [Threat 1] | High/Med/Low | High/Med/Low | [This decision addresses this] |

---

## Options Considered
[Same structure as Template 1 options section]

---

## Decision
[What security approach/control was chosen]

**Security properties guaranteed by this decision**:
- Confidentiality: [What is protected and how]
- Integrity: [What ensures data hasn't been tampered]
- Availability: [What prevents denial of service]
- Non-repudiation: [What provides audit trail]

---

## Consequences
### Positive
- ✅ [Security improvement 1]
- ✅ [Compliance requirement satisfied]

### Negative
- ❌ [Performance impact — quantified if possible]
- ❌ [Developer experience impact]
- ❌ [Operational complexity added]

### Compliance Obligations Created
- [Any ongoing obligation: e.g., "Audit logs must be reviewed monthly"]
- [Certification: e.g., "Annual pen test required"]

---

## Security Review
- Reviewed by: [Security Lead name]
- Pen test required: Yes / No
- Review frequency: [Annual / Per release / Triggered by X]
```

---

## ADR Index Template

Maintain this index file in `/docs/adr/README.md`:

```markdown
# Architecture Decision Records Index

## Active Decisions

| ADR # | Title | Status | Date | Category |
|-------|-------|--------|------|---------|
| ADR-0001 | [Title] | Accepted | [Date] | Technology |
| ADR-0002 | [Title] | Accepted | [Date] | Pattern |
| ADR-0003 | [Title] | Proposed | [Date] | Infrastructure |

## Superseded Decisions
| ADR # | Title | Superseded By | Date |
|-------|-------|--------------|------|
| ADR-00XX | [Title] | ADR-00YY | [Date] |

## Deprecated Decisions
| ADR # | Title | Reason | Date |
|-------|-------|--------|------|

## Decision Categories
- **Technology**: Database, framework, library, tool choices
- **Pattern**: Architectural patterns (DDD, CQRS, microservices)
- **Infrastructure**: Cloud, deployment, CI/CD choices
- **Security**: Auth, encryption, compliance approaches
- **Data**: Schema, migration, retention decisions
- **Process**: Team workflow, branching, versioning decisions
```

---

## Standard ADRs Every Project Needs

**Minimum set of ADRs for any project (links to Architecture Document):**

```
ADR-0001: Architectural Pattern Selection
          (DDD / CQRS / Microservices / Monolith decision)

ADR-0002: Cloud Provider and Region
          (AWS / Azure / GCP + regions for compliance)

ADR-0003: Primary Database Selection
          (PostgreSQL / MongoDB / etc.)

ADR-0004: Message Queue / Event Streaming
          (Kafka / RabbitMQ / Azure Service Bus / none)

ADR-0005: Frontend Framework
          (React / Vue / Angular + SSR/SPA/MPA)

ADR-0006: Backend Language and Framework
          (.NET Core / Spring Boot / Django / etc.)

ADR-0007: Authentication and Authorization
          (OAuth2 / JWT / SAML / SSO provider)

ADR-0008: Multi-tenancy Model
          (Single-tenant / Shared schema / Separate DB)

ADR-0009: API Style
          (REST / GraphQL / gRPC)

ADR-0010: Batch Processing Framework
          (Spring Batch / Airflow / Hangfire / etc.)

ADR-0011: CI/CD Approach
          (GitHub Actions / Azure DevOps / Jenkins)

ADR-0012: Container and Orchestration Strategy
          (Docker + Kubernetes / ECS / App Service / none)

ADR-0013: Observability Stack
          (Logging / Metrics / Tracing tools)

ADR-0014: Code Repository and Branching Strategy
          (Git + GitFlow / Trunk-based / etc.)

ADR-0015: API Versioning Strategy
          (URI / Header / Query param versioning)
```

**Additional ADRs triggered during development:**
```
ADR-001X: Caching strategy (Redis / Memcached / in-memory)
ADR-001X: Search technology (Elasticsearch / DB full-text)
ADR-001X: File storage approach (S3 / Blob / GridFS)
ADR-001X: Email service provider (SendGrid / SES / SMTP)
ADR-001X: Specific library choice when alternatives exist
ADR-001X: Data migration approach (big bang / phased)
ADR-001X: Feature flag strategy (LaunchDarkly / custom)
ADR-001X: Test strategy (unit / integration / E2E tools)
ADR-001X: Code style and linting rules
ADR-001X: Error handling and response format standard
```

---

## ADR Quality Checklist

Before declaring an ADR complete:

**Content Completeness:**
- [ ] Status is set correctly
- [ ] Date is correct
- [ ] Deciders and consulted parties named
- [ ] Context explains WHY decision was needed (not just what)
- [ ] Constraints that influenced decision are explicit
- [ ] Minimum 2 options considered (ideally 3+)
- [ ] Each option has BOTH pros AND cons
- [ ] Rejected options have clear reasons for rejection
- [ ] Comparison matrix or scoring if multiple options
- [ ] Decision stated clearly in one sentence
- [ ] Decision justified with evidence (BRD/FSD/ARCH refs)
- [ ] Why it beats each rejected option — specific
- [ ] Consequences include BOTH positive AND negative
- [ ] Actions required with owners listed
- [ ] Review date or revisit triggers specified

**Readability:**
- [ ] Readable in under 5 minutes
- [ ] Future reader test passed (see Principle 4)
- [ ] No jargon without explanation
- [ ] Numbered correctly and filed in index
- [ ] Links to related ADRs included

**Honesty Check:**
- [ ] Does this ADR only list positives? (If yes — incomplete)
- [ ] Is the rejected option's reasoning specific? (Not just "it's worse")
- [ ] Are the negative consequences specific? (Not vague)
- [ ] Is there a revisit trigger? (All decisions should be revisitable)

---

## Common ADR Mistakes to Avoid

| Mistake | Example | Fix |
|---------|---------|-----|
| Decision without context | "We chose Redis" | Explain what caching problem Redis solves |
| No rejected options | "We chose PostgreSQL" | Document MongoDB and DynamoDB considered |
| Only positives listed | "React is great because..." | Include: learning curve, bundle size concerns |
| Vague rejection reason | "MongoDB wasn't suitable" | "MongoDB rejected because 15 FK relationships make document model complex" |
| No revisit trigger | Decision is permanent forever | "Review if data volume exceeds 10TB" |
| Generic description | Copy-paste from Wikipedia | Explain how it applies to THIS system specifically |
| Too long | 10-page ADR | Max 2 pages — move detail to separate design doc |
| Too short | 3-line ADR | Needs context + options + honest consequences |
| Missing date | Undated ADR | Always date — technology landscape changes |
| Superseded without link | Old ADR still says "Accepted" | Update status + link to new ADR |

---

## ADR Generation Prompt

When you have the decision context ready, use this to generate the ADR:

```
I need to create an ADR for the following decision.
Use the ADR Creation Skill — apply Template [1/2/3/4/5] based on
the decision type.

Decision Type: [Technology / Pattern / Infrastructure / Security / Reversal]
ADR Number   : ADR-[NNNN]
Project      : [Project name]

DECISION MADE:
[One sentence describing exactly what was decided]

CONTEXT:
[Why was this decision needed? What problem triggered it?
 What constraints existed? What BRD/FSD sections are relevant?]

OPTIONS WE CONSIDERED:
Option 1: [Name] — [Brief description]
Option 2: [Name] — [Brief description]
Option 3: [Name if applicable]

WHY WE CHOSE Option [N]:
[Reasons for the choice]

WHY WE REJECTED the others:
Option [X] rejected because: [Reason]
Option [Y] rejected because: [Reason]

TEAM:
Decided by  : [Names]
Consulted   : [Names]

CONSTRAINTS THAT INFLUENCED DECISION:
- [Constraint 1: budget / timeline / skills / existing tech]
- [Constraint 2]

Please generate a complete ADR using the appropriate template,
ensuring all sections are complete, options have honest pros/cons,
consequences include both positive and negative, and a revisit
trigger is specified.
```

---

## ADR Maintenance Rules

```
RULE 1 — NEVER DELETE AN ADR
  Even superseded ADRs must be kept.
  History is valuable. Mark as "Superseded by ADR-NNNN".

RULE 2 — NEVER EDIT A FINALIZED ADR (except status/links)
  If a decision changes: create a NEW ADR that supersedes this one.
  The original reasoning must remain readable for historical context.

RULE 3 — UPDATE THE INDEX IMMEDIATELY
  Every new ADR → update /docs/adr/README.md the same day.

RULE 4 — LINK BOTH WAYS
  If ADR-0010 supersedes ADR-0005:
  - ADR-0010 says "Supersedes ADR-0005"
  - ADR-0005 status updated to "Superseded by ADR-0010"

RULE 5 — ADR IN PULL REQUEST
  For architecture decisions made during implementation:
  Create ADR in the same PR that implements the decision.

RULE 6 — REVIEW PERIODICALLY
  At least annually: review all "Accepted" ADRs.
  Check: Is this still the right decision?
  Have any revisit triggers been met?

RULE 7 — TEAM AWARENESS
  New team members read ALL ADRs before writing code.
  ADR index is part of project onboarding checklist.
```
