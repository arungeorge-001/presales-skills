---
name: brd-creation
description: >
  Use this skill whenever a Product Manager, Business Analyst, or any user
  needs to create a Business Requirements Document (BRD) from a customer
  document, email brief, meeting notes, or any high-level input. Triggers
  include: "create BRD", "write BRD", "prepare business requirements",
  "customer sent a document", "received requirements from client",
  "convert this to BRD", or any request to formalize business needs into
  a structured document. This skill ensures all expert-level BA checkpoints
  are covered including assumptions, security, multi-tenancy, compliance,
  integrations, scale, risks, and open questions.
role: Product Manager / Business Analyst in a Product Company
---

# BRD Creation Skill

## Overview

This skill guides Claude to act as a **Senior Business Analyst with 15+ years
of product company experience** when creating a Business Requirements Document
(BRD) from any customer-provided input.

The BRD is the **single source of truth** for what the business needs. Every
section must be completed. Missing information must be flagged explicitly using
standardized markers — never guessed or skipped.

---

## Core Behavior Rules

### 1. THINK BEFORE WRITING
- Read the entire customer document before starting
- Identify the business domain (Finance, Healthcare, Retail, HR, etc.)
- Map out all implied modules even if not explicitly stated
- Surface ambiguities BEFORE generating the BRD, or flag them inline

### 2. NEVER GUESS — FLAG GAPS
- If information is missing, use: `[NEEDS CLARIFICATION - NC-{Section}-{Number}]`
- Never invent numbers, timelines, or scope items
- Every assumption made must be documented in Section 11

### 3. BE EXHAUSTIVE ON SECURITY & COMPLIANCE
- Always ask/infer compliance needs based on the business domain
- Finance → PCI-DSS, RBI guidelines
- Healthcare → HIPAA, HL7
- EU customers → GDPR
- Government → Data sovereignty laws
- Never omit the Security section even for simple systems

### 4. MULTI-TENANCY IS ALWAYS EVALUATED
- Every BRD must explicitly state the tenancy model
- Even if single-tenant, document that decision and why

### 5. ASSUMPTIONS ARE FIRST-CLASS CITIZENS
- Every assumption gets a unique code: `ASM-{Number}`
- Every assumption includes: what was assumed, why, and impact if wrong
- Assumptions drive estimation buffer — treat them seriously

---

## BRD Document Structure

Generate the BRD in the following order. Do NOT skip any section.
If a section has no data, state why and flag it.

---

### SECTION 0: DOCUMENT CONTROL

```
Document Title    : Business Requirements Document — [System Name]
Document Version  : v1.0
Prepared By       : [PM/BA Name]
Prepared For      : [Customer / Stakeholder Name]
Date Created      : [Date]
Last Updated      : [Date]
Status            : Draft / Under Review / Approved
BRD Reference ID  : BRD-[ProjectCode]-001
```

**Revision History**
| Version | Date | Author | Changes Made | Reviewed By |
|---------|------|--------|--------------|-------------|

**Document Distribution**
| Name | Role | Access Level |
|------|------|--------------|

---

### SECTION 1: EXECUTIVE SUMMARY

**1.1 Project Overview**
- What is being built (in 3-5 sentences, plain English)
- The business problem this solves
- Why now — business urgency or trigger for this project

**1.2 Business Objectives**
| # | Objective | Success Metric (KPI) | Target Value | Timeline |
|---|-----------|---------------------|--------------|----------|

**1.3 Strategic Alignment**
- How this system aligns with the company's broader strategy
- Which business goals this enables

**1.4 Executive Constraints**
- Budget range (if mentioned or inferable)
- Hard deadlines (regulatory, contractual, seasonal)
- Leadership-mandated constraints

**1.5 Expected Business Value**
- Quantifiable benefits (cost savings, revenue, efficiency)
- Qualitative benefits (customer satisfaction, brand, compliance)

---

### SECTION 2: SCOPE

**2.1 In-Scope — Modules & Features**

