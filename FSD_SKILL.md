---
name: fsd-creation
description: >
  Use this skill whenever a Product Manager or Business Analyst needs to
  create a Functional Specification Document (FSD) from a BRD (Business
  Requirements Document). Triggers include: "create FSD", "write FSD",
  "prepare functional spec", "convert BRD to FSD", "write functional
  requirements", "detail out the features", "prepare functional document",
  or any request to translate business requirements into detailed functional
  specifications that developers, architects, QA engineers, and DevOps teams
  can use directly. This skill covers every functional detail including
  screens, workflows, business rules, logical data contracts, business logic
  specifications, batch jobs, integrations, notifications, reports, data
  seeding, feature flags, and master summary tables. FSD is always
  pattern-neutral and technology-neutral — architectural patterns like
  REST, GraphQL, BFF, microservices are decided AFTER FSD by Architecture.
role: Senior Product Manager / Senior Business Analyst in a Product Company
input: A completed and reviewed BRD (Business Requirements Document)
version: 2.0 — Corrected, Enhanced, Pattern-Neutral
---

# FSD Creation Skill v2.0

## MOST CRITICAL CONCEPT — READ FIRST

### The FSD is PATTERN-NEUTRAL and TECHNOLOGY-NEUTRAL

```
FSD answers: WHAT must the system do?
Architecture answers: HOW will it be built?

FSD Says (Business Language):
  "Fetch paginated order list filtered by status"

Architecture Decides (Technical Language):
  REST: GET /api/v1/orders?status=active&page=1
  OR GraphQL: query { orders(status: ACTIVE, page: 1) }
  OR BFF: aggregates order + customer + inventory services
  OR gRPC: OrderService.ListOrders(ListOrdersRequest)
```

The FSD NEVER contains:
- HTTP methods (GET, POST, PUT, DELETE)
- REST endpoint URLs (/api/v1/resource)
- Service class names or method signatures
- Patterns: REST, GraphQL, BFF, microservices, monolith

The FSD ALWAYS contains:
- What data the screen needs to display
- What actions the user can perform
- What business rules must be enforced
- What background processing is required
- What events/notifications must be triggered

### Correct Sequence

```
BRD → FSD → Architecture → Estimation
      ↑
  You are here. Pattern-neutral. Technology-neutral.
  Architecture reads YOUR output to make its decisions.
```

### BFF Clarification

BFF (Backend for Frontend) is an Architecture decision, not an FSD decision.
FSD says: "Order Summary screen needs order details + customer info + inventory status"
Architecture decides: "We'll use a BFF to aggregate these into one call"
Same FSD → Different architectures. The FSD does not change.

---

## Overview

This skill transforms Claude into a **Senior Business Analyst and Product
Manager with 15+ years of product company experience**, writing world-class
Functional Specification Documents.

**The FSD is complete when ALL of the following are true:**
- Frontend Developer builds every screen with ZERO questions to PM
- Backend Developer designs every service with ZERO ambiguity
- QA Engineer writes 100% of test cases from this document alone
- DevOps Engineer plans all batch infrastructure from this document
- Architect makes all architectural decisions using this document
- PM is never the middleman for technical clarifications

If any team would need to ask the PM a question → FSD is incomplete.

---

## Superintelligence Principles for FSD Creation

Adapted from Karpathy's LLM excellence principles for document writing:

### PRINCIPLE 1 — READ THE ENTIRE BRD BEFORE WRITING
- Consume the COMPLETE BRD before writing a single FSD line
- Build full mental map: Domain → Modules → Features → Screens →
  Data Flows → Business Rules → Background Processing
- Identify ALL modules: explicit (stated) + implicit (implied)
- Never start writing module 1 before understanding module 10

### PRINCIPLE 2 — EXPAND, NEVER COMPRESS
- Every BRD requirement expands into multiple FSD specifications
- BRD "User can manage orders" →
  FSD: Order List + Create + Edit + View + Cancel + Approve +
       History + Search + Export + Notifications + Batch Jobs
- FSD should be 3-5× more detailed than BRD
- Never summarize where specification is needed

### PRINCIPLE 3 — SURFACE ALL IMPLICIT REQUIREMENTS
```
BRD States          → FSD Must Also Include
────────────────────────────────────────────────────────
"User login"        → Forgot password, Reset password, MFA,
                       Account lockout, Session expiry,
                       Remember me, Post-login redirect, Logout

"List screen"       → Search, Filter, Sort, Pagination,
                       Export, Empty state, Loading state,
                       Error state, Bulk actions, Row selection

"Create form"       → Validation, Cancel with confirmation,
                       Autosave, Draft mode, Success message,
                       Redirect after save, Duplicate detection

"Delete action"     → Confirmation dialog, Soft delete,
                       Cascade impact warning, Audit log, Undo

"Approval flow"     → Reject with comments, Escalation on timeout,
                       Email notifications, Status history, Recall

"Dashboard"         → Date range filter, Refresh, Loading state,
                       No-data state, Widget drill-down, Export

"File upload"       → Type validation, Size limit, Virus scan,
                       Progress bar, Preview, Failed upload retry

"Admin panel"       → User management, Role assignment,
                       Audit logs, System config, Tenant mgmt

"Search"            → Debounce, No results state, Clear,
                       Search history, Auto-suggestions

"Report"            → Filters, Export formats, Scheduled delivery

"Notification"      → In-app bell, Mark as read, Preferences

"Import data"       → Template download, Validation report,
                       Import history, Error rows download

"Workflow"          → SLA tracking, Escalation, History log
```

### PRINCIPLE 4 — NEVER GUESS, ALWAYS FLAG
```
Missing info        → [OPEN QUESTION - OQ-{Module}-{Number}: {question}]
Assumed behavior    → [ASSUMPTION - ASM-{Module}-{Number}: {statement}]
BRD conflict        → [BRD CONFLICT - Sections {X} vs {Y}: {description}]
Design decision     → [DESIGN DECISION NEEDED - {topic}: {options}]
Architecture needed → [ARCHITECTURE DEPENDENT - {what needs deciding}]
```

### PRINCIPLE 5 — THINK IN FOUR LAYERS SIMULTANEOUSLY
```
Layer 1 — SCREEN LAYER:
  What does the user see? Fields, actions, layout, states

Layer 2 — DATA CONTRACT LAYER:
  What data must flow in and out? (Pattern-neutral)

Layer 3 — BUSINESS LOGIC LAYER:
  What rules, validations, calculations must execute?

Layer 4 — BACKGROUND PROCESSING LAYER:
  What runs asynchronously, scheduled, or event-driven?
```

### PRINCIPLE 6 — EDGE CASES ARE CORE REQUIREMENTS
- Every happy path has minimum 3 exception paths
- Always specify: no data, max data, network failure, timeout,
  concurrent edit, permission change mid-session, expired token,
  duplicate submission, partial failure
