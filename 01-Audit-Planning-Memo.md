# 01 — Audit Planning Memo

**To:** Board of Directors, AeroLogistics Solutions Ltd.
**From:** Internal Audit Team
**Date:** July 1, 2026
**Subject:** Engagement Notice — ERP Access Control Audit
**Engagement Reference:** AUD-ERP-2026-001

---

## Engagement Letter

In November 2025, a procurement fraud incident was detected involving unauthorized vendor creation and fraudulent invoice processing totaling $250,000. The Board of Directors authorized a comprehensive audit of user access controls within the ERPNext environment in response. This memo confirms the engagement and outlines the audit approach, scope, timeline, and resource requirements.

---

## Audit Objectives

1. Evaluate the design and operating effectiveness of user access provisioning controls, including request, authorization, and revocation processes
2. Assess role assignment practices against the principle of least privilege
3. Identify Segregation of Duties (SOD) conflicts across finance, procurement, inventory, and operations modules
4. Verify that terminated user accounts are deactivated in a timely manner
5. Determine compliance with ISO 27001:2022 Annex A.9 (Access Control) and A.12 (Operations Security) requirements
6. Provide actionable remediation recommendations to address control deficiencies

---

## Scope Boundaries

### In-Scope

| Element | Detail |
|---|---|
| **ERP System** | ERPNext v14 (self-hosted on Ubuntu 22.04 LTS, PostgreSQL 14, nginx reverse proxy) |
| **Modules** | Finance & Accounts, Procurement, Inventory/Warehouse, Sales, HR/Payroll, System Administration |
| **User Population** | 214 active users across Finance (28), Procurement (15), Warehouse (42), Operations (53), Sales (36), HR (12), IT / System Administration (17), Executive (11) |
| **Controls Evaluated** | User access request, authorization, provisioning, deprovisioning, recertification, role management, SOD |
| **Period** | January 1, 2025 – June 30, 2026 |

### Out-of-Scope

- Physical access controls to data center and office premises
- Network-level security (firewall, VPN, IDS/IPS configurations)
- Application-level security code review
- Third-party system integrations (customs clearance API, freight tracking systems)
- Database-level direct access controls (PostgreSQL roles and permissions)

---

## ERP Environment Technical Overview

### Architecture

| Component | Specification |
|---|---|
| **ERP Platform** | ERPNext v14 (Frappe Framework v14) |
| **Hosting** | Self-hosted, single-tenant, on-premises at AeroLogistics data center (Chicago, IL) |
| **Database** | PostgreSQL 14 (primary), Redis (caching) |
| **Application Servers** | 2 x Ubuntu 22.04 LTS VMs (16 vCPU, 32 GB RAM each) |
| **Web Server** | nginx 1.24 reverse proxy with SSL termination |
| **Authentication** | Local ERPNext authentication (no LDAP/SSO integration) |
| **Backup** | Daily automated backups to S3-compatible storage, 30-day retention |

### Installed Modules

The ERPNext instance runs the following standard modules: Accounts, Buying (Procurement), Selling (Sales), Stock (Inventory/Warehouse), Manufacturing (limited deployment), HR, Payroll, Projects, and custom reporting features.

### User Demographics

| Department | User Count | Key Roles |
|---|---|---|
| Finance & Accounts | 28 | Finance Manager, Accountant, Accounts Payable, Accounts Receivable, Tax Specialist |
| Procurement | 15 | Procurement Manager, Procurement Officer, Buyer |
| Warehouse & Logistics | 42 | Warehouse Manager, Warehouse Supervisor, Inventory Clerk, Logistics Coordinator |
| Operations | 53 | Operations Manager, Depot Manager, Fleet Supervisor, Operations Analyst |
| Sales & Marketing | 36 | Sales Manager, Sales Representative, Customer Account Manager |
| Human Resources | 12 | HR Manager, HR Executive, Payroll Officer |
| IT / System Administration | 17 | System Manager (highest-privilege role in ERPNext, sometimes referred to internally as System Administrator), Database Administrator, IT Support |
| Executive | 11 | CEO, CFO, COO, Department Heads (read-only access) |