List ALL modules and features explicitly in scope.
Group by module. For each module state:
- Module name
- Brief description
- Priority: P1 (Must Have) / P2 (Should Have) / P3 (Nice to Have)
- Phase: Phase 1 / Phase 2 / Future

| Module | Feature | Description | Priority | Phase |
|--------|---------|-------------|----------|-------|

**2.2 Out of Scope — Explicitly Excluded**

This is as important as in-scope. List everything NOT included.
Prevents scope creep during development.

| Item | Reason Excluded | Future Phase? |
|------|----------------|---------------|

**2.3 Deferred Items (Future Phases)**
- Features mentioned but deferred
- Decisions that affect architecture but are not built now

**2.4 Scope Boundaries**
- Which existing systems this touches vs. does not touch
- Where this system's responsibility starts and ends
- Handoff points between this system and others

---

### SECTION 3: STAKEHOLDERS

**3.1 Stakeholder Registry**
| Name | Role | Organization | Interest | Influence | Communication |
|------|------|-------------|----------|-----------|---------------|

Influence scale: High / Medium / Low
Interest scale: High / Medium / Low

**3.2 Decision Authority Matrix**
| Decision Type | Decision Maker | Consulted | Informed |
|---------------|---------------|-----------|---------|

**3.3 End User Groups**
- Who are the actual day-to-day users of the system?
- Where are they located (geography)?
- What is their technical proficiency?

---

### SECTION 4: USER PERSONAS

For each distinct user type, capture a full persona:

**Persona Template:**
```
Persona Name     : [Name / Role Title]
User Type        : Internal / External / Partner / Admin / System
Department       : [Department]
Geography        : [Location / Region]
Technical Level  : Low / Medium / High
Volume           : Approx. how many users of this type?

Goals            : What does this user want to achieve?
Pain Points      : What frustrations do they face today?
Key Tasks        : Top 5 tasks they need the system to perform
Success Looks Like: What does a great experience mean for this persona?
Special Needs    : Accessibility, language, device, or compliance needs?
```

---

### SECTION 5: FUNCTIONAL REQUIREMENTS

Format: `FR-[ModuleCode]-[Number]: [Requirement Statement]`
Priority: Must Have / Should Have / Nice to Have
Source: Which stakeholder or document this came from

**5.1 Functional Requirements by Module**

For EACH module identified in Section 2:

| ID | Requirement | Priority | Source | Acceptance Criteria |
|----|-------------|----------|--------|---------------------|

**Acceptance Criteria Format:**
```
GIVEN [context/precondition]
WHEN [user action or system event]
THEN [expected outcome]
```

**5.2 User Journey Requirements**
- High-level user journeys that cut across modules
- End-to-end flows that must work as a whole

**5.3 Reporting & Analytics Requirements**
| Report Name | Description | Consumers | Frequency | Format |
|-------------|-------------|-----------|-----------|--------|

**5.4 Notification Requirements**
| Trigger Event | Channel | Recipients | Timing | Content Summary |
|---------------|---------|------------|--------|-----------------|

**5.5 Search & Filter Requirements**
- What entities must be searchable?
- What filters are required across the system?
- Full-text search needed? Advanced search?

---

### SECTION 6: NON-FUNCTIONAL REQUIREMENTS

#### 6.1 Performance Requirements
| Metric | Requirement | Notes |
|--------|-------------|-------|
| Page load time | e.g., < 3 seconds for 95th percentile | |
| API response time | e.g., < 500ms for standard APIs | |
| Report generation | e.g., < 30 seconds for standard reports | |
| Concurrent users | e.g., 500 simultaneous users | |
| Transactions/sec | e.g., 100 TPS during peak | |
| Batch processing SLA | e.g., nightly job < 2 hours | |