- These are specification requirements, not optional additions

### PRINCIPLE 7 — WRITE IN BUSINESS LANGUAGE ONLY
```
❌ WRONG (technology language — belongs in Architecture):
   "Call GET /api/v1/orders with Bearer token, returns 200 JSON"
   "OrderService.getOrders() queries orders table with tenant filter"

✅ CORRECT (business language — belongs in FSD):
   "Fetch the list of orders matching the selected filters,
    showing only orders belonging to the current user's tenant,
    paginated with the user's preferred page size"
```

### PRINCIPLE 8 — GOAL-DRIVEN COMPLETENESS TEST
Before moving to next module, verify:
```
✅ Developer builds this with ZERO PM questions?
✅ QA writes every test case from this spec alone?
✅ Architect estimates effort to ±10% accuracy?
✅ DevOps plans batch infrastructure from this alone?
```
If any is NO → module spec is incomplete. Keep writing.

---

## Pre-Writing Checklist

Complete ALL before writing any FSD section:

```
□ Read entire BRD completely
□ List ALL explicit modules (stated in BRD scope)
□ List ALL implicit modules (implied but not stated)
□ List ALL user roles and data scopes
□ List ALL external integrations
□ List ALL batch/scheduled operations implied
□ Identify business domain → apply domain intelligence
□ Note compliance requirements → impacts audit, security
□ Map ALL user journeys end-to-end
□ Identify ALL state machines (entities with lifecycle)
□ Identify ALL approval workflows
□ Check multi-tenancy impact on every module
□ Check mobile vs desktop behavioral differences
□ Check multi-language applicability
□ Note any existing systems being replaced
```

---

## FSD Document Structure

Generate ALL sections. If not applicable: state why explicitly.

---

### SECTION 0: DOCUMENT CONTROL

```
Document Title    : Functional Specification Document — [System Name]
FSD Version       : v1.0
Reference BRD     : BRD-[Code]-[Version]
Prepared By       : [Name / Role]
Date Created      : [Date]
Last Updated      : [Date]
Status            : Draft / Under Review / Approved
FSD Reference ID  : FSD-[ProjectCode]-001

IMPORTANT NOTE TO ALL READERS:
This FSD is pattern-neutral and technology-neutral.
API patterns (REST/GraphQL/BFF/gRPC), service boundaries,
and deployment architecture are decided in the Architecture
Document, not here. This document specifies WHAT the system
must do. HOW it is built is an Architecture decision.
```

**Revision History**
| Version | Date | Author | Section | Reason |
|---------|------|--------|---------|--------|

**Audience Guide**
| Team | Key Sections | What They Use It For |
|------|-------------|---------------------|
| Frontend Dev | 3, 4.2, 4.3, 4.5 | Screen specs, data contracts |
| Backend Dev | 4.4, 4.6, 4.7 | Business logic, operations |
| QA Engineers | 4.2, 4.3, 4.4, 4.9 | Test scenario derivation |
| DevOps | 4.7, 5.4 | Batch job infrastructure planning |
| Architects | Section 5 Master Tables | Architecture input |
| PM | All | Scope control and sign-off |

---

### SECTION 1: SYSTEM OVERVIEW

**1.1 System Description**
Plain-language description. No technical jargon.
What does this system do? Who uses it? What problem does it solve?

**1.2 System Context**
```
Feeds data INTO this system   : [External sources]
Receives data FROM this system: [External targets]
User types who interact       : [All user groups]
Systems being replaced        : [If any — impact on data migration]
```

**1.3 Complete Module Map**
| # | Module | Description | Primary Users | Priority | Phase |
|---|--------|-------------|---------------|----------|-------|
Include ALL modules: explicit from BRD + implicit ones identified.

**1.4 BRD Constraints Affecting FSD**
| Constraint from BRD | Impact on FSD Content |
|--------------------|----------------------|
| Multi-tenant system | Every module scopes data per tenant |
| GDPR compliance | Audit trail, consent, masking in every module |
| Mobile users | Every screen spec includes mobile behavior |
| Offline support | Every screen has offline state defined |

---

### SECTION 2: USER ROLES & PERMISSIONS

**2.1 Role Definitions**
```
Role Name      : [Name]
Role Code      : ROLE-[XX]
Role Type      : Super Admin / System Admin / Business User /
                 End User / External / API Consumer / Guest
Description    : What this role represents in business terms
Real User      : Who holds this role in real life
Data Scope     : ALL data / Own data only / Dept data /
                 Tenant data / Assigned records only
Time Access    : Unrestricted / Business hours only / [Other]
Special Rules  : [Any conditional access rules]
```

**2.2 Feature Permission Matrix**
| Module | Feature | [Role 1] | [Role 2] | [Role 3] | [Role 4] |
|--------|---------|----------|----------|----------|----------|

Values: FULL / VIEW / CREATE / EDIT / NONE / COND* (*explain in notes)

**2.3 Data-Level Permission Matrix**
| Entity | [Role 1] | [Role 2] | [Role 3] | Scope Rule |
|--------|----------|----------|----------|------------|

**2.4 Special Access Rules**
- Time-based restrictions
- IP or location restrictions
- Approval required to gain access
- Temporary / delegated access
- MFA step-up for elevated operations

**2.5 Role Lifecycle**
```
Assignment   : [How roles are given: Admin / Self-register / Auto]
Change       : [Who approves changes + audit log]
Offboarding  : [What happens to data: Reassign / Archive / Retain]
Reactivation : [Can deactivated users return? Yes/No]
```

---

### SECTION 3: MASTER SCREEN INVENTORY

**Complete this section FULLY before writing any module detail.**
**Screen count + complexity is the primary input for frontend estimation.**

**3.1 Complete Screen Inventory**
| # | Code | Module | Screen Name | Type | Complexity | Roles | Has Form | Has Grid | Has Chart | Is Wizard | Is Modal |
|---|------|--------|------------|------|------------|-------|----------|----------|-----------|-----------|----------|

**Screen Types:**
```
LIST_GRID     — Search, filter, sort, paginate tabular data
FORM          — Create or Edit with fields and validations
DETAIL_VIEW   — Read-only record display
DASHBOARD     — KPIs, charts, widgets, metrics
WIZARD        — Multi-step process with progress
MODAL_POPUP   — Overlay screen (count each modal separately)
REPORT        — Data grid for export/print
SETTINGS      — Configuration screens
AUTH          — Login, Forgot Password, MFA, Reset Password
PROFILE       — User profile management
NOTIFICATION  — Notification center / inbox
ONBOARDING    — First-time user / new tenant setup
```

