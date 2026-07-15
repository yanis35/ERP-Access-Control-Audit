# ERP Access Control Audit Walkthrough — AeroLogistics Solutions

**Audit Date:** July 2026  
**Engagement Type:** Post-Incident Access Control Review  
**ERP Platform:** ERPNext v14 (self-hosted)  
**Company:** AeroLogistics Solutions Ltd.

---

## Background

AeroLogistics Solutions is a mid-size supply chain and logistics company with 500 employees and approximately $150M in annual revenue. The organization operates on a self-hosted ERPNext instance (v14) managing core business processes across finance, operations, inventory, procurement, human resources, and sales modules. The system serves 200+ users across six departments.

In November 2025, a procurement fraud incident was identified: a warehouse supervisor exploited excessive system privileges to create a fictitious vendor, submit fraudulent purchase orders, and process approximately $250,000 in unauthorized invoices over an eight-month period. The incident remained undetected due to the absence of segregation of duties controls, lack of vendor master change monitoring, and the employee holding incompatible roles across procurement and finance functions.

This audit was engaged to comprehensively review user access provisioning, role assignments, and segregation of duties across the ERPNext environment.

---

## Scope

| Scope Element | Detail |
|---|---|
| **In-Scope Modules** | Finance & Accounts, Procurement, Inventory/Warehouse, Sales, HR, System Administration |
| **User Population** | 214 active ERP users across Finance, Operations, Procurement, Warehouse, Sales, and HR |
| **Audit Period** | January 2025 – June 2026 (18 months) |
| **Standards Referenced** | ISO 27001:2022 (Annex A.9 Access Control, A.12 Operations Security), ITGC, COBIT 2019 |
| **Exclusions** | Network-level access controls, physical security, third-party vendor systems |

---

## Methodology

1. **Planning & Risk Assessment** — Define scope, identify key risks, document ERP environment
2. **Data Extraction** — Export user master list, role assignments, permission profiles, transaction logs from ERPNext
3. **User Access Provisioning Review** — Sample test 25 access requests, test terminations (5 users), review recertification
4. **Role Assignment Analysis** — Map roles to job functions, identify orphaned/over-permissioned roles, analyze role hierarchy
5. **Segregation of Duties Analysis** — Define incompatible pairs, test 214 users for SOD conflicts, assess compensating controls
6. **Reporting & Remediation Planning** — Document findings, assign risk ratings, build 30-60-90 day action plan

---

## Key Metrics

| Metric | Value |
|---|---|
| Total ERP Users Reviewed | 214 |
| Active User Accounts | 214 |
| Deactivated/Orphaned Accounts Identified | 11 |
| Unique Role Definitions | 87 (40 standard ERPNext + 47 custom) |
| Critical Roles Identified | 12 |
| Access Request Records Sampled | 25 |
| Terminated User Accounts Tested | 5 |
| SOD Conflict Pairs Defined | 12 |
| User-Level SOD Conflicts Detected | 10 |
| **Total Findings** | **18** |
| Access Provisioning Findings | 8 |
| Role Assignment Findings | 7 |
| Segregation of Duties Findings | 2 |
| Monitoring & Oversight Findings | 1 |

---

## Findings Summary by Severity

| Risk Rating | Count |
|---|---|
| **Critical** | 3 |
| **High** | 7 |
| **Medium** | 6 |
| **Low** | 2 |

---

## Table of Contents

| # | Document | Description |
|---|---|---|
| 1 | [01-Audit-Planning-Memo.md](01-Audit-Planning-Memo.md) | Engagement memo, audit objectives, scope boundaries, risk assessment, and 3-week timeline |
| 2 | [02-User-Access-Provisioning.md](02-User-Access-Provisioning.md) | Access provisioning test results, sample testing (25 requests), termination testing (5 users), recertification review |
| 3 | [03-Role-Assignment-Review.md](03-Role-Assignment-Review.md) | Role hierarchy analysis, critical role mapping, over-permissioned roles, role proliferation |
| 4 | [04-SOD-Conflict-Matrix.md](04-SOD-Conflict-Matrix.md) | Incompatible pair definitions, user-level conflict analysis (10 conflicts), compensating controls |
| 5 | [05-Findings-Register.md](05-Findings-Register.md) | Complete findings register — 18 findings with risk ratings, ISO 27001 references, root cause analysis |
| 6 | [06-Management-Action-Plan.md](06-Management-Action-Plan.md) | Remediation roadmap, 30-60-90 day plan, responsible owners, tracking status |