#### 6.2 Availability & Reliability
| Metric | Requirement |
|--------|-------------|
| Uptime SLA | e.g., 99.9% (allows ~8.7 hours downtime/year) |
| Planned maintenance window | e.g., Sundays 2-4 AM IST |
| Recovery Time Objective (RTO) | e.g., < 4 hours |
| Recovery Point Objective (RPO) | e.g., < 1 hour data loss |
| Disaster Recovery | Active-Active / Active-Passive / Backup-Restore |

#### 6.3 Scalability Requirements
| Dimension | Current | 1 Year | 3 Years |
|-----------|---------|--------|---------|
| Total users | | | |
| Concurrent users | | | |
| Data volume (GB/TB) | | | |
| Transactions/day | | | |
| Peak load scenario | | | |

Auto-scaling expectation: Yes / No
Scaling strategy preference: Horizontal / Vertical / Both

#### 6.4 Security Requirements

**This section is MANDATORY. Never omit.**

**6.4.1 Authentication**
- Authentication mechanism: SSO / OAuth2 / SAML / Username-Password / MFA
- MFA required: Yes / No — If yes, which factors?
- Session timeout: [Duration]
- Password policy: complexity, expiry, history
- Single Sign-On: Yes / No — Provider: [Azure AD / Okta / Google / Custom]

**6.4.2 Authorization**
- Access control model: RBAC / ABAC / DAC
- Role hierarchy (if known from customer doc)
- Field-level security needed: Yes / No
- Data-level security (users see only their data): Yes / No
- IP restriction / Whitelist needed: Yes / No

**6.4.3 Data Security**
- Encryption at rest: Yes / No — Standard: AES-256 or equivalent
- Encryption in transit: Yes / No — TLS 1.2+ mandatory
- Sensitive fields (PII, financial): List all fields requiring masking/encryption
- Data masking in non-production environments: Yes / No
- Key management approach: [Cloud KMS / HSM / Custom]

**6.4.4 Audit & Logging**
- Audit trail required: Yes / No
- What events must be logged? (login, data changes, exports, admin actions)
- Log retention period: [Duration]
- Tamper-proof logs required: Yes / No
- Who can access audit logs?

**6.4.5 Vulnerability & Penetration Testing**
- Pen test required before go-live: Yes / No
- Frequency of security assessments: [Annual / Per Release]
- Security code review: Yes / No

**6.4.6 API Security**
- API authentication: API Key / OAuth2 Bearer Token / JWT
- Rate limiting required: Yes / No — Limit: [X requests/minute]
- API Gateway: Yes / No

#### 6.5 Compliance Requirements

**Domain-based compliance detection:**
- Finance / Payments → PCI-DSS, SOX, RBI Guidelines
- Healthcare → HIPAA, HITECH, HL7 FHIR
- EU customers or data → GDPR, ePrivacy
- Government → Data sovereignty, security clearance levels
- HR / Payroll → GDPR, local labor laws
- Insurance → IRDAI (India), FCA (UK), NAIC (US)
- General / Global → ISO 27001, SOC 2 Type II

| Regulation | Applicable | Key Requirements | Responsible Party |
|------------|------------|-----------------|------------------|

**Specific compliance items:**
- Data residency: Must data stay within a specific country? Yes / No
- Right to erasure (GDPR Article 17): Must support user data deletion
- Data portability: Must users be able to export their data
- Consent management: Is user consent tracking required
- Cookie consent: Yes / No (web applications)
- Data Processing Agreement (DPA) needed with vendors

#### 6.6 Accessibility Requirements
- WCAG compliance level: A / AA / AAA
- Screen reader support needed: Yes / No
- Keyboard navigation: Yes / No
- Color contrast standards
- Language/localization: Languages supported

#### 6.7 Browser & Device Compatibility
| Platform | Supported Versions / Specs |
|----------|---------------------------|
| Desktop browsers | Chrome, Firefox, Edge, Safari — last 2 versions |
| Mobile browsers | iOS Safari, Android Chrome |
| Mobile responsive | Yes / No |
| Native mobile app | iOS / Android / Both / Not required |
| Minimum screen resolution | e.g., 1280x768 |
| Offline capability | Yes / No |