**Complexity:**
```
LOW    (2-3 dev days): Read-only, <5 fields, no rules, 1 data source
MEDIUM (4-6 dev days): Form 6-15 fields, list with filters, 2-4 operations
HIGH   (8-15 dev days): 15+ fields, wizard, dashboard, real-time, drag-drop
```

**3.2 Screen Count Summary**
| Metric | Count |
|--------|-------|
| Total Screens | |
| LOW Complexity | |
| MEDIUM Complexity | |
| HIGH Complexity | |
| Total Forms | |
| Total List/Grid | |
| Total Dashboards | |
| Total Wizards | |
| Total Modals | |
| Total Auth Screens | |

**3.3 Screen Navigation Map**
Full sitemap hierarchy showing parent-child relationships of all screens.

---

### SECTION 4: MODULE SPECIFICATIONS

**Repeat the complete block below for EVERY module.**
Never abbreviate. Never say "similar to above."
Each module must be 100% self-contained.

---

```
════════════════════════════════════════════
MODULE: [MODULE NAME]
Code: MOD-[XX] | Phase: [1/2/3] | Priority: [P1/P2/P3]
════════════════════════════════════════════
```

#### 4.1 MODULE OVERVIEW

```
Purpose         : What this module does and why
Business Value  : What problem it solves
Primary Users   : Roles who actively use this module
Secondary Users : Roles with read-only or indirect access
Depends On      : Other modules that must exist first
Used By         : Other modules that depend on this
Multi-tenant    : How tenancy scopes this module's data
```

**Entity State Machine**
For every entity with a lifecycle:
```
Valid States    : [List all]
Initial State   : [On creation]
Terminal States : [No further transitions possible]

State Transitions:
| From State | Event/Action | To State | Who Triggers | Notification | Audit Log |
|-----------|-------------|----------|-------------|-------------|----------|

Blocked Transitions:
| From State | Blocked Action | Reason |
|-----------|---------------|--------|
```

---

#### 4.2 SCREENS & FEATURES

For EVERY screen in this module:

```
────────────────────────────────────────
SCREEN: [Screen Name]
Code: SCR-[ModCode]-[Number]
────────────────────────────────────────
Type           : [From Section 3 types]
Complexity     : LOW / MEDIUM / HIGH
Navigation     : Home > [Module] > [Screen]
Parent Screen  : [What navigates here]
Child Screens  : [What this navigates to]
Accessible By  : [Roles]
Browser Title  : [e.g., "Orders | SystemName" or "#ORD-001 | SystemName"]
```

**USER STORY:**
```
AS A [role]
I WANT TO [action]
SO THAT [business value]

Acceptance Criteria:
GIVEN [precondition]
WHEN [user performs action]
THEN [system response]
AND [additional outcome]
```

**SCREEN LAYOUT (ASCII Sketch):**
```
Provide an ASCII layout for every non-trivial screen.
Gives developers and architects instant visual context.

Example — List Screen:
┌────────────────────────────────────────────────────┐
│ 📋 Orders                        [+ New Order]     │
├────────────────────────────────────────────────────┤
│ [🔍 Search...]  [Status ▼]  [Date ▼]  [Reset]     │
├────────────────────────────────────────────────────┤
│ ☐ │ # │ Customer │ Status  │ Amount │ Date   │ ⋮  │
│───┼───┼──────────┼─────────┼────────┼────────┼────│
│ ☐ │001│ John Doe │ Active  │ $500   │ Jan 1  │ ⋮  │
├────────────────────────────────────────────────────┤
│ Showing 1-20 of 150      [< 1 2 3 >]    [Export]  │
└────────────────────────────────────────────────────┘

Example — Form Screen:
┌────────────────────────────────────────────────────┐
│ ← Back     Create Order              [Save Draft]  │
├────────────────────────────────────────────────────┤
│ CUSTOMER DETAILS                                   │
│ Customer Name*  [_______________________________]  │
│ Contact Email*  [_______________________________]  │
│                                                    │
│ ORDER DETAILS                                      │
│ Order Type*     [Dropdown ▼]                       │
│ Priority        ○ Low  ● Medium  ○ High            │
│ Delivery Date*  [📅 Pick Date]                     │
├────────────────────────────────────────────────────┤
│ [Cancel]                          [Submit Order →] │
└────────────────────────────────────────────────────┘
```

**FIELDS SPECIFICATION:**

For LIST/GRID screens:
| # | Column | Data | Sortable | Filterable | Default Sort | Format | Mobile |
|---|--------|------|---------|------------|-------------|--------|--------|

Grid Behaviors:
```
Default page size   : [X] rows
Page size options   : 10 / 20 / 50 / 100
Pagination type     : Standard / Infinite scroll / Load more
Row click           : Navigate to detail / Inline expand / None
Row selection       : None / Single / Multi (bulk actions: [list])
Empty state         : [Icon + message + CTA button text]
Loading state       : Skeleton loader / Spinner
Error state         : [Message + Retry button]
No search results   : "No results for [term]" + Clear link
Filtered to zero    : "No records match filters" + Clear filters
Column resize       : Yes / No
```

For FORM screens:
| # | Field | Label | Type | Mandatory | Default | Validation | Dependency | Mobile |
|---|-------|-------|------|-----------|---------|------------|------------|--------|

Field Types: Text / Number / Email / Phone / Password / URL /
Dropdown / Multi-select / Searchable dropdown / Date / DateTime /
Time / Date Range / Toggle / Checkbox / Radio / File Upload /
Image Upload / Rich Text / Auto-complete / Lookup / Tag Input /
Hidden / Read-Only / Calculated / Disabled

Field Dependencies:
```
[Field A] SHOWN ONLY WHEN [Field B] = [Value]
[Field C] REQUIRED WHEN [Field D] is checked
[Field E] DISABLED WHEN status = 'Approved'
[Field F] = AUTO-CALCULATED: [Field G] × [Field H]
```

For DASHBOARD screens:
| # | Widget | Type | Data Needed | Filters | Drill-down | Refresh |
|---|--------|------|-------------|---------|------------|---------|

**ACTIONS:**
| # | Action | Type | Visible To | Condition | Outcome |
|---|--------|------|-----------|-----------|---------|

Types: Primary Button / Secondary Button / Danger Button /
Icon Button / Context Menu (⋮) / FAB (Mobile) / Swipe (Mobile)

**KEYBOARD SHORTCUTS (enterprise/power-user screens):**
| Shortcut | Action | Context |
|---------|--------|---------|
| Ctrl+N | New record | List screens |
| Ctrl+S | Save | Form screens |
| Escape | Close / Cancel | Modals, Wizards |
| Ctrl+F | Focus search | List screens |

**VALIDATIONS:**

Field-level:
| Field | Rule | Error Message | When |
|-------|------|---------------|------|

Form-level (cross-field rules):
| Rule | Fields | Message | Location |
|------|--------|---------|---------|