---

## Risk Assessment

### Key Risks in ERP Access Controls

| Risk ID | Risk Description | Inherent Risk | Control Area |
|---|---|---|---|
| R-01 | Users retain access after role change or termination, enabling unauthorized transactions | **Critical** | Access Deprovisioning |
| R-02 | Incompatible roles assigned to same user, enabling fraud without detection | **Critical** | Segregation of Duties |
| R-03 | Excessive privileges granted beyond job function (over-permissioned roles) | **High** | Role-Based Access Control |
| R-04 | Shared or generic accounts without individual accountability | **High** | User Authentication |
| R-05 | Access recertification not performed periodically, leading to privilege creep | **High** | Access Review |
| R-06 | Lack of audit logging or monitoring for privileged operations | **High** | Monitoring & Detection |
| R-07 | Orphaned accounts of former employees or contractors remain active | **Medium** | Access Deprovisioning |
| R-08 | No formal access request and approval workflow documented | **Medium** | Access Provisioning |
| R-09 | Vendor master data changes not logged or reviewed | **High** | Change Management |
| R-10 | Emergency/break-glass access not monitored or reviewed | **Medium** | Privileged Access |

---

## Audit Approach

### Phase 1: Data Extraction (Days 1–3)

- Extract full user master list from ERPNext via Frappe API and database queries
- Export role assignment matrix for all 214 users
- Extract permission profiles for all 87 roles
- Export system logs for user creation, modification, and deactivation events
- Request HR records for terminated employees (past 18 months)
- Collect sample access request forms and approval records

### Phase 2: User Access Provisioning Review (Days 4–9)

- Select statistical sample of 25 access requests from the audit period
- Trace each request from initiation through approval to actual access granted
- Test 5 terminated user accounts for timely deactivation
- Review the most recent access recertification exercise (completeness and coverage)

### Phase 3: Role Assignment Analysis (Days 7–12)

- Map all 87 roles to job functions and departments
- Identify orphaned roles (no active users), over-permissioned roles, and role proliferation
- Review System Manager and other privileged role assignments

### Phase 4: Segregation of Duties Analysis (Days 10–16)

- Define incompatible transaction pairs based on ERPNext capabilities and COBIT/ISO guidance
- Run automated SOD conflict detection queries against user-role matrix
- Identify users with conflicting role combinations
- Assess compensating controls (manual approvals, audit trails, supervisory review)

### Phase 5: Reporting (Days 17–21)

- Draft findings register with risk ratings
- Conduct exit conference with management
- Issue final audit report with management action plan

> **Note:** Phases 2-4 run partially in parallel with two auditors assigned.

---

## Timeline

| Week | Phase | Key Deliverables |
|---|---|---|
| **Week 1** (Jul 1–5) | Planning & Data Extraction | Approved planning memo, extracted data sets |
| **Week 1–2** (Jul 3–11) | Access Provisioning & Role Review | Provisioning test results, role mapping, findings |
| **Week 2–3** (Jul 10–18) | SOD Analysis & Compensating Controls | SOD conflict matrix, compensating control assessment |
| **Week 3** (Jul 17–21) | Reporting & Remediation Planning | Draft report, exit conference, final audit report |

---

## Resource Requirements

| Role | Allocation |
|---|---|
| Lead IT Auditor | 21 days (full engagement) |
| Senior Auditor | 15 days (phases 2–5) |
| ERPNext System Manager (IT) | 5 days (data extraction, technical support) |
| Department Process Owners | 3 days (interviews, walkthroughs) |

---

## Key Management Personnel

| Role | Name |
|---|---|
| Chief Financial Officer | Sarah Chen |
| IT Director | Marcus Webb |
| Head of Internal Audit | Jennifer Liu |
| Audit Committee Chair | Robert Kim |