---

### SECTION 7: MULTI-TENANCY REQUIREMENTS

**This section is MANDATORY. Every BRD must address tenancy.**

**7.1 Tenancy Model Decision**

| Question | Answer |
|----------|--------|
| Is this a SaaS product serving multiple customers? | Yes / No |
| Tenancy model | Single-Tenant / Multi-Tenant / Hybrid |
| Number of tenants expected (current) | |
| Number of tenants expected (3 years) | |

**7.2 If Multi-Tenant:**

**Isolation Level Required:**
| Data | Isolated per tenant? | Method |
|------|---------------------|--------|
| Application data | Yes/No | Separate DB / Schema / TenantID column |
| File storage | Yes/No | Separate buckets / Folder prefix |
| Configuration | Yes/No | Per-tenant config table |
| Audit logs | Yes/No | |

**Tenant Onboarding:**
- Self-service onboarding: Yes / No
- Admin-provisioned: Yes / No
- Onboarding SLA: [How fast must a new tenant be ready?]

**Per-Tenant Customization:**
| Customization Type | Supported | Notes |
|-------------------|-----------|-------|
| Custom branding (logo, colors) | Yes/No | |
| Custom domain / subdomain | Yes/No | |
| Custom workflows | Yes/No | |
| Custom fields | Yes/No | |
| Custom integrations | Yes/No | |
| Custom user roles | Yes/No | |
| Feature toggles per tenant | Yes/No | |

**Tenant Data Isolation for Compliance:**
- Regulatory reason for isolation (if applicable)
- Geographic data residency per tenant: Yes / No

**7.3 If Single-Tenant:**
- State clearly: "This is a single-tenant system."
- Reason for single-tenant choice
- Future consideration for multi-tenancy: Yes / No

---

### SECTION 8: INTEGRATION REQUIREMENTS

**8.1 System Integration Inventory**

| # | System Name | Direction | Data Exchanged | Frequency | Protocol | Priority |
|---|-------------|-----------|----------------|-----------|----------|----------|
| 1 | [System] | Inbound/Outbound/Both | [Data] | Real-time/Batch/On-demand | REST/SOAP/File/Queue | P1/P2 |

**8.2 For Each Integration, Capture:**
```
Integration Name  : [Name]
System            : [Third-party / Internal system name]
Direction         : Inbound / Outbound / Bidirectional
Purpose           : Why is this integration needed?
Data Exchanged    : What data flows, in what format (JSON/XML/CSV/EDI)?
Trigger           : Real-time event / Scheduled / On-demand
Frequency         : [Every X minutes / Daily / Weekly / Per transaction]
Volume            : Expected records per exchange
Authentication    : API Key / OAuth / Certificate / IP Whitelist
Error Handling    : What happens if integration fails?
SLA               : How fast must data sync?
Sandbox Available : Yes / No (impacts estimation)
Documentation     : Link to API docs (if known)
Owner             : Who owns this integration on both sides?
```

**8.3 Third-Party Services**
| Service | Purpose | Vendor | License/Cost Model |
|---------|---------|--------|-------------------|
| Email service | Transactional email | SendGrid/SES/etc. | |
| SMS gateway | OTP / notifications | Twilio/etc. | |
| Payment gateway | Online payments | Stripe/Razorpay/etc. | |
| Map service | Location features | Google Maps/etc. | |
| Analytics | Usage analytics | Mixpanel/GA/etc. | |
| Document generation | PDF/Reports | iText/etc. | |

**8.4 File-Based Integrations**
| Integration | File Format | Transfer Method | Frequency | Volume |
|-------------|-------------|-----------------|-----------|--------|
| | CSV/Excel/XML | SFTP/S3/Email | | |

---

### SECTION 9: DATA REQUIREMENTS

**9.1 Key Data Entities**
| Entity | Description | Approx. Volume | Growth Rate | Sensitivity |
|--------|-------------|----------------|-------------|-------------|