**BUSINESS RULES:**
Format: `BR-[ModCode]-[Number]: [Rule]`
| Code | Rule | Enforcement | BRD Source |
|------|------|-------------|-----------|
Enforcement: UI Only / Backend Only / Both (recommended) / DB Level

**CALCULATED FIELDS:**
| Field | Formula | Inputs | Trigger |
|-------|---------|--------|---------|

**WORKFLOWS:**

Happy Path:
```
Step 1: User [action]       → System [response]
Step 2: User [action]       → System [response]
Step N: [Success outcome + message + redirect to: screen]
```

Exception Paths (minimum 3 per feature):
```
Exception 1 — [Name]:
  Trigger        : [What causes this]
  System behavior: [What happens internally]
  User sees      : [Exact message or visual change]
  Recovery       : [What user can do next]
```

**APPROVAL WORKFLOW:**
```
Initiator  : [Role]
Trigger    : [Action that starts approval]

Step 1:
  Approver      : [Role]
  SLA           : Must act within [X hours]
  On Approve    : → [Next step or final]
  On Reject     : → Status: [X] + Notify: [who] + Resubmit: [Yes/No]
  Comment       : Required on reject / Optional / Not needed
  Escalation    : After [X hrs] → notify [Role B]
                  After [2X hrs] → auto [action]

Final:
  On Complete   : Status → [Final] + Notify [recipients]
  On Full Reject: Status → [Rejected] + Notify [initiator]
  Recall        : Before step [X] only / Any time while pending
```

**CONFIRMATION DIALOGS:**
```
Action        : [e.g., Delete Record]
Title         : "Delete this record?"
Body          : "This will permanently remove [entity] and all related
                 data ([X] related items). This cannot be undone."
Confirm Button: "Yes, Delete" (Danger style)
Cancel Button : "Keep Record"
```

**UNDO BEHAVIOR:**
```
Supported  : Yes / No
Window     : [X] seconds (shown in toast with "Undo" link)
What undone: [Exactly what reverts]
Mechanism  : [Soft-delete reversal / Status revert / etc.]
```

**CONCURRENT USER SCENARIOS:**
```
Two users edit same record:
  → [Optimistic lock warning / Conflict dialog / Last-write-wins]

Admin deactivates active user:
  → [Immediate force logout / Session expires on next action]

Record deleted while user has it open:
  → ["Record no longer exists" shown on user's next action]

Batch job modifies data user is viewing:
  → [Stale data banner / Auto-refresh / No action]
```

**ERROR MESSAGES:**
| Scenario | User Message | Recovery |
|---------|-------------|---------|

**SUCCESS MESSAGES & REDIRECTS:**
| Action | Message | Redirect To | After |
|--------|---------|-------------|-------|

**LOADING & EMPTY STATES:**
| State | User Sees | When Shown |
|-------|----------|-----------|
| Loading | Skeleton / Spinner | Immediate on data fetch |
| Empty — first time | [Art + message + CTA] | No records ever |
| Empty — filtered | "No records match filters" + Clear | After filter |
| No search results | "No results for '[term]'" + Clear | After search |
| Error | [Icon + message + Retry] | On fetch failure |

**PRINT / PDF:**
```
Supported  : Yes / No
Layout     : Portrait / Landscape
Includes   : [Fields shown]
Excludes   : [Fields hidden: e.g., action buttons]
Header     : [Logo / title / date]
Footer     : [Page number / generated timestamp]
Filename   : [Convention: ModuleName_ID_YYYYMMDD.pdf]
```

**LOCALISATION:**
```
Labels         : Translatable / English only
Dynamic content: Translatable / English only
Date format    : Locale-aware
Number format  : Locale-aware
Currency       : Locale-aware with symbol
RTL support    : Yes / No
Timezone       : User local / System timezone
```

**MOBILE BEHAVIOR:**
```
Layout change   : [How layout adapts on <768px]
Hidden columns  : [Grid columns collapsed on mobile]
Primary action  : [Bottom sheet / FAB / Swipe gesture]
Touch gestures  : Swipe to [action] / Pull to refresh
```

**FIRST-TIME / ONBOARDING STATE:**
```
Empty state CTA : [What new user sees — button/link to start]
Tooltip/guide   : Step [X] of [Y] / None
Help text       : [Contextual guidance message]
```

**FEATURE FLAGS:**
| Flag Name | Default | Who Toggles | Scope | Purpose |
|-----------|---------|-------------|-------|---------|

**TEST SCENARIO HINTS (for QA team):**
```
□ Happy path  : [Standard flow description]
□ Validation  : [Key field/rule to validate]
□ Permission  : [Role that should be denied]
□ Edge case 1 : [Boundary: minimum value / empty input]
□ Edge case 2 : [Boundary: maximum value / very long text]
□ Empty state : No records in system
□ Max load    : [X] records in grid
□ Concurrent  : Two users acting on same record simultaneously
□ Mobile      : Smallest supported screen (375px wide)
□ Performance : Load time measurement with [X] records
```

**PERFORMANCE TARGETS:**
```
Initial load  : < [X] seconds
Data fetch    : < [X] seconds for [Y] records
Search        : < [X]ms after debounce
Export (sync) : < [X] seconds for [Y] records
```

---

#### 4.3 WORKFLOWS & PROCESS FLOWS

For each business process in this module:

```
Process: [Name]
Type   : Linear / Branching / Cyclical / Approval-based
SLA    : Must complete within [X hours/days]

HAPPY PATH:
Actor      │ Action                 │ System Response
───────────┼────────────────────────┼──────────────────
[Role]     │ [Action]               │ [Response]
System     │ [Auto step]            │ [Result]
End        │ [Success state]        │

EXCEPTION 1 — [Name]:
Trigger   : [Condition]
Steps     : [How handled]
Resolution: [Outcome]
User sees : [Exact message]
```

---

#### 4.4 BUSINESS RULES

Format: `BR-[ModCode]-[Number]: [Rule]`

| Code | Business Rule | Priority | Enforcement | Source |
|------|-------------|---------|-------------|--------|

**Status Transition Rules:**
| Current Status | Allowed Next States | Blocked | Rule |
|---------------|--------------------|---------|----|

---

#### 4.5 LOGICAL DATA CONTRACTS — UI PERSPECTIVE

**What data the screen needs. Pattern-neutral. No HTTP/REST/GraphQL.**

For each data operation the UI requires:

```
CONTRACT: [Operation Name in Business Terms]
Code: DC-[ModCode]-[Number]
────────────────────────────────────────────
Triggered By    : [User action or screen event]
Used On Screen  : [Screen code SCR-MOD-XXX]
Purpose         : [Business purpose in plain English]
Accessible To   : [Roles]
Requires Auth   : Yes / No

DATA INPUT:
  For list operations:
  - Filter by    : [Each filter: field + options]
  - Search by    : [What fields are searched]
  - Sort by      : [Available sort fields + default]
  - Paginate     : Page number + page size (default [X])

  For single record:
  - Identifier   : [What uniquely identifies the record]

  For create/update:
  - Payload      : [All fields being sent + types]

DATA OUTPUT:
  For list       : [All fields shown in grid rows] + pagination info
  For detail     : [All fields shown on detail screen]
  For dashboard  : [All metrics, aggregates, time-series needed]

BUSINESS CONSTRAINTS:
  Tenant scope   : Return only current tenant's data
  Role scope     : [Any role-specific data restrictions]
  Ownership scope: [User sees only assigned/own records]
  Data masking   : [Fields masked for certain roles]

PERFORMANCE:
  Response target : < [X] milliseconds
  Expected volume : [X records typical] / [Y records maximum]
  Cacheable       : Yes (for [X] seconds) / No (always fresh)

UX BEHAVIORS:
  Loading state  : Skeleton / Spinner / Inline indicator
  Empty state    : [What user sees: message + CTA]
  Error state    : [Message + recovery action]
  Refresh needed : Yes (every [X] seconds) / No
  Real-time push : Yes (on [event]) / No

INPUT VALIDATION (client-side before sending):
  [List format validations: date format, number range, required fields]
```

**Data Contract Summary:**
| Code | Operation Name | Screen | Triggered By | Role | Cached | Real-time |
|------|---------------|--------|--------------|------|--------|-----------|

---

#### 4.6 BUSINESS LOGIC SPECIFICATION

**What business logic must execute. Pattern-neutral. No class/method names.**

For each business operation:

```
OPERATION: [Name in Business Terms]
Code: BL-[ModCode]-[Number]
────────────────────────────────────────────
Trigger Type    : User action / System event / Another operation /
                  Scheduled / External event

Triggered By    : [Specific trigger description]
Purpose         : [Business purpose — what problem this solves]

INPUT:
  [All data inputs, their types, required/optional]

BUSINESS LOGIC STEPS:
  Step 1 — VALIDATE:
    • [Rule: what is checked]
    • [Rule: what is checked]
    • On failure: [Exact error raised + to whom]

  Step 2 — AUTHORIZE:
    • Does user have permission to perform this operation?
    • Does user own or have access to this record?
    • Are any special conditions met? [List]

  Step 3 — APPLY BUSINESS RULES:
    • BR-[Code]: [Rule applied]
    • BR-[Code]: [Rule applied]

  Step 4 — CORE OPERATION:
    • [What data is read]
    • [What data is created / updated / deleted]
    • [Calculations performed]
    • Must be atomic (all succeed or all fail): Yes / No
    • [What is atomic: describe the transaction scope]

  Step 5 — SIDE EFFECTS:
    • [Related entity status changes]
    • [Other module data updates]
    • [Cache that must be invalidated]

  Step 6 — EVENTS RAISED:
    • [Event name]: triggered when [condition]
      Recipients: [Who needs to know about this event]

  Step 7 — OUTPUT:
    • [What is returned or confirmed after success]

SERVER-SIDE VALIDATIONS (must be on server — never trust client only):
| Validation | Rule | Error Message |
|-----------|------|---------------|

DATA OPERATIONS:
  Data Read         : [Entity / criteria used to select]
  Data Written      : [Entity + Create / Update / Delete]
  Atomic Scope      : [What must succeed together or fail together]
  Query Performance : [Indexed fields this operation depends on]

EVENTS PUBLISHED:
| Event Name | When Published | Who Consumes This |
|------------|---------------|-------------------|

EVENTS CONSUMED:
| Event Name | Source | What This Operation Does With It |
|------------|--------|----------------------------------|

ERROR HANDLING:
| Error Scenario | Error Type | Message | Retry? |
|---------------|------------|---------|--------|

PERFORMANCE:
  Target execution  : < [X] milliseconds
  Peak load         : [X] operations/minute
  Caching           : Yes (what, for how long) / No

AUDIT LOG:
  Required          : Yes / No
  Log entry captures: [User, timestamp, entity, old values, new values]
  Retention         : [X years]

SECURITY:
  Fields excluded from logs: [PII/sensitive fields never logged]
  Input sanitized against  : [XSS / Injection / Other]
```

**Business Logic Summary:**
| Code | Operation | Trigger | Writes To | Events Published | SLA |
|------|-----------|---------|-----------|-----------------|-----|

---

#### 4.7 BATCH JOBS

List ALL batch/scheduled operations. If none: "No batch jobs — [reason]."

```
JOB: [Job Name]
Code: BJ-[ModCode]-[Number]
────────────────────────────────────────────
PURPOSE:
  [Business problem solved in plain English]
  [What breaks for the business if this job doesn't run?]

TRIGGER:
  Type        : Scheduled / Event-based / Manual / API-triggered
  Cron        : [Expression] = [Human readable: "Daily at 2:00 AM"]
  Event       : [Event name if event-triggered]
  Manual      : [Who + how: Admin UI / CLI]
  Pre-condition: [What must be true before job runs]

INPUT:
  Source      : Database table / File (SFTP/S3) / API / Queue
  Details     : [Table name / path / operation / queue name]
  Data scope  : [Filter criteria — what subset is processed]

PROCESSING:
  Step 1: [Fetch / extract input]
  Step 2: [Validate / cleanse]
  Step 3: [Transform / process]
  Step 4: [Apply business rules]
  Step 5: [Write output]
  Step 6: [Mark processed / archive input]
  Step 7: [Generate summary + send notification]

OUTPUT:
  Target      : Database / File / Email / API / Queue / Report
  Details     : [Table name / path / recipients / queue name]
  Format      : [DB records / CSV / PDF / Email / JSON]

VOLUME:
  Typical records : [Input] → [Output]
  Peak records    : [Input] → [Output]
  SLA             : Must complete within [X hours/minutes]

FAILURE HANDLING:
  Partial failure : Rollback all / Keep processed / Skip + log failures
  Complete failure: Alert immediately + [auto-retry / manual restart]
  Data state      : [How data remains consistent on failure]

RETRY:
  Auto-retry      : Yes / No
  Max attempts    : [X]
  Interval        : Fixed [Xmin] / Exponential backoff
  Max retry window: [X hours] then give up

IDEMPOTENCY:
  Safe to re-run  : Yes / No
  Mechanism       : [Job run ID / Status flag / Dedup key]

DEPENDENCIES:
  Run after job   : [BJ-XX-XX: job name]
  Run before job  : [BJ-XX-XX: job name]
  External prereq : [Condition: e.g., "vendor file must be present"]
  Parallel safe   : Yes / No

MONITORING & ESCALATION:
  Success alert   : [Who] via [channel]
  Failure alert   : [Who] via [channel] — Priority: P1/P2/P3

  SLA Escalation:
  | Delay | Alert To | Action |
  |-------|---------|--------|
  | Not started +15min past schedule | Dev team | Investigate |
  | Not complete within SLA | Tech Lead | Intervene |
  | Not complete within 2× SLA | Management | Escalate |
  | Failed after all retries | On-call + Management | Emergency |

  Execution log   : Start / End / Records in / Out / Failed / Status
  Dashboard       : [Tool: Airflow / Admin UI / CloudWatch]

ARCHIVAL:
  Archive input   : Yes (to [location] after [X days]) / No
  Delete processed: Yes (after [X days]) / No
```

