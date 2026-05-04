---
name: estimation-creation
description: >
  Use this skill whenever a Product Manager, Tech Lead, or Solution Architect
  needs to create a Bottom-Up Effort Estimation from BRD, FSD, and Architecture
  documents. Triggers include: "create estimation", "prepare estimate",
  "effort estimation", "how long will this take", "project estimate",
  "resource estimation", "story point estimation", "person-day estimate",
  or any request to quantify effort for a software project. This skill
  produces a fully transparent, matrix-driven estimation with per-module,
  per-screen, per-API, per-batch-job breakdowns that can be independently
  adjusted. All assumptions are explicit. All complexity ratings are
  justified. Final output is a master matrix with person-days (PD) per
  section, per module, and a consolidated summary the PM can adjust.
role: Senior PM / Solution Architect / Estimation Lead
inputs: Completed BRD + Completed FSD + Completed Architecture Document
version: 1.0
---

# Estimation Creation Skill v1.0

## Core Philosophy

### Bottom-Up Estimation — Why It's Most Accurate

```
TOP-DOWN (Avoid for commitments):
  Guess the whole → split into parts → 40-60% accurate
  "This looks like a 6-month project" → dangerous

ANALOGY (Useful only for pre-sales):
  "Similar to Project X which took Y months" → 50-65% accurate
  No two projects are the same

BOTTOM-UP (This skill — use for planning):
  Estimate every screen + API + batch job + integration
  Roll up → adjust for complexity → add buffer → 80-90% accurate
  Every number is traceable, every assumption is visible
  PM can adjust any row → total recalculates

THREE-POINT (Use alongside Bottom-Up for risk):
  O = Optimistic / M = Most Likely / P = Pessimistic
  E = (O + 4M + P) / 6  ← PERT formula
  Produces confidence range, not just single number
```

### The Estimation Matrix Philosophy

Every number in this estimation must be:
```
TRACEABLE  → Links back to specific FSD screen/API/job
JUSTIFIED  → Complexity rating with reason
ADJUSTABLE → PM can change any row independently
TRANSPARENT→ No black-box totals
VERSIONED  → Changes tracked with reason
```

---

## Superintelligence Principles for Estimation

### PRINCIPLE 1 — NEVER ESTIMATE FROM MEMORY
Every estimate must reference a specific:
- FSD screen code (SCR-MOD-XXX)
- FSD data contract code (DC-MOD-XXX)
- FSD business logic code (BL-MOD-XXX)
- FSD batch job code (BJ-MOD-XXX)
- Architecture section

### PRINCIPLE 2 — COMPLEXITY IS ARGUED, NOT ASSUMED
```
❌ WRONG: "Order Detail screen — MEDIUM — 5 days"

✅ CORRECT: "SCR-ORD-003 Order Detail — HIGH — 10 days
  Reason: Pre-filled form with 18 fields (FORM spec),
  3 dynamic field groups (show/hide rules BR-ORD-007),
  inline approval workflow (4-step), real-time status
  polling (WebSocket), concurrent edit detection,
  FSD Section 4.2 confirmed HIGH complexity rating"
```

### PRINCIPLE 3 — ARCHITECTURE ADDS EFFORT
Architectural decisions add real effort beyond FSD:
```
DDD adopted        → Each aggregate needs design + impl
CQRS adopted       → Every operation has two models
Event Sourcing     → Event store + projections
Microservices      → Service mesh + API gateway + containers
Multi-tenancy      → Tenant scoping on every component
High compliance    → Security controls + audit everywhere
```
These are NOT captured in per-screen estimates — add explicitly.

### PRINCIPLE 4 — INTEGRATION EFFORT IS ALWAYS UNDERESTIMATED
Standard rule: Double your first integration estimate.
```
What devs think: "It's just an API call — 1 day"
Reality:
  → Read integration docs: 0.5 day
  → Get sandbox credentials: 0.5-2 days
  → Implement happy path: 1 day
  → Handle all error responses: 1 day
  → Handle rate limits + retries: 0.5 day
  → Write integration tests: 0.5 day
  → Debug in staging: 0.5 day
  Total: 4-5 days for a "simple" API
```

### PRINCIPLE 5 — QA IS 25-35% OF DEV — ALWAYS
This is the most commonly skipped line item.
QA estimate = 30% of total development effort (minimum).
Never zero. Never "they'll do it in parallel."

### PRINCIPLE 6 — PRODUCE AN ADJUSTABLE MATRIX
The PM must be able to:
- Change complexity of any screen → total updates
- Remove a feature → see exact PD impact
- Add buffer % → see adjusted total
- Change team size → see timeline impact
Every row must be independently adjustable.

---

## Pre-Estimation Checklist

```
□ Read BRD: scope boundaries, constraints, team size, budget
□ Read FSD Master Screen Inventory (Section 5.1) — count screens
□ Read FSD Master Data Contract List (Section 5.2) — count operations
□ Read FSD Master Business Logic List (Section 5.3) — count operations
□ Read FSD Master Batch Job List (Section 5.4) — count + volume
□ Read FSD Master Integration List (Section 5.5)
□ Read Architecture: pattern complexity, tech stack, infra design
□ Read Architecture Section 14: Estimation Readiness table
□ Note architectural effort adjusters from ARCH Section 14.2
□ Confirm team composition (roles + count + seniority)
□ Confirm working hours per day (productive hours, not clock hours)
□ Confirm sprint length (typically 2 weeks)
□ Note any BRD constraints: budget ceiling, hard deadlines
□ List all [OPEN QUESTION] from FSD — each is a risk
□ List all [ARCHITECTURE CONFLICT] from ARCH — each is a risk
```

---

## Estimation Parameters (Set Before Starting)