Sensitivity: Public / Internal / Confidential / Restricted

**9.2 Data Migration**
- Is data migration required? Yes / No
- Source systems for migration
- Estimated volume of historical data
- Data quality issues expected: Yes / No
- Migration approach: Big Bang / Phased / Parallel Run
- Data cleansing needed: Yes / No

**9.3 Data Retention Policy**
| Data Type | Retention Period | After Retention | Legal Basis |
|-----------|-----------------|-----------------|-------------|
| | | Archive/Delete | |

**9.4 Master Data Management**
- Is there a master data source? (Customer master, Product master etc.)
- Who owns master data?
- Sync frequency with master data

**9.5 Sensitive / PII Data Inventory**
| Data Field | Entity | Sensitivity Level | Masking Required | Encrypted |
|------------|--------|------------------|------------------|-----------|
| Full Name | User | PII | Yes | Yes |
| Email | User | PII | Partial | Yes |
| Payment Card | Transaction | PCI | Yes | Yes |

---

### SECTION 10: SCALE & GEOGRAPHY

**10.1 User Scale**
| Metric | Launch | Year 1 | Year 3 |
|--------|--------|--------|--------|
| Total registered users | | | |
| Daily active users | | | |
| Concurrent peak users | | | |
| Geographic locations | | | |

**10.2 Transaction Scale**
| Transaction Type | Daily Volume | Peak Volume | Peak Period |
|-----------------|-------------|-------------|-------------|

**10.3 Geography & Localization**
| Requirement | Details |
|-------------|---------|
| Countries/regions | |
| Languages | |
| Date/time format | |
| Currency | |
| Number format | |
| Right-to-left support | Yes/No |
| Local regulatory requirements per region | |

**10.4 Peak Load Scenarios**
- Describe specific peak scenarios (e.g., month-end processing, Black Friday, tax season)
- Expected spike multiplier (e.g., 5x normal load during peak)
- Duration of peak

---

### SECTION 11: ASSUMPTIONS

**This section is MANDATORY and must be comprehensive.**

Every assumption gets a unique code and full detail.

Format:
```
ASM-[Number]: [Assumption Statement]
Category     : [Scope / Technical / Business / Data / Integration / Compliance]
Basis        : Why was this assumption made?
Impact if Wrong : What happens to scope/cost/timeline if wrong?
Owner        : Who needs to confirm this?
Status       : Unconfirmed / Confirmed / Rejected
```

**Standard Assumptions Always Capture:**

```
ASM-001: All browser compatibility testing is for latest 2 versions only
ASM-002: Third-party API sandbox environments are available for development
ASM-003: Customer provides test data for UAT
ASM-004: Customer has necessary licenses for all third-party integrations
ASM-005: Single language unless multi-language is explicitly stated
ASM-006: Development team has access to all required cloud environments
ASM-007: No data migration unless explicitly stated in scope
ASM-008: Email/SMS provider selection is customer's responsibility
ASM-009: UAT is conducted by customer, not the development team
ASM-010: Performance testing environment mirrors production specifications
```

---

### SECTION 12: CONSTRAINTS

**12.1 Business Constraints**
| Constraint | Description | Impact |
|------------|-------------|--------|
| Timeline | Hard deadline and why | |
| Budget | Range or ceiling | |
| Resource | Team size or skill limitations | |
| Regulatory | Compliance deadlines | |

**12.2 Technical Constraints**
| Constraint | Description | Impact on Architecture |
|------------|-------------|----------------------|
| Mandated cloud | e.g., Must use Azure | |
| Mandated language | e.g., Must use Java | |
| Existing infrastructure | e.g., Must integrate with legacy SAP | |
| Open source only | Cannot use licensed software | |
| Security clearance | Special access requirements | |
| Data center location | Must host in specific region | |

**12.3 Organizational Constraints**
- Team availability and skill set
- Change management constraints (user adoption risk)
- Policy constraints (IT policies, procurement rules)

---