**Batch Job Summary:**
| Code | Job Name | Trigger | Frequency | SLA | Vol(Typ) | Vol(Peak) | After |
|------|---------|---------|-----------|-----|----------|-----------|-------|

**Batch Job Discovery Checklist (check against BRD):**
```
□ Data sync with external systems (ERP / CRM / Legacy)
□ Nightly/weekly report generation + email delivery
□ Reminder notifications (due dates, inactivity, expiry)
□ Invoice / billing / statement generation
□ Reconciliation (financial matching, inventory sync)
□ Data archival / purge / retention cleanup
□ SLA breach detection + auto-escalation
□ Cache refresh / dashboard pre-computation
□ Search index rebuild / re-indexing
□ ETL to data warehouse / analytics
□ Subscription / license expiry checks
□ Audit log rotation + compliance reports
□ Customer data export jobs
□ Notification digest (daily/weekly summaries)
□ Data integrity validation / health checks
□ Tenant provisioning / deprovisioning
```

---

#### 4.8 INTEGRATIONS

```
Code   : INT-[ModCode]-[Number]
System : [External system name]
Direction: Inbound / Outbound / Bidirectional
Purpose: [Business reason]

OUTBOUND (this system → external):
  Trigger        : [Event / Schedule / User action]
  Data sent      : [Business description of payload]
  Error handling : [Retry / Alert / Queue / Fail gracefully]

INBOUND (external → this system):
  Arrival method : [Webhook / Poll / File / Queue]
  Data received  : [Business description]
  Processing     : [What system does with it]
  Acknowledgement: [How system confirms receipt]

FAILURES:
| Failure | Business Impact | Handling |
|---------|----------------|---------|
```

---

#### 4.9 NOTIFICATIONS

```
Code    : NOTIF-[ModCode]-[Number]
Trigger : [What event causes this]
Channel : Email / SMS / Push / In-App / Slack / Webhook
Recipients: [Role / User / Dynamic rule]
Timing  : Immediate / Delayed [Xmin] / Scheduled [cron]
Priority: High (instant) / Normal / Low (digest batch)

TEMPLATE:
  Email subject  : "[Subject with {{variables}}]"
  Email body     : [Full content with {{all_variables}} listed]
  SMS text       : [Max 160 chars]
  Push title     : [Short title]
  Push body      : [Short message]
  In-app message : [Notification center text]
  Variables      : {{var_name}} = [what value it holds]

CONDITIONS:
  Send when      : [Condition]
  Do not send    : [Suppression condition]
  Frequency cap  : Max [X] per [hour/day] per user
  User opt-out   : Yes (in notification preferences) / No (mandatory)
  Quiet hours    : Respect [Xpm-Yam]? Yes / No

DELIVERY:
  Retry on fail  : Yes ([X] times at [interval]) / No
  Track delivery : Yes (opens/clicks) / No
```

**Notification Summary:**
| Code | Trigger | Channel | Recipients | Timing | Cap | Opt-out |
|------|---------|---------|------------|--------|-----|---------|

---

#### 4.10 REPORTS

```
Code    : RPT-[ModCode]-[Number]
Name    : [Report name]
Purpose : [What business question this answers]
Access  : [Roles]
Trigger : On-demand / Scheduled / Event-triggered

FILTERS:
| Filter | Type | Default | Required |
|--------|------|---------|---------|

OUTPUT COLUMNS:
| Column | Source | Aggregation | Format | Sortable |
|--------|--------|-------------|--------|---------|

DATA RULES:
  Grouping    : [How grouped]
  Aggregations: [SUM/COUNT/AVG which fields]
  Default sort: [Field + direction]
  Max rows    : [X] — above this: paginate / async

EXPORT:
  Formats     : PDF / Excel / CSV
  Async export: For > [X] rows → background job + email link
  Filename    : [ReportName_YYYYMMDD_HHmm.ext]

SCHEDULED DELIVERY (if applicable):
  Frequency   : Daily / Weekly / Monthly
  Cron        : [Expression]
  Recipients  : [Email list / Role]
  Format      : [PDF / Excel]
  Condition   : Send only if [condition]

PERFORMANCE:
  SLA         : Generate within [X] seconds (standard range)
  Async cutoff: Switch to async if > [X] records
```

---

#### 4.11 DATA ENTITIES

```
Entity : [Name]
Code   : ENT-[ModCode]-[Number]
Purpose: [What this represents in business terms]
Owner  : This module
Read By: [Other modules that read this entity]

FIELDS:
| # | Field | Type | Length | Required | Unique | Indexed | Encrypted | PII | Notes |
|---|-------|------|--------|---------|--------|---------|-----------|-----|-------|

RELATIONSHIPS:
| Entity | Type | FK Field | On Delete | On Update |
|--------|------|----------|-----------|-----------|
Types: One-to-One / One-to-Many / Many-to-Many

CONSTRAINTS:
PK: [field] | UK: [field(s)] | FK: [field → table.field] | CHK: [rule]

SENSITIVITY:
| Field | Level | Masking | Encrypted | Log on Change |
|-------|-------|---------|-----------|--------------|
Levels: Public / Internal / Confidential / PII / PCI / PHI

SOFT DELETE: Yes [field: is_deleted / deleted_at] / No
AUDIT FIELDS: created_at / created_by / updated_at / updated_by — Yes / No
CONCURRENCY: Version field for optimistic locking — Yes / No

MULTI-TENANT:
  tenant_id field on entity : Yes / No
  All queries include tenant filter: Yes / No (enforced at data layer)
```

---

#### 4.12 DATA SEEDING REQUIREMENTS

```
Seed data needed before this module can function:

| Type | Description | Volume | Source | Who Provides | When |
|------|-------------|--------|--------|-------------|------|
| Master data | Country list, currencies | ~250 | Standard | Dev team | Pre-Dev |
| Reference data | Product categories | ~50 | Customer | Customer | Pre-UAT |
| Config data | System settings defaults | ~20 | Standard | Dev team | Pre-Dev |
| Role/permission data | Default roles + perms | ~10 | FSD spec | Dev team | Pre-Dev |
| Test data | Sample records for QA | ~1000 | Generated | QA | Pre-QA |
| Demo data | Realistic data for training | ~500 | Generated | PM | Pre-UAT |
```