```
TEAM COMPOSITION:
  Tech Lead / Architect    : [X] person(s)
  Senior Backend Developer : [X] person(s)
  Backend Developer        : [X] person(s)
  Senior Frontend Developer: [X] person(s)
  Frontend Developer       : [X] person(s)
  Mobile Developer         : [X] person(s) [if applicable]
  DevOps Engineer          : [X] person(s)
  QA Engineer              : [X] person(s)
  Project Manager          : [X] person(s)
  Business Analyst         : [X] person(s)
  Total Team Size          : [X] persons

WORKING PARAMETERS:
  Productive hours/day     : 6 hours (not 8 — meetings, reviews, admin)
  Working days/week        : 5 days
  Sprint length            : 2 weeks (10 working days)
  Team velocity (if known) : [X] story points per sprint [optional]

COMPLEXITY BENCHMARKS — FRONTEND (Person-Days):
  LOW screen    : [2] PD  (simple view, <5 fields, no rules)
  MEDIUM screen : [4] PD  (form 6-15 fields, filters, validations)
  HIGH screen   : [8] PD  (wizard, dashboard, real-time, complex rules)
  Modal/Popup   : [1] PD  per modal

  ADJUST IF:
  + Add 50% if screen has real-time (WebSocket/polling)
  + Add 30% if screen has drag-and-drop
  + Add 20% if screen has file upload with preview
  + Add 30% if screen has complex chart (multiple series)

COMPLEXITY BENCHMARKS — BACKEND OPERATIONS (Person-Days):
  SIMPLE operation  : [1] PD  (basic CRUD, 1-2 tables, no rules)
  MEDIUM operation  : [2] PD  (business rules, validations, 3-5 tables)
  COMPLEX operation : [4] PD  (aggregate logic, events, transactions,
                               external calls, CQRS, workflow)

  ADJUST IF (Architecture-driven):
  + Add 1 PD if DDD aggregate design required
  + Add 1 PD if CQRS command+query models needed
  + Add 0.5 PD if event published after operation
  + Add 1 PD if saga/workflow coordination needed

COMPLEXITY BENCHMARKS — BATCH JOBS (Person-Days):
  SIMPLE batch  : [2] PD  (single step, low volume, DB to DB)
  MEDIUM batch  : [4] PD  (multi-step, file handling, retry logic)
  COMPLEX batch : [8] PD  (high volume, parallel, external API,
                            complex transformation, reconciliation)

COMPLEXITY BENCHMARKS — INTEGRATIONS (Person-Days):
  SIMPLE        : [3] PD  (REST, documented, sandbox available)
  MEDIUM        : [6] PD  (OAuth, data mapping, error handling)
  COMPLEX       : [12] PD (legacy, SOAP, EDI, no sandbox, complex auth)

RISK BUFFER:
  LOW risk    : +15%  (few unknowns, tech familiar, clear scope)
  MEDIUM risk : +25%  (some unknowns, new tech elements)
  HIGH risk   : +40%  (many unknowns, new tech, complex integrations)
```

---

## Estimation Document Structure

---

### SECTION 0: DOCUMENT CONTROL

```
Document Title      : Effort Estimation — [System Name]
Version             : v1.0
Prepared By         : [Name / Role]
Reference BRD       : BRD-[Code]-[Version]
Reference FSD       : FSD-[Code]-[Version]
Reference ARCH      : ARCH-[Code]-[Version]
Date Created        : [Date]
Last Updated        : [Date]
Estimation Method   : Bottom-Up + Three-Point for high-risk items
Estimation Unit     : Person-Days (PD) — 1 PD = 6 productive hours
```

**Revision History**
| Version | Date | Author | What Changed | Impact on Total |
|---------|------|--------|-------------|-----------------|

---

### SECTION 1: SCOPE CONFIRMATION

**1.1 In-Scope for This Estimation**
Everything being estimated — from BRD Section 2.1:
| # | Module / Feature | Priority | Phase | Estimated |
|---|-----------------|---------|-------|----------|

**1.2 Explicitly Out of Scope**
Everything NOT included — prevents scope creep disputes:
| # | Item | Reason Excluded |
|---|------|----------------|

**1.3 Estimation Assumptions**

```
Format: EA-[Number]: [Assumption] — Impact if wrong: [impact on estimate]

EA-001: Team works [6] productive hours per day (not 8)
        Impact if wrong: Every 1hr/day increase = -15% timeline
EA-002: All [OPEN QUESTION] items in FSD are RESOLVED before dev starts
        Impact if wrong: Each unresolved OQ = avg +2 days rework per module
EA-003: Third-party sandbox environments available within [2] weeks of start
        Impact if wrong: Integration work delayed, blocking critical path
EA-004: Customer provides test data for UAT within [2] weeks of staging
        Impact if wrong: UAT phase delayed by [X] weeks
EA-005: No data migration unless explicitly included in scope
        Impact if wrong: Add [X] PD for data migration if required
EA-006: Dev team has cloud environment access from Day 1
        Impact if wrong: +5-10 PD for environment setup delays
EA-007: Requirements frozen after FSD sign-off
        Impact if wrong: Each major change request adds [X] PD
EA-008: UAT conducted by customer team, not dev team
        Impact if wrong: Add [X] PD for dev team UAT support
EA-009: Performance testing environment mirrors production
        Impact if wrong: Performance issues found in prod = emergency effort
EA-010: All architecture decisions are final before development starts
        Impact if wrong: Mid-project architecture change = major rework

[Add all FSD assumptions ASM-001 through ASM-020 as inherited assumptions]
```

---

### SECTION 2: FRONTEND ESTIMATION MATRIX

**Instructions for PM:**
```
- Each row = one screen from FSD Section 5.1
- Complexity PD = base days from parameters above
- Adjustments = additional days for special features
- Total PD = Complexity PD + Adjustments
- You can change any Complexity or Adjustment cell
- Module subtotals and grand total auto-explain below
```

#### 2.1 Per-Screen Estimation Matrix

| # | FSD Code | Module | Screen Name | Type | Complexity | Base PD | Real-time +? | File Upload +? | Charts +? | Other + | Total PD | Notes |
|---|----------|--------|------------|------|------------|---------|-------------|----------------|-----------|---------|---------|-------|
| 1 | SCR-AUTH-001 | Auth | Login Screen | AUTH | LOW | 2 | — | — | — | — | 2 | Standard login |
| 2 | SCR-AUTH-002 | Auth | Forgot Password | AUTH | LOW | 2 | — | — | — | — | 2 | Email flow |
| 3 | SCR-AUTH-003 | Auth | Reset Password | AUTH | LOW | 2 | — | — | — | — | 2 | Token validation |
| 4 | SCR-AUTH-004 | Auth | MFA Verification | AUTH | MEDIUM | 4 | — | — | — | — | 4 | OTP entry |
| 5 | SCR-[MOD]-001 | [Module] | [Screen Name] | [Type] | [LOW/MED/HIGH] | [X] | [+X or —] | [+X or —] | [+X or —] | [+X] | [Total] | [Reason] |

**Column Key:**
```
FSD Code    : Links to FSD Section 5.1 — traceability
Complexity  : LOW=2PD / MEDIUM=4PD / HIGH=8PD (base)
Base PD     : From complexity benchmark
Real-time + : +1 to +3 PD if WebSocket/polling needed (from FSD DC code)
File Upload+: +1 to +2 PD if file upload with preview (from FSD spec)
Charts +    : +1 to +3 PD per complex chart widget
Other +     : [Drag-drop +2 / Wizard extra steps +1 each / etc.]
Total PD    : Base PD + all adjustments
Notes       : Key FSD reference or justification
```

#### 2.2 Frontend Module Subtotals

| Module | Screens | LOW | MEDIUM | HIGH | Modals | Base PD | Adjustments | Module Total PD |
|--------|---------|-----|--------|------|--------|---------|-------------|----------------|
| Auth | [X] | [X] | [X] | [X] | [X] | [X] | [X] | **[X]** |
| [Module 2] | [X] | [X] | [X] | [X] | [X] | [X] | [X] | **[X]** |
| [Module N] | [X] | [X] | [X] | [X] | [X] | [X] | [X] | **[X]** |
| **TOTAL** | **[X]** | **[X]** | **[X]** | **[X]** | **[X]** | **[X]** | **[X]** | **[X]** |