### SECTION 13: DEPENDENCIES

**13.1 Internal Dependencies**
| Dependency | Owner | Required By | Risk if Delayed |
|------------|-------|-------------|-----------------|

**13.2 External Dependencies**
| Dependency | Vendor/Party | Required By | SLA | Risk |
|------------|-------------|-------------|-----|------|
| API credentials | Third-party | Dev start | | |
| Infrastructure access | IT/Cloud team | Dev start | | |
| Legal sign-off | Legal | Before go-live | | |
| Compliance cert | Regulator | Before go-live | | |

**13.3 Predecessor Projects or Systems**
- Are there other projects this depends on completing first?
- Is there a system being decommissioned that this replaces?

---

### SECTION 14: RISKS

| # | Risk | Category | Probability | Impact | Severity | Mitigation |
|---|------|----------|-------------|--------|----------|------------|

Risk Categories: Business / Technical / Data / Compliance / Resource / Third-Party
Probability: High / Medium / Low
Impact: High / Medium / Low
Severity: Critical / High / Medium / Low (Probability × Impact)

**Always include these standard risk checks:**
- [ ] Scope creep risk (requirements may expand)
- [ ] Integration risk (third-party APIs may not work as expected)
- [ ] Data quality risk (migration data may be incomplete/dirty)
- [ ] Compliance risk (regulatory approval may take longer)
- [ ] Resource risk (key team members unavailable)
- [ ] Technology risk (chosen stack has unknown limitations)
- [ ] Vendor risk (third-party may not meet SLA)

---

### SECTION 15: OPEN QUESTIONS & CLARIFICATIONS NEEDED

**This section consolidates ALL `[NEEDS CLARIFICATION]` items.**

Format:
```
NC-[Section]-[Number]: [Question]
Priority : High / Medium / Low
Owner    : [Who must answer — Customer / PM / Legal / IT]
Deadline : [When needed — before FSD / before Architecture / before UAT]
Impact   : What is blocked until this is answered?
```

| Code | Question | Priority | Owner | Deadline | Impact |
|------|----------|----------|-------|----------|--------|

---

### SECTION 16: GLOSSARY

| Term | Definition | Context |
|------|-----------|---------|

---

### SECTION 17: DOCUMENT SIGN-OFF

| Role | Name | Signature | Date | Status |
|------|------|-----------|------|--------|
| Business Owner | | | | Approved/Pending |
| Product Manager | | | | Approved/Pending |
| Technical Lead | | | | Reviewed/Pending |
| QA Lead | | | | Reviewed/Pending |
| Customer Representative | | | | Approved/Pending |

---

## Output Quality Checklist

Before finalizing the BRD, verify ALL of the following:

### Completeness Checks
- [ ] Every section has content or an explicit "Not Applicable" with reason
- [ ] Every module mentioned in customer doc is captured in scope
- [ ] All user types from customer doc appear as personas
- [ ] Every integration mentioned is in Section 8
- [ ] All `[NEEDS CLARIFICATION]` items are in Section 15

### Expert BA Checks
- [ ] In-scope AND out-of-scope both explicitly defined
- [ ] Acceptance criteria written for all P1 requirements
- [ ] Security section complete with auth, authorization, encryption, audit
- [ ] Compliance requirements detected from business domain and documented
- [ ] Multi-tenancy decision made and documented
- [ ] All PII/sensitive data fields inventoried in Section 9.5
- [ ] Data retention policies defined
- [ ] All assumptions have impact statements
- [ ] Risks have mitigations
- [ ] Performance benchmarks specified
- [ ] Availability SLA (RTO/RPO) specified
- [ ] Scalability projections for 1yr and 3yr captured
- [ ] Sign-off section populated

### Flag Usage Checks
- [ ] `[NEEDS CLARIFICATION - NC-XX-XX]` used for ALL missing info
- [ ] `ASM-XXX` codes assigned to all assumptions
- [ ] `FR-[Module]-XXX` codes assigned to all requirements
- [ ] No guessed numbers or invented scope items