---

#### 4.13 FEATURE FLAGS

```
| Flag Name | Feature | Default | Who Toggles | Scope | Purpose |
|-----------|---------|---------|-------------|-------|---------|
| FEAT_[NAME] | [Description] | ON/OFF | [Role] | Global/Tenant/User | [Why] |

Use cases for flags:
□ Beta features not ready for all users
□ Per-tenant activation (SaaS)
□ Emergency kill switch
□ Gradual rollout (start 10% tenants)
□ Features awaiting regulatory approval
□ A/B testing
```

---

#### 4.14 OPEN QUESTIONS

Format: `OQ-[ModCode]-[Number]: [Question]`

| Code | Question | Priority | Owner | Deadline | Blocks |
|------|---------|---------|-------|---------|--------|

```
════════════════════════════════════════════
END OF MODULE — REPEAT SECTION 4 FOR EVERY MODULE
Do not proceed to Section 5 until ALL modules are complete.
════════════════════════════════════════════
```

---

### SECTION 5: MASTER SUMMARY TABLES

**Direct inputs to the Architecture Document and Estimation.**
These tables alone allow an architect to design the system.

#### 5.1 Master Screen Inventory
| # | Code | Module | Screen | Type | Complexity | Roles |
|---|------|--------|--------|------|------------|-------|

**Count by Module:**
| Module | LOW | MEDIUM | HIGH | Total |
|--------|-----|--------|------|-------|
| **TOTAL** | | | | |

#### 5.2 Master Data Contracts (Logical Operations)
| # | Code | Module | Operation Name | Screen | Role | Cached | Real-time |
|---|------|--------|---------------|--------|------|--------|-----------|

#### 5.3 Master Business Logic Operations
| # | Code | Module | Operation | Trigger | Writes To | Events | SLA |
|---|------|--------|-----------|---------|-----------|--------|-----|

#### 5.4 Master Batch Job List
| # | Code | Module | Job Name | Trigger | Frequency | SLA | Volume | Priority |
|---|------|--------|---------|---------|-----------|-----|--------|----------|

#### 5.5 Master Integration List
| # | Code | Module | System | Direction | Trigger | Frequency | Priority |
|---|------|--------|--------|-----------|---------|-----------|----------|

#### 5.6 Master Notification List
| # | Code | Module | Trigger | Channel | Recipients | Timing |
|---|------|--------|---------|---------|------------|--------|

#### 5.7 Master Report List
| # | Code | Module | Report Name | Format | Roles | Scheduled |
|---|------|--------|------------|--------|-------|-----------|

#### 5.8 Master Data Entity List
| # | Code | Module | Entity | PII | Encrypted | Multi-tenant | Soft Delete |
|---|------|--------|--------|-----|-----------|-------------|------------|

#### 5.9 Master Business Rules
| # | Code | Module | Rule | Enforcement | Source |
|---|------|--------|------|-------------|--------|

#### 5.10 Master Feature Flags
| # | Flag | Module | Default | Scope | Purpose |
|---|------|--------|---------|-------|---------|

#### 5.11 Master Data Seeding
| # | Module | Type | Volume | Source | Who | When |
|---|--------|------|--------|--------|-----|------|

---

### SECTION 6: CROSS-CUTTING CONCERNS

Defined once. Applied everywhere. No repetition per module.

#### 6.1 Authentication Flows

**Login:**
```
1. Enter credentials
2. Validate (do not reveal which field is wrong)
3. After [X] failures: lock account [Y] mins + notify
4. If MFA enabled: prompt OTP (valid [X] mins, max [X] attempts)
5. On success: issue token + session → redirect to destination
```

**Session Management:**
| Attribute | Value |
|-----------|-------|
| Token type | JWT / Secure cookie |
| Expiry | [X] minutes |
| Refresh token | Yes ([X] days) / No |
| Concurrent sessions | Max [X] / Single only |
| Inactivity timeout | [X] mins (warn at [X-2] mins) |
| Force logout by admin | Yes / No |

**Forgot Password:**
```
1. Enter email → system sends reset link (valid [X] hrs, one-time)
2. User clicks link → enter new password (meets policy)
3. Password updated → ALL sessions invalidated → confirm email sent
```

**Account Lockout:**
| Attribute | Value |
|-----------|-------|
| Max failed attempts | [X] |
| Lockout duration | [X] minutes / Admin unlock required |
| Self-unlock | Time expiry / Email link / Admin only |

#### 6.2 Global Audit Trail

| Event | Logged | Captures | Retention |
|-------|--------|---------|----------|
| Login success/fail | Yes | User, IP, device, timestamp | 1 yr |
| Create | Yes | Entity, user, timestamp, new values | 5 yrs |
| Update | Yes | Entity, user, timestamp, old+new values | 5 yrs |
| Delete | Yes | Entity, user, timestamp, deleted values | 7 yrs |
| Permission change | Yes | User, old role, new role, by whom | 5 yrs |
| Export | Yes | User, scope, row count, timestamp | 2 yrs |
| Approval action | Yes | Approver, decision, comments | 7 yrs |
| Batch job run | Yes | Job, start, end, counts, status | 1 yr |

#### 6.3 File Upload Standards
| Attribute | Specification |
|-----------|-------------|
| Allowed types | PDF, JPEG, PNG, XLSX, DOCX, CSV [+per module] |
| Max per file | [X] MB |
| Max per record | [X] MB total |
| Storage | Cloud object storage |
| URL type | Signed URL ([X] hrs expiry) — not public |
| Virus scan | Yes before storing / No |
| Encrypted at rest | Yes / No |
| Preview | PDF + images in-browser / Download only |

#### 6.4 Export Standards
| Attribute | Specification |
|-----------|-------------|
| Formats | CSV / Excel / PDF |
| Sync max | [X] records (then async) |
| Async | Background job + email with download link |
| Download expiry | [X] hours |
| Audit | Yes (user + scope + count + timestamp) |
| Filename | [Module]_[YYYYMMDD]_[HHMM].[ext] |

#### 6.5 Pagination Standards
| Attribute | Value |
|-----------|-------|
| Default size | 20 |
| Options | 10 / 20 / 50 / 100 |
| Server max | 100 (enforced server-side) |
| Total count | Always shown: "Showing X-Y of Z" |

#### 6.6 Global Error Handling
| Error | User Sees | System Action |
|-------|----------|---------------|
| Validation | Inline errors | Block submission |
| Unauthenticated | Redirect to login + return URL | Clear session |
| Unauthorized | "You don't have access" | Log attempt |
| Not found | "This record doesn't exist" | Log 404 |
| Server error | "Something went wrong" + error ID | Log full trace |
| Timeout | "Connection lost. Retry?" | Show retry |
| Session expired | Modal prompt to re-login | Redirect on confirm |
| Maintenance | Full-page maintenance screen | 503 |