#### 2.3 Frontend Additional Effort

| # | Task | PD | Justification |
|---|------|----|---------------|
| FE-ADD-001 | Project setup + base architecture | [X] | One-time: routing, state mgmt, component lib, i18n setup |
| FE-ADD-002 | Authentication flow + token management | [X] | JWT handling, refresh, interceptors, session timeout |
| FE-ADD-003 | Shared layout components | [X] | Header, sidebar, breadcrumb, footer, navigation |
| FE-ADD-004 | Shared reusable components | [X] | Table, form, modal, pagination, file upload components |
| FE-ADD-005 | API integration layer | [X] | HTTP client, error handling, loading states, retry |
| FE-ADD-006 | Real-time infrastructure | [X] | WebSocket setup (if applicable — from ARCH Section 4.1) |
| FE-ADD-007 | Responsive / mobile breakpoints | [X] | All screens adapted for mobile (from FSD Section 6.9) |
| FE-ADD-008 | Internationalisation setup | [X] | i18n library, translation keys (if multi-language in BRD) |
| FE-ADD-009 | Accessibility implementation | [X] | ARIA labels, keyboard nav, WCAG compliance |
| FE-ADD-010 | Frontend unit tests | [X] | Core components + critical flows (target: [X]% coverage) |
| FE-ADD-011 | Performance optimisation | [X] | Code splitting, lazy loading, bundle size |
| FE-ADD-012 | Cross-browser testing fixes | [X] | From FSD Section 7.5 browser matrix |
| **SUBTOTAL** | | **[X]** | |

#### 2.4 Frontend Total Summary

```
┌─────────────────────────────────────────────────────┐
│              FRONTEND ESTIMATION SUMMARY             │
├─────────────────────────────────────────────────────┤
│ Screen Matrix Total (Section 2.1-2.2)               │
│   LOW screens    : [X] × 2 PD     =  [X] PD        │
│   MEDIUM screens : [X] × 4 PD     =  [X] PD        │
│   HIGH screens   : [X] × 8 PD     =  [X] PD        │
│   Modals         : [X] × 1 PD     =  [X] PD        │
│   Adjustments    :                =  [X] PD        │
│   Screen Subtotal                 = [XX] PD        │
│                                                     │
│ Additional Effort (Section 2.3)   = [XX] PD        │
│                                                     │
│ FRONTEND TOTAL                    = [XX] PD        │
│ As % of total project             =  [X]%          │
└─────────────────────────────────────────────────────┘
```

---

### SECTION 3: BACKEND ESTIMATION MATRIX

#### 3.1 Per-Operation Estimation Matrix

| # | FSD Code | Module | Operation Name | Op Type | Complexity | Base PD | DDD +? | CQRS +? | Event +? | Saga +? | Multi-tenant +? | Total PD | Notes |
|---|----------|--------|---------------|---------|------------|---------|--------|---------|---------|---------|----------------|---------|-------|
| 1 | BL-AUTH-001 | Auth | Authenticate User | User-init | MEDIUM | 2 | — | — | +0.5 | — | — | 2.5 | Token issue + MFA check |
| 2 | BL-AUTH-002 | Auth | Refresh Token | System | SIMPLE | 1 | — | — | — | — | — | 1 | Token rotation |
| 3 | BL-[MOD]-001 | [Module] | [Operation] | [Type] | [SIMP/MED/COMP] | [X] | [+X/—] | [+X/—] | [+X/—] | [+X/—] | [+X/—] | [Total] | [Notes] |

**Column Key:**
```
FSD Code     : Links to FSD Section 5.3 — traceability
Op Type      : User-initiated / System / Event-triggered / Scheduled
Base PD      : SIMPLE=1 / MEDIUM=2 / COMPLEX=4
DDD +        : +1 PD if aggregate design + implementation needed
CQRS +       : +1 PD if separate command + query models required
Event +      : +0.5 PD per domain event published by this operation
Saga +       : +1 to +2 PD if multi-step saga coordination needed
Multi-tenant+: +0.5 PD if complex tenant isolation logic required
Total PD     : Base + all adjustments
```

#### 3.2 Backend Module Subtotals

| Module | Operations | SIMPLE | MEDIUM | COMPLEX | Base PD | Arch Adjustments | Module Total PD |
|--------|-----------|--------|--------|---------|---------|-----------------|----------------|
| Auth | [X] | [X] | [X] | [X] | [X] | [X] | **[X]** |
| [Module 2] | [X] | [X] | [X] | [X] | [X] | [X] | **[X]** |
| [Module N] | [X] | [X] | [X] | [X] | [X] | [X] | **[X]** |
| **TOTAL** | **[X]** | **[X]** | **[X]** | **[X]** | **[X]** | **[X]** | **[X]** |

#### 3.3 Backend Additional Effort

| # | Task | PD | Justification |
|---|------|----|---------------|
| BE-ADD-001 | Project setup + base architecture | [X] | Solution structure, DI setup, middleware, logging |
| BE-ADD-002 | Authentication + authorization framework | [X] | JWT/OAuth implementation, role checks, middleware |
| BE-ADD-003 | Database schema design + migrations | [X] | All entities from FSD Section 5.8 — [X] entities |
| BE-ADD-004 | DDD domain model implementation | [X] | Aggregates, Value Objects, Domain Services (if DDD) |
| BE-ADD-005 | CQRS infrastructure setup | [X] | Command bus, query bus, separate models (if CQRS) |
| BE-ADD-006 | Event publishing infrastructure | [X] | Event dispatcher, queue publisher (if EDA) |
| BE-ADD-007 | Multi-tenancy middleware | [X] | Tenant resolution, data scoping (if multi-tenant) |
| BE-ADD-008 | API versioning setup | [X] | Versioning middleware, routing |
| BE-ADD-009 | API documentation | [X] | Swagger/OpenAPI generation and maintenance |
| BE-ADD-010 | Global error handling + logging | [X] | Standard error responses, structured logging |
| BE-ADD-011 | Audit trail implementation | [X] | Audit log service, interceptors (from BRD Section 6.4) |
| BE-ADD-012 | Performance optimisation | [X] | Query optimisation, N+1 fixes, index tuning |
| BE-ADD-013 | Backend unit + integration tests | [X] | Target [X]% coverage — core services + APIs |
| BE-ADD-014 | Security implementation | [X] | Input validation, CSRF, rate limiting, encryption |
| **SUBTOTAL** | | **[X]** | |

#### 3.4 Backend Total Summary

```
┌─────────────────────────────────────────────────────┐
│             BACKEND ESTIMATION SUMMARY              │
├─────────────────────────────────────────────────────┤
│ Operation Matrix Total (Section 3.1-3.2)            │
│   SIMPLE operations  : [X] × 1 PD  =  [X] PD       │
│   MEDIUM operations  : [X] × 2 PD  =  [X] PD       │
│   COMPLEX operations : [X] × 4 PD  =  [X] PD       │
│   Architecture adj.  :             =  [X] PD       │
│   Operations Subtotal              = [XX] PD       │
│                                                     │
│ Additional Effort (Section 3.3)    = [XX] PD       │
│                                                     │
│ BACKEND TOTAL                      = [XX] PD       │
│ As % of total project              =  [X]%         │
└─────────────────────────────────────────────────────┘
```