---

## Standardized Flag Reference

| Flag | When to Use | Format |
|------|-------------|--------|
| NEEDS CLARIFICATION | Information missing from customer doc | `[NEEDS CLARIFICATION - NC-{Section}-{Number}]` |
| ASSUMPTION | Something assumed, not confirmed | `ASM-{Number}` in Section 11 |
| OUT OF SCOPE | Explicitly excluded item | State in Section 2.2 |
| FUTURE PHASE | Deferred feature | State in Section 2.3 |
| HIGH RISK | Risk that could derail project | Flag in Section 14 with Severity: Critical |
| COMPLIANCE FLAG | Regulatory item requiring legal review | `[COMPLIANCE FLAG - {Regulation}]` |

---

## Domain-Specific Intelligence

When you identify the business domain from the customer document,
automatically apply the following domain-specific requirements:

### Financial / Banking / Payments
- PCI-DSS compliance for card data
- RBI guidelines (India) / FCA (UK) / SEC/FINRA (US)
- Double-entry audit trail for all transactions
- Maker-Checker workflow for high-value transactions
- Reconciliation requirements
- Regulatory reporting requirements
- Anti-money laundering (AML) checks

### Healthcare
- HIPAA compliance (US) / NHS standards (UK)
- HL7 FHIR for data exchange
- Patient consent management
- Clinical audit trails
- Integration with EMR/EHR systems
- Drug interaction / clinical decision support rules

### E-commerce / Retail
- PCI-DSS for payments
- GST/VAT calculation requirements
- Inventory management rules
- Return/refund policy logic
- Marketplace vs. direct model

### HR / Payroll
- GDPR for employee data
- Local labor law compliance per country
- Payroll calculation rules (taxes, deductions)
- Leave management regulations
- Statutory reporting

### Government / Public Sector
- Data sovereignty requirements
- Security clearance levels
- Accessibility mandates (Section 508 US / EN 301 549 EU)
- Open data requirements
- Public procurement rules

### SaaS / Multi-Tenant Platform
- Always recommend multi-tenant architecture evaluation
- Tenant onboarding automation
- Usage metering / billing per tenant
- Feature flag management per tenant
- Tenant data export for portability

---

## Example Output Format

The BRD should be output as:

1. **Document Control block** at the top
2. **Each section with clear heading** (Section 1: Executive Summary, etc.)
3. **Tables** for structured data (stakeholders, requirements, risks)
4. **Code blocks** for structured templates (persona, assumption, etc.)
5. **Flag markers** inline where information is missing
6. **Summary Open Questions table** at the end (Section 15)

Length expectation: A thorough BRD for a medium complexity system should be
40-80 pages equivalent. Do not truncate. Do not summarize prematurely.
If the customer document is short/high-level, that means MORE flags and
assumptions — not a shorter BRD.

---

## Karpathy Principles Applied to BRD Creation

The following principles from Andrej Karpathy's LLM guidelines are
adapted for BRD creation (NOTE: The original Karpathy skills repo is
for coding tasks and is NOT applicable here — these are adapted equivalents):

**Think Before Writing**
- Read full customer document before starting any section
- Identify business domain first — it determines compliance requirements
- Map all implied modules before writing scope

**Surface Assumptions Explicitly**
- Never silently assume — document every assumption with ASM-code
- Present ambiguities as NC flags, not silent decisions
- Push back or flag when customer document contradicts itself

**Completeness Over Speed**
- A BRD with gaps is worse than a delayed BRD
- Every `[NEEDS CLARIFICATION]` represents a risk — surface them all
- Missing security/compliance requirements discovered late = costly rework

**Goal-Driven Output**
- The goal is a document that a developer, architect, QA, and estimator
  can ALL use independently without needing to ask the PM questions
- Success = zero ambiguity for the technical team
- Test: Would an architect need to ask the PM anything after reading this?
  If yes, the BRD is incomplete.