#### 6.7 Loading & Empty States
| State | User Sees |
|-------|----------|
| Loading | Skeleton loader |
| Empty — first time | Illustration + message + primary CTA |
| Empty — filtered | "No records match" + Clear filters |
| No search results | "No results for '[term]'" + Clear |
| Error loading | Error + Retry button |
| Form submitting | Button disabled + spinner |
| File uploading | Progress bar with % |

#### 6.8 Onboarding & First-Time UX
```
New user first login:
  Force password change  : Yes / No
  Profile completion     : Prompted? Yes / No
  Guided tour            : Yes ([X] steps) / No
  Empty state CTAs       : Yes (every empty screen has CTA)

New tenant (if multi-tenant):
  Setup wizard           : Yes ([X] steps) / No
  Demo/sample data       : Yes (removable) / No
  Admin checklist        : Yes (X steps to go live) / No
  Onboarding emails      : Yes ([X] emails over [Y] days) / No
```

---

### SECTION 7: NON-FUNCTIONAL SPECIFICATIONS

#### 7.1 Performance Targets
| Operation | Target | Condition |
|-----------|--------|-----------|
| Dashboard | < 3 sec | All widgets, P95 |
| List (first page) | < 2 sec | Default size, P95 |
| Form open | < 1.5 sec | P95 |
| Search | < 1 sec | After debounce |
| Standard report | < 10 sec | Default range |
| Business operation | < 500 ms | P95 |
| Batch (standard) | < 2 hrs | Overnight window |

#### 7.2 Security Specifications
```
All text inputs    : Max length enforced
All numeric inputs : Range validation
All date inputs    : Format + reasonable range
File uploads       : Type whitelist + size + virus scan
Rich text          : HTML sanitized (allow safe tags only)
All operations     : Server-side re-validation always
DB queries         : Parameterized always (no concatenation)
State changes      : CSRF protection
Sensitive fields   : Never in logs or error messages
```

#### 7.3 Accessibility
```
WCAG level         : [A / AA / AAA] (from BRD)
Images             : Alt text required
Form fields        : Label elements (not placeholder-only)
Errors             : Icon + text (not color-only)
Focus              : Visible focus ring
Keyboard nav       : All features reachable via keyboard
Screen reader      : ARIA labels on all interactive elements
```

---

### SECTION 8: ASSUMPTIONS & OPEN QUESTIONS

#### 8.1 Standard Assumptions (Always Apply)
```
ASM-001: All data formats are JSON unless Architecture decides otherwise
ASM-002: Standard pagination: page + pageSize on all list operations
ASM-003: Timestamps stored UTC; displayed in user local timezone
ASM-004: Soft delete used unless hard delete explicitly required
ASM-005: Audit fields on all entities: created_at/by, updated_at/by
ASM-006: All list screens support minimum CSV export
ASM-007: Field validation on blur; form validation on submit attempt
ASM-008: All auth checked server-side on every operation
ASM-009: API versioning from day 1; breaking changes = new version
ASM-010: File storage via cloud object storage (not server filesystem)
ASM-011: Single timezone unless multi-timezone explicitly in BRD
ASM-012: English only unless multi-language in BRD
ASM-013: Desktop-first, responsive mobile unless mobile-first stated
ASM-014: All communication over HTTPS/TLS
ASM-015: Monetary values stored as integers (cents) to avoid float errors
ASM-016: Soft-deleted records invisible to all standard queries
ASM-017: Empty states handled on all list/data screens
ASM-018: All forms prevent double-submit
ASM-019: Inactivity logout with warning dialog before expiry
ASM-020: All destructive actions require confirmation dialog
```

#### 8.2 Module-Specific Assumptions
| Code | Module | Assumption | Impact if Wrong |
|------|--------|-----------|----------------|

#### 8.3 Consolidated Open Questions
| Code | Module | Question | Priority | Owner | Deadline | Blocks |
|------|--------|---------|---------|-------|---------|--------|

#### 8.4 BRD Conflicts Found
| Code | BRD Sections | Conflict | Resolution From |
|------|-------------|---------|----------------|

#### 8.5 Architecture-Dependent Items
| Item | Depends On | Impact on FSD |
|------|-----------|--------------|
| Real-time mechanism | Architecture: WebSocket/polling/SSE | UX spec |
| File upload flow | Architecture: presigned URL vs proxy | Flow spec |
| Search technology | Architecture: DB vs Elasticsearch | Performance spec |

---

### SECTION 9: FSD COMPLETION CHECKLIST

**Do not declare FSD complete until ALL items checked.**

**Architecture Neutrality:**
- [ ] No HTTP methods (GET/POST/PUT/DELETE) in FSD
- [ ] No REST endpoint URLs (/api/v1/resource) in FSD
- [ ] No service/class/method names in FSD
- [ ] No BFF, microservices, monolith references in FSD
- [ ] Data contracts written in business language only

**Document Level:**
- [ ] All explicit modules from BRD covered
- [ ] All implicit modules identified and covered
- [ ] Master Summary Tables complete (Section 5)
- [ ] Architecture-dependent items listed (Section 8.5)

**Per Module (verify for EACH module):**
- [ ] User stories written for every feature
- [ ] ASCII sketch provided for every screen
- [ ] All fields specified (no "standard fields")
- [ ] All validations specified (field + form level)
- [ ] All business rules documented with BR codes
- [ ] Happy path + minimum 3 exception paths
- [ ] Confirmation dialogs for destructive actions
- [ ] Undo behavior specified
- [ ] Concurrent user scenarios addressed
- [ ] Error messages specified exactly
- [ ] Success messages + redirects specified
- [ ] Loading / empty / error states specified
- [ ] Mobile behavior specified
- [ ] Localisation behavior specified
- [ ] Print/PDF specified (if applicable)
- [ ] Browser tab title specified
- [ ] Keyboard shortcuts (enterprise screens)
- [ ] Feature flags specified
- [ ] Test scenario hints for QA
- [ ] Performance targets specified
- [ ] Data contracts (pattern-neutral)
- [ ] Business logic operations documented
- [ ] Batch jobs documented (or explicitly "none")
- [ ] Data seeding requirements captured
- [ ] Open questions captured with OQ codes

**Final Quality Test:**
- [ ] Frontend developer: ZERO questions needed to PM?
- [ ] Backend developer: ZERO ambiguity in business logic?
- [ ] QA engineer: 100% test cases derivable from this?
- [ ] DevOps: batch infrastructure fully plannable?
- [ ] Architect: all decisions possible from this document?
- [ ] PM: never needed as technical middleman?