---

### SECTION 4: BATCH JOBS ESTIMATION MATRIX

#### 4.1 Per-Job Estimation Matrix

| # | FSD Code | Module | Job Name | Trigger | Frequency | Volume | Complexity | Base PD | Framework Setup +? | Parallel +? | Monitoring +? | Total PD | Notes |
|---|----------|--------|---------|---------|-----------|--------|------------|---------|-------------------|------------|--------------|---------|-------|
| 1 | BJ-[MOD]-001 | [Module] | [Job Name] | Scheduled | Daily | [X] records | SIMPLE | 2 | — | — | 0.5 | [X] total | [Notes] |
| 2 | BJ-[MOD]-002 | [Module] | [Job Name] | Event | On-demand | [X] records | MEDIUM | 4 | — | +1 | 0.5 | [X] total | [Notes] |

**Column Key:**
```
Volume          : From FSD batch job spec — records per run
Complexity      : SIMPLE=2 / MEDIUM=4 / COMPLEX=8 PD
Framework Setup+: Only for FIRST job — setup framework infrastructure
Parallel +      : +1 to +2 PD if parallel processing required
Monitoring +    : +0.5 PD per job for dashboard + alert setup
```

#### 4.2 Batch Module Subtotals

| Module | Jobs | SIMPLE | MEDIUM | COMPLEX | Base PD | Adjustments | Module Total PD |
|--------|------|--------|--------|---------|---------|-------------|----------------|
| [Module] | [X] | [X] | [X] | [X] | [X] | [X] | **[X]** |
| **TOTAL** | **[X]** | **[X]** | **[X]** | **[X]** | **[X]** | **[X]** | **[X]** |

#### 4.3 Batch Additional Effort

| # | Task | PD | Justification |
|---|------|----|---------------|
| BJ-ADD-001 | Batch framework setup (one-time) | [X] | [Framework from ARCH Section 8.2] |
| BJ-ADD-002 | Job monitoring dashboard | [X] | Visual dashboard for all jobs + history |
| BJ-ADD-003 | DLQ + retry infrastructure | [X] | Dead letter handling, alert routing |
| BJ-ADD-004 | Job dependency orchestration | [X] | DAG setup for dependent jobs |
| BJ-ADD-005 | Batch testing framework | [X] | Test harness for job runs |
| **SUBTOTAL** | | **[X]** | |

#### 4.4 Batch Total Summary

```
┌─────────────────────────────────────────────────────┐
│            BATCH JOBS ESTIMATION SUMMARY             │
├─────────────────────────────────────────────────────┤
│ Job Matrix Total (Section 4.1-4.2)                  │
│   SIMPLE jobs  : [X] × 2 PD  =  [X] PD             │
│   MEDIUM jobs  : [X] × 4 PD  =  [X] PD             │
│   COMPLEX jobs : [X] × 8 PD  =  [X] PD             │
│   Adjustments  :             =  [X] PD             │
│   Jobs Subtotal              = [XX] PD             │
│                                                     │
│ Additional Effort (Section 4.3) = [XX] PD          │
│                                                     │
│ BATCH TOTAL                  = [XX] PD             │
│ As % of total project        =  [X]%               │
└─────────────────────────────────────────────────────┘
```

---

### SECTION 5: INTEGRATION ESTIMATION MATRIX

#### 5.1 Per-Integration Estimation Matrix

| # | FSD Code | Module | System Name | Direction | Protocol | Sandbox? | Complexity | Base PD | Auth Setup +? | Data Mapping +? | Error Handling +? | Testing +? | Total PD | Notes |
|---|----------|--------|------------|-----------|----------|---------|------------|---------|--------------|----------------|------------------|-----------|---------|-------|
| 1 | INT-[MOD]-001 | [Module] | [System] | Outbound | REST | Yes | SIMPLE | 3 | — | +0.5 | +0.5 | +0.5 | 4.5 | [Notes] |
| 2 | INT-[MOD]-002 | [Module] | [System] | Inbound | Webhook | No | MEDIUM | 6 | +1 | +1 | +1 | +1 | 10 | No sandbox adds risk |

**Column Key:**
```
Sandbox?        : No sandbox = add 2 PD minimum (testing is harder)
Complexity      : SIMPLE=3 / MEDIUM=6 / COMPLEX=12 PD
Auth Setup +    : +1 PD for OAuth2 / +2 PD for certificate-based / — for API key
Data Mapping +  : +0.5-2 PD depending on transformation complexity
Error Handling +: +0.5-1 PD for retry, circuit breaker, DLQ setup
Testing +       : +0.5-1 PD for integration tests + mock setup
```

#### 5.2 Message Queue Setup (if applicable)

| # | Task | PD | Justification |
|---|------|----|---------------|
| MQ-001 | Message broker setup | [X] | [Technology from ARCH Section 5] — cluster config |
| MQ-002 | Topic/queue creation | [X] | [X] topics from ARCH Section 5.4 |
| MQ-003 | DLQ setup | [X] | Per queue DLQ + monitoring |
| MQ-004 | Schema registry | [X] | If Avro/Protobuf (from ARCH) |
| MQ-005 | Consumer implementation | [X] | Per consumer from ARCH Section 5.3 |
| MQ-006 | Producer implementation | [X] | Per producer from ARCH Section 5.3 |
| **SUBTOTAL** | | **[X]** | |

#### 5.3 Integration Total Summary

```
┌─────────────────────────────────────────────────────┐
│           INTEGRATION ESTIMATION SUMMARY             │
├─────────────────────────────────────────────────────┤
│ Integration Matrix Total (Section 5.1)              │
│   SIMPLE        : [X] × 3 PD  =  [X] PD            │
│   MEDIUM        : [X] × 6 PD  =  [X] PD            │
│   COMPLEX       : [X] × 12 PD =  [X] PD            │
│   Adjustments   :             =  [X] PD            │
│   Int. Subtotal               = [XX] PD            │
│                                                     │
│ Message Queue Setup (Section 5.2) = [XX] PD        │
│                                                     │
│ INTEGRATION TOTAL             = [XX] PD            │
│ As % of total project         =  [X]%              │
└─────────────────────────────────────────────────────┘
```

---

### SECTION 6: DEVOPS & INFRASTRUCTURE ESTIMATION MATRIX

#### 6.1 Infrastructure Setup Matrix

| # | Task | Environment | Complexity | PD | Justification |
|---|------|-------------|------------|----|----|
| DO-001 | Cloud account + network setup (VPC/VNET) | All | MEDIUM | [X] | Subnets, security groups, peering |
| DO-002 | Development environment | Dev | LOW | [X] | Scaled-down infra |
| DO-003 | QA environment | QA | LOW | [X] | Mirror of dev |
| DO-004 | Staging environment | Staging | MEDIUM | [X] | Near-prod config |
| DO-005 | Production environment | Prod | HIGH | [X] | Full HA setup |
| DO-006 | Database provisioning (all envs) | All | MEDIUM | [X] | [X] environments × setup |
| DO-007 | Redis/Cache setup | All | LOW | [X] | Per environment |
| DO-008 | Message queue/broker setup | All | MEDIUM | [X] | From ARCH Section 5 |
| DO-009 | File storage setup | All | LOW | [X] | Buckets + policies + CDN |
| DO-010 | API Gateway setup | All | MEDIUM | [X] | From ARCH Section 4.2 |
| DO-011 | Load balancer setup | Staging+Prod | MEDIUM | [X] | Health checks + rules |
| DO-012 | Auto-scaling configuration | Staging+Prod | MEDIUM | [X] | From ARCH Section 9.5 |
| DO-013 | SSL certificates | All | LOW | [X] | Cert provisioning + renewal |
| DO-014 | Container registry setup | All | LOW | [X] | Image repo + policies |
| DO-015 | Kubernetes cluster (if applicable) | All | HIGH | [X] | From ARCH Section 9.4 |
| DO-016 | Secret management setup | All | MEDIUM | [X] | Vault / Key Vault / KMS |
| DO-017 | Backup configuration | Prod | MEDIUM | [X] | DB + file backup + testing |
| DO-018 | Disaster recovery setup | Prod | HIGH | [X] | From BRD RTO/RPO |

#### 6.2 CI/CD Pipeline Matrix

| # | Task | PD | Justification |
|---|------|----|---------------|
| CICD-001 | Source control setup + branching strategy | [X] | Git setup, branch rules, PR templates |
| CICD-002 | Build pipeline (CI) | [X] | Compile + unit tests + SAST |
| CICD-003 | Test pipeline | [X] | Integration tests + coverage |
| CICD-004 | Docker image build + push pipeline | [X] | Per service/app |
| CICD-005 | Deploy to Dev (auto) | [X] | On merge to develop |
| CICD-006 | Deploy to QA (auto) | [X] | On merge to main |
| CICD-007 | Deploy to Staging (manual gate) | [X] | Approval + deploy |
| CICD-008 | Deploy to Production (manual gate) | [X] | Approval + change record + deploy |
| CICD-009 | Rollback pipeline | [X] | One-click rollback to previous |
| CICD-010 | Infrastructure as Code | [X] | Terraform / Bicep for all resources |

#### 6.3 Monitoring & Observability Matrix

| # | Task | PD | Justification |
|---|------|----|---------------|
| OBS-001 | Centralised logging setup | [X] | Log aggregation + search |
| OBS-002 | Application metrics + dashboards | [X] | Request rate, error rate, latency |
| OBS-003 | Infrastructure metrics | [X] | CPU, memory, disk, network |
| OBS-004 | Distributed tracing setup | [X] | TraceId propagation across services |
| OBS-005 | Alerting rules configuration | [X] | From ARCH Section 12.2 — [X] alerts |
| OBS-006 | Health check endpoints | [X] | Liveness + readiness per service |
| OBS-007 | Batch job monitoring dashboard | [X] | Job status + history + SLA tracking |
| OBS-008 | Security monitoring | [X] | Failed logins, anomaly detection |

#### 6.4 DevOps Total Summary

```
┌─────────────────────────────────────────────────────┐
│            DEVOPS ESTIMATION SUMMARY                │
├─────────────────────────────────────────────────────┤
│ Infrastructure Setup (Section 6.1) = [XX] PD       │
│ CI/CD Pipelines (Section 6.2)      = [XX] PD       │
│ Monitoring & Observability (6.3)   = [XX] PD       │
│                                                     │
│ DEVOPS TOTAL                       = [XX] PD       │
│ As % of total project              =  [X]%         │
└─────────────────────────────────────────────────────┘
```

---

### SECTION 7: QA & TESTING ESTIMATION MATRIX

**QA = 30% of total development effort (Frontend + Backend + Batch + Integration)**

#### 7.1 QA Activity Matrix

| # | Activity | Module/Scope | PD | Justification |
|---|----------|-------------|----|----|
| QA-001 | Test strategy + test plan creation | All modules | [X] | One-time per module |
| QA-002 | Test case writing — Frontend | [X] screens | [X] | Avg [X] test cases/screen × [X] min |
| QA-003 | Test case writing — Backend APIs | [X] operations | [X] | Avg [X] test cases/API × [X] min |
| QA-004 | Test case writing — Batch Jobs | [X] jobs | [X] | Avg [X] test cases/job |
| QA-005 | Test case writing — Integrations | [X] integrations | [X] | Happy path + error scenarios |
| QA-006 | Functional testing — [Module 1] | [Module] | [X] | [X] screens × [X] days |
| QA-007 | Functional testing — [Module 2] | [Module] | [X] | |
| QA-008 | Functional testing — [Module N] | [Module] | [X] | |
| QA-009 | API testing (Postman/Newman) | All APIs | [X] | [X] operations × avg effort |
| QA-010 | Integration testing | [X] integrations | [X] | End-to-end integration flows |
| QA-011 | Batch job testing | [X] jobs | [X] | Normal + failure + retry scenarios |
| QA-012 | Regression testing (Round 1) | Full system | [X] | After bug fixes from round 1 |
| QA-013 | Regression testing (Round 2) | Full system | [X] | After bug fixes from round 2 |
| QA-014 | Performance / load testing | Critical paths | [X] | From BRD NFR targets |
| QA-015 | Security testing | Auth + APIs | [X] | OWASP top 10, pen test support |
| QA-016 | Accessibility testing | All screens | [X] | WCAG [level] from BRD |
| QA-017 | Cross-browser testing | All screens | [X] | [X] browsers from FSD Section 7.5 |
| QA-018 | Mobile / responsive testing | All screens | [X] | From FSD mobile specs |
| QA-019 | UAT support | All modules | [X] | QA available during customer UAT |
| QA-020 | Bug triage and verification | All | [X] | Verify dev fixes, update test results |

#### 7.2 QA Module Breakdown

| Module | Screens | APIs | Jobs | Test Cases (est.) | QA PD |
|--------|---------|------|------|------------------|-------|
| Auth | [X] | [X] | [X] | [X] | [X] |
| [Module 2] | [X] | [X] | [X] | [X] | [X] |
| [Module N] | [X] | [X] | [X] | [X] | [X] |
| **TOTAL** | **[X]** | **[X]** | **[X]** | **[X]** | **[X]** |

#### 7.3 QA Benchmark Validation

```
Dev total (Frontend + Backend + Batch + Integration) = [XX] PD
QA benchmark (30% of dev total)                     = [XX] PD
QA matrix total (Section 7.1)                       = [XX] PD

✅ QA matrix aligns with benchmark   (within ±5%)
⚠️ QA matrix differs from benchmark — reason: [explain]
```

#### 7.4 QA Total Summary

```
┌─────────────────────────────────────────────────────┐
│              QA ESTIMATION SUMMARY                  │
├─────────────────────────────────────────────────────┤
│ Test planning + case writing = [XX] PD             │
│ Functional testing          = [XX] PD             │
│ Non-functional testing      = [XX] PD             │
│ UAT support + bug verify    = [XX] PD             │
│ Regression rounds           = [XX] PD             │
│                                                     │
│ QA TOTAL                    = [XX] PD             │
│ As % of total project       =  [X]%               │
└─────────────────────────────────────────────────────┘
```

---

### SECTION 8: PROJECT MANAGEMENT ESTIMATION MATRIX

| # | Activity | PD | Justification |
|---|----------|----|----|
| PM-001 | Project kickoff + planning | [X] | Charter, plan, RACI, risk register |
| PM-002 | Sprint planning (all sprints) | [X] | [X] sprints × [X] hours per planning |
| PM-003 | Sprint reviews (all sprints) | [X] | [X] sprints × [X] hours per review |
| PM-004 | Sprint retrospectives | [X] | [X] sprints × [X] hours per retro |
| PM-005 | Daily standups | [X] | [X] sprints × 10 days × 0.25 hr |
| PM-006 | Stakeholder reporting | [X] | Weekly status × [X] weeks |
| PM-007 | Risk management | [X] | Ongoing throughout project |
| PM-008 | Change request management | [X] | Estimated [X] CRs at [X] hours each |
| PM-009 | Technical documentation | [X] | API docs, runbooks, deployment guides |
| PM-010 | User documentation / training | [X] | User manual, training materials |
| PM-011 | Deployment coordination | [X] | Release planning + go-live checklist |
| PM-012 | Post go-live support (hypercare) | [X] | [X] weeks × [X] team members |
| **TOTAL** | | **[X]** | |

---

### SECTION 9: MASTER ESTIMATION MATRIX

**THE PRIMARY ADJUSTABLE TABLE — PM uses this for scenario planning.**

#### 9.1 Summary Matrix by Module and Category

```
Instructions for PM:
- This is your master control table
- Change any PD value → adjust the total below accordingly
- Each row is independently adjustable
- Column headers are estimation categories
- Row totals = sum across all categories
- Column totals = sum across all modules
```

| Module | Frontend PD | Backend PD | Batch PD | Integration PD | QA PD | Total PD | % of Project |
|--------|------------|------------|---------|----------------|-------|---------|-------------|
| **Auth** | [X] | [X] | [X] | [X] | [X] | **[X]** | [X]% |
| **[Module 2]** | [X] | [X] | [X] | [X] | [X] | **[X]** | [X]% |
| **[Module 3]** | [X] | [X] | [X] | [X] | [X] | **[X]** | [X]% |
| **[Module N]** | [X] | [X] | [X] | [X] | [X] | **[X]** | [X]% |
| **Cross-cutting** | [X] | [X] | — | — | [X] | **[X]** | [X]% |
| **Subtotal (Dev)** | **[X]** | **[X]** | **[X]** | **[X]** | **[X]** | **[X]** | 100% |

#### 9.2 Summary Matrix by Estimation Category

| Category | PD | % of Total | Screens/Items | Avg PD/Item |
|----------|----|-----------|--------------:|------------|
| **Frontend — Screens** | [X] | [X]% | [X] screens | [X] PD/screen |
| **Frontend — Additional** | [X] | [X]% | — | — |
| **Backend — Operations** | [X] | [X]% | [X] operations | [X] PD/op |
| **Backend — Additional** | [X] | [X]% | — | — |
| **Batch Jobs** | [X] | [X]% | [X] jobs | [X] PD/job |
| **Integrations** | [X] | [X]% | [X] integrations | [X] PD/int |
| **Message Queue** | [X] | [X]% | [X] topics | — |
| **DevOps — Infrastructure** | [X] | [X]% | [X] envs | — |
| **DevOps — CI/CD** | [X] | [X]% | [X] pipelines | — |
| **DevOps — Monitoring** | [X] | [X]% | — | — |
| **QA & Testing** | [X] | [X]% | [X] test cases | — |
| **Project Management** | [X] | [X]% | [X] sprints | — |
| **BASE TOTAL** | **[X]** | **100%** | | |

#### 9.3 Complexity Distribution Summary

**Frontend Screen Distribution:**
| Complexity | Count | PD Each | Total PD | % of Frontend |
|------------|-------|---------|---------|--------------|
| LOW | [X] | 2 | [X] | [X]% |
| MEDIUM | [X] | 4 | [X] | [X]% |
| HIGH | [X] | 8 | [X] | [X]% |
| Modals | [X] | 1 | [X] | [X]% |
| Additional | — | — | [X] | [X]% |
| **TOTAL** | **[X]** | | **[X]** | 100% |

**Backend Operation Distribution:**
| Complexity | Count | PD Each | Total PD | % of Backend |
|------------|-------|---------|---------|-------------|
| SIMPLE | [X] | 1 | [X] | [X]% |
| MEDIUM | [X] | 2 | [X] | [X]% |
| COMPLEX | [X] | 4 | [X] | [X]% |
| Arch. Adj. | — | — | [X] | [X]% |
| Additional | — | — | [X] | [X]% |
| **TOTAL** | **[X]** | | **[X]** | 100% |

---

### SECTION 10: RISK BUFFER & FINAL ESTIMATE

#### 10.1 Architecture-Driven Effort Adjusters

From ARCH Section 14.2 — additional effort for architectural complexity:

| Decision | Adopted | Additional PD | Calculation | Applied To |
|----------|---------|--------------|-------------|-----------|
| DDD adopted | Yes/No | [X] | Backend PD × 15% | Backend |
| CQRS adopted | Yes/No | [X] | Backend PD × 12% | Backend |
| Event Sourcing | Yes/No | [X] | Backend PD × 25% | Backend |
| Microservices | Yes/No | [X] | DevOps PD × 30% | DevOps |
| Multi-tenancy | Yes/No | [X] | (FE+BE) PD × 12% | FE + BE |
| High compliance | Yes/No | [X] | Total Dev × 15% | All dev |
| New technology | Yes/No | [X] | [Specific team] × 20% | Affected team |
| **TOTAL ADJUSTERS** | | **[X] PD** | | |

#### 10.2 Risk Assessment Matrix

| # | Risk | Probability | Effort Impact | Buffer PD | Mitigation |
|---|------|-------------|--------------|-----------|-----------|
| R-001 | Unresolved FSD open questions ([X] items) | High | High | [X] | Resolve all OQs before sprint 1 |
| R-002 | Integration API differs from documentation | Medium | Medium | [X] | Build integration POC in sprint 1 |
| R-003 | Performance targets missed first pass | Medium | High | [X] | Performance test from sprint 3 |
| R-004 | Third-party sandbox not available | Medium | Medium | [X] | Chase credentials week 1 |
| R-005 | Scope creep (undiscovered requirements) | High | High | [X] | Strict change request process |
| R-006 | Key team member unavailable | Low | High | [X] | Cross-train, document everything |
| R-007 | Compliance requirement changes | Low | High | [X] | Legal review in sprint 1 |
| R-008 | New technology learning curve | [Prob] | [Impact] | [X] | Tech spike in sprint 1 |
| **TOTAL RISK BUFFER** | | | | **[X] PD** | |

#### 10.3 Risk Buffer Selection

```
Risk Buffer Options:
  LOW    (+15%): Few unknowns, team knows tech, scope clear, no complex integrations
  MEDIUM (+25%): Some open questions, some new tech, moderate integration complexity
  HIGH   (+40%): Many unknowns, new tech stack, complex integrations, compliance risk

Risk Assessment for this project:
  Open questions remaining : [X] items → [Low/Medium/High] risk
  New technology elements  : [X] new tools → [Low/Medium/High] risk
  Integration complexity   : [X] complex integrations → [Low/Medium/High] risk
  Compliance requirements  : [Level] → [Low/Medium/High] risk
  Team familiarity         : [Level] → [Low/Medium/High] risk

RECOMMENDED BUFFER : [X]%
REASON             : [Specific justification per risk factor above]
```

#### 10.4 Three-Point Estimate for High-Risk Items

For any item with HIGH complexity or HIGH risk, apply PERT:
`E = (Optimistic + 4 × Most Likely + Pessimistic) / 6`

| Item | Optimistic | Most Likely | Pessimistic | PERT Result | Std Dev |
|------|-----------|-------------|-------------|-------------|---------|
| [High risk item 1] | [O] PD | [M] PD | [P] PD | [E] PD | [(P-O)/6] |
| [High risk item 2] | [O] PD | [M] PD | [P] PD | [E] PD | [(P-O)/6] |

---

### SECTION 11: FINAL CONSOLIDATED ESTIMATE

#### 11.1 Final Effort Summary

```
┌─────────────────────────────────────────────────────────────────┐
│                  FINAL EFFORT ESTIMATE                          │
│                  [System Name] — v[X.X]                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  DEVELOPMENT EFFORT                                             │
│  ─────────────────────────────────────────────                 │
│  Frontend Development          [XXX] PD   ([XX]%)             │
│    ├── Screen implementation   [XXX] PD                        │
│    └── Additional/setup        [XXX] PD                        │
│                                                                 │
│  Backend Development           [XXX] PD   ([XX]%)             │
│    ├── Operations/APIs         [XXX] PD                        │
│    └── Additional/setup        [XXX] PD                        │
│                                                                 │
│  Batch Jobs                    [XXX] PD   ([XX]%)             │
│  Integrations                  [XXX] PD   ([XX]%)             │
│  ─────────────────────────────────────────────                 │
│  Dev Subtotal                  [XXX] PD                        │
│                                                                 │
│  INFRASTRUCTURE & OPERATIONS                                    │
│  ─────────────────────────────────────────────                 │
│  DevOps / Infrastructure       [XXX] PD   ([XX]%)             │
│                                                                 │
│  QUALITY ASSURANCE                                              │
│  ─────────────────────────────────────────────                 │
│  QA & Testing                  [XXX] PD   ([XX]%)             │
│                                                                 │
│  PROJECT MANAGEMENT                                             │
│  ─────────────────────────────────────────────                 │
│  PM / Coordination             [XXX] PD   ([XX]%)             │
│                                                                 │
│  ══════════════════════════════════════════════                │
│  BASE TOTAL                    [XXX] PD                        │
│                                                                 │
│  ADJUSTMENTS                                                    │
│  ─────────────────────────────────────────────                 │
│  Architecture complexity adj.  [XXX] PD                        │
│  Risk buffer ([X]%)            [XXX] PD                        │
│  ──────────────────────────────────────────                    │
│  Total Adjustments             [XXX] PD                        │
│                                                                 │
│  ══════════════════════════════════════════════                │
│  FINAL TOTAL ESTIMATE          [XXX] PD                        │
│                                                                 │
│  Confidence level: [HIGH 85-90% / MEDIUM 65-75% / LOW 45-60%]│
└─────────────────────────────────────────────────────────────────┘
```

#### 11.2 Per-Module Final Estimate Table

**This is the PM's adjustment table — change any PD, recalculate totals.**

| # | Module | FE PD | BE PD | Batch PD | Int PD | QA PD | PM PD | Module Total | Priority | Phase |
|---|--------|-------|-------|---------|--------|-------|-------|-------------|---------|-------|
| 1 | Auth | [X] | [X] | — | — | [X] | [X] | **[X]** | P1 | Phase 1 |
| 2 | [Module] | [X] | [X] | [X] | [X] | [X] | [X] | **[X]** | P1 | Phase 1 |
| 3 | [Module] | [X] | [X] | [X] | — | [X] | [X] | **[X]** | P2 | Phase 2 |
| | **DevOps** | — | — | — | — | — | — | **[X]** | — | Phase 1 |
| | **Subtotal** | **[X]** | **[X]** | **[X]** | **[X]** | **[X]** | **[X]** | **[X]** | | |
| | Arch Adj. | | | | | | | **[X]** | | |
| | Risk Buffer| | | | | | | **[X]** | | |
| | **GRAND TOTAL** | | | | | | | **[X] PD** | | |

#### 11.3 Scenario Planning Table

**Adjust scope to fit different budget/timeline scenarios:**

| Scenario | Scope Changes | Removed PD | Remaining PD | Timeline | Risk |
|----------|--------------|-----------|-------------|---------|------|
| **Full Scope** | All features as per FSD | — | [X] PD | [X] months | [Level] |
| **MVP (Phase 1)** | P1 modules only | -[X] PD | [X] PD | [X] months | [Level] |
| **Reduced** | P1 + P2, exclude [X] | -[X] PD | [X] PD | [X] months | [Level] |
| **Minimum** | Critical path only | -[X] PD | [X] PD | [X] months | [Level] |

---

### SECTION 12: TIMELINE & SPRINT PLAN

#### 12.1 Team Capacity Calculation

```
Team composition: [from Section 0 parameters]
Working days per sprint (2 weeks): 10 days
Total team PD per sprint:
  Tech Lead         : [X] persons × 10 days × [X]% on development = [X] PD
  Backend Devs      : [X] persons × 10 days × [X]% on development = [X] PD
  Frontend Devs     : [X] persons × 10 days × [X]% on development = [X] PD
  DevOps            : [X] persons × 10 days × [X]% on development = [X] PD
  QA Engineers      : [X] persons × 10 days × [X]% on development = [X] PD
  ──────────────────────────────────────────────────────────────────
  TOTAL TEAM PD per sprint                                        = [X] PD
  Sprint overhead (ceremonies, reviews, standups)                 = [X] PD
  NET PRODUCTIVE PD per sprint                                    = [X] PD

Number of sprints needed: [Total PD] ÷ [Net PD/sprint] = [X] sprints
Total duration          : [X] sprints × 2 weeks         = [X] weeks = [X] months
```

#### 12.2 Phase Breakdown

| Phase | Scope | Sprints | Duration | PD | Key Deliverable |
|-------|-------|---------|----------|----|----------------|
| Phase 0 | Setup + Architecture spike | [X] | [X] weeks | [X] | Environments + tech spike |
| Phase 1 | Foundation + core modules | [X] | [X] weeks | [X] | Auth + [core modules] |
| Phase 2 | Business modules | [X] | [X] weeks | [X] | [Business feature modules] |
| Phase 3 | Integrations + batch | [X] | [X] weeks | [X] | All integrations + jobs |
| Phase 4 | QA + hardening | [X] | [X] weeks | [X] | Full regression + performance |
| Phase 5 | UAT + go-live | [X] | [X] weeks | [X] | Customer UAT + deployment |
| Phase 6 | Hypercare | [X] | [X] weeks | [X] | Post go-live support |
| **TOTAL** | | **[X]** | **[X] weeks** | **[X]** | |

#### 12.3 Sprint Plan

| Sprint | Weeks | Focus | FE Deliverables | BE Deliverables | DevOps | QA |
|--------|-------|-------|----------------|----------------|--------|----|
| Sprint 0 | [W1-2] | Setup | Project setup | Project setup | All environments | Test plan |
| Sprint 1 | [W3-4] | Foundation | Auth screens | Auth APIs + DB schema | CI/CD | Auth test cases |
| Sprint 2 | [W5-6] | [Module] | [Screens] | [APIs] | Monitoring | [Module] test cases |
| Sprint N | [WX-Y] | [Focus] | [Screens] | [APIs] | — | [Test cases] |
| Sprint N+1 | [WX-Y] | QA/Fix | Bug fixes | Bug fixes | — | Regression round 1 |
| Sprint N+2 | [WX-Y] | UAT | UAT fixes | UAT fixes | Prod deploy | UAT support |

#### 12.4 Timeline Summary

```
┌─────────────────────────────────────────────────────┐
│                TIMELINE SUMMARY                     │
├─────────────────────────────────────────────────────┤
│ Project Start     : [Date / TBD]                    │
│ Development End   : [Date / TBD]                    │
│ QA Completion     : [Date / TBD]                    │
│ UAT Start         : [Date / TBD]                    │
│ Go-Live Target    : [Date / TBD]                    │
│ Hypercare End     : [Date / TBD]                    │
│                                                     │
│ Total Duration    : [X] weeks / [X] months          │
│ Total Sprints     : [X] sprints                     │
│ Total Effort      : [X] PD                          │
└─────────────────────────────────────────────────────┘
```

---

### SECTION 13: TEAM ALLOCATION MATRIX

| Role | Count | Sprint 0 | Phase 1 | Phase 2 | Phase 3 | Phase 4 | Phase 5 | Total PD |
|------|-------|---------|---------|---------|---------|---------|---------|---------|
| Tech Lead | [X] | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | [X] |
| Backend Dev | [X] | ✅ | ✅ | ✅ | ✅ | 50% | — | [X] |
| Frontend Dev | [X] | 50% | ✅ | ✅ | ✅ | 50% | — | [X] |
| DevOps | [X] | ✅ | ✅ | 50% | 50% | ✅ | ✅ | [X] |
| QA Engineer | [X] | 50% | ✅ | ✅ | ✅ | ✅ | ✅ | [X] |
| PM | [X] | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | [X] |
| **Total PD** | | **[X]** | **[X]** | **[X]** | **[X]** | **[X]** | **[X]** | **[X]** |

---

### SECTION 14: ESTIMATION CONFIDENCE & SIGN-OFF

#### 14.1 Confidence Statement

```
CONFIDENCE LEVEL: [HIGH / MEDIUM / LOW]

HIGH (85-90% accurate):
  ✅ BRD fully complete, zero open items
  ✅ FSD fully complete, all modules specified
  ✅ Architecture finalized
  ✅ All FSD open questions resolved
  ✅ Team composition confirmed
  ✅ Third-party API docs reviewed

MEDIUM (65-75% accurate):
  ⚠️ [X] FSD open questions unresolved
  ⚠️ Some integrations not fully documented
  ⚠️ New technology element in stack

LOW (45-60% accurate):
  ❌ Multiple open questions
  ❌ Architecture not final
  ❌ Third-party APIs not reviewed

This estimation confidence: [LEVEL]

Factors limiting confidence:
  1. [Factor 1 — specific]
  2. [Factor 2 — specific]

To increase confidence to HIGH:
  1. [Action needed]
  2. [Action needed]
```

#### 14.2 PM Adjustment Guide

```
HOW TO ADJUST THIS ESTIMATE:

To REDUCE scope:
  1. Go to Section 11.2 (Per-Module Final Table)
  2. Set a module's row to 0 (remove from scope)
  3. Recalculate column totals
  4. Recalculate Section 11.1 final total
  5. Recalculate risk buffer on new total
  6. Document reason in revision history

To CHANGE complexity:
  1. Go to Section 2.1 (Frontend Matrix) or Section 3.1 (Backend Matrix)
  2. Change complexity rating for the item
  3. Update Total PD for that row
  4. Update Module Subtotal (Section 2.2 or 3.2)
  5. Update Section 11.2 module total
  6. Recalculate grand total

To CHANGE team size:
  1. Go to Section 12.1 (Team Capacity)
  2. Update team composition
  3. Recalculate Net PD per sprint
  4. Recalculate number of sprints needed
  5. Update Section 12.4 (Timeline)

To ADD a feature:
  1. Identify FSD screen/API/job code
  2. Add row to appropriate section (2.1, 3.1, 4.1, or 5.1)
  3. Update module subtotal
  4. Update Section 11.2
  5. Recalculate grand total
```

#### 14.3 Sign-Off

| Role | Name | Date | Status |
|------|------|------|--------|
| Estimation Lead | | | Prepared |
| Tech Lead / Architect | | | Reviewed |
| Product Manager | | | Approved |
| Project Sponsor | | | Approved |
| Customer Representative | | | Accepted |

---

## Estimation Completion Checklist

```
□ All parameters set (team, hours/day, complexity benchmarks)
□ Scope confirmed — in-scope and out-of-scope listed
□ All assumptions documented (EA-001 through EA-0XX)
□ EVERY FSD screen has a row in Section 2.1 matrix
□ EVERY FSD business logic operation in Section 3.1 matrix
□ EVERY FSD batch job in Section 4.1 matrix
□ EVERY FSD integration in Section 5.1 matrix
□ Architecture adjusters applied and justified
□ QA estimate = ~30% of dev total (validated)
□ DevOps estimate covers all environments + CI/CD + monitoring
□ PM estimate covers all sprints + overhead
□ Risk buffer selected with justification
□ Three-point estimate for HIGH risk items
□ Final table (Section 11.2) allows per-module adjustment
□ Scenario planning table shows min/max options
□ Sprint plan covers all phases
□ Team allocation matrix complete
□ Confidence level stated with specific reasons
□ PM adjustment guide included
□ Sign-off table populated
```
