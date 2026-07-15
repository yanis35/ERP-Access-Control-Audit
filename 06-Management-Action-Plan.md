# 06 — Management Action Plan

**Remediation Roadmap:** 30-60-90 Day Plan  
**Status Date:** July 21, 2026  
**Overall Owner:** Chief Financial Officer (CFO), with delegated ownership to IT Director and Department Heads

---

## Remediation Governance

The Management Action Plan (MAP) will be tracked through the following governance structure:

- **Executive Sponsor:** CFO — provides authority and resources
- **Program Owner:** IT Director — coordinates remediation activities across technical and functional teams
- **Audit Liaison:** Head of Internal Audit — validates remediation evidence and tracks status
- **Frequency of Review:** Bi-weekly steering committee meetings during the 90-day remediation period
- **Status Reporting:** Monthly dashboard to Audit Committee

---

## Action Plan Summary

| Total Findings | Actions Defined | 30-Day Milestone | 60-Day Milestone | 90-Day Milestone |
|---|---|---|---|---|
| 18 | 24 remediation actions | 9 actions | 8 actions | 7 actions |

---

## 30-Day Remediation Actions (Days 1–30)

| Ref ID | Finding(s) | Action Description | Responsible Owner | Target Date | Priority | Status |
|---|---|---|---|---|---|---|
| MAP-01 | F-007 | Immediately deactivate terminated employee accounts for Patricia Nguyen and Rebecca Turner. Verify no unauthorized activity occurred using these accounts. | IT Director | Day 1 | **Critical** | Open |
| MAP-02 | F-010 | Restructure the Warehouse Supervisor role: remove all procurement permissions (vendor creation, PO creation, invoice processing). Implement as separate procurement roles requiring dual authorization. | IT Director / Procurement Head | Day 7 | **Critical** | Open |
| MAP-03 | F-011 | Remove all business roles (Finance Manager, Purchase Manager, Stock Manager) from the four IT staff accounts holding System Manager role. Create separate emergency accounts if needed. | IT Director | Day 7 | **Critical** | Open |
| MAP-04 | F-016 | Remove Vendor Creator or Purchase Order Creator roles from affected users identified in the SOD conflict matrix. Implement segregation so no single user holds both roles. | IT Director / Procurement Head | Day 7 | **Critical** | Open |
| MAP-05 | F-017 | Separate invoice approval and payment processing roles across three finance user accounts (Maria Santos, Elena Popescu, AP clerk). Reassign to Finance Manager and Accounts Payable respectively. | CFO / Finance Controller | Day 7 | **Critical** | Open |
| MAP-06 | F-013 | Immediately correct the Ops Viewer role permissions — remove write access on Stock Ledger Entry and Serial No DocTypes. Verify 22 affected users now have true read-only access. | IT Director | Day 14 | **High** | Open |
| MAP-07 | F-003 | Implement a privileged access management (PAM) workflow: new process requiring written approval from IT Director for any System Manager or privileged role assignment. Quarterly recertification of all privileged roles. | IT Director | Day 21 | **High** | Open |
| MAP-08 | F-012 | Review and restructure the three legacy accounts with combined Finance Manager and Procurement Officer roles. Adjust to match current job functions. | CFO / IT Director | Day 21 | **High** | Open |
| MAP-09 | F-018 | Configure daily ERPNext activity log review for high-risk events. Deploy automated alerting for: vendor creation, large journal entries (>$5K), payment batch changes, new user creation with privileged roles. | IT Director | Day 30 | **High** | Open |

### 30-Day Milestone Review

| Deliverable | Target |
|---|---|
| Critical SOD conflicts resolved | 100% (5 of 5) |
| Terminated accounts deactivated | 100% (2 of 2) |
| Privileged access process documented | 100% |
| Audit log monitoring configured | 100% |

---

## 60-Day Remediation Actions (Days 31–60)

| Ref ID | Finding(s) | Action Description | Responsible Owner | Target Date | Priority | Status |
|---|---|---|---|---|---|---|
| MAP-10 | F-001, F-002 | Implement two-step provisioning process: (1) IT provisions requested roles only, (2) Department head confirms access within 5 business days via a formal acknowledgment form. Deploy ERPNext workflow rules. | IT Director / HR Head | Day 35 | **High** | Open |
| MAP-11 | F-004 | Define standard permission tiers (Read, Create, Modify, Full) for each DocType per job role. Map all 87 roles to specific tiers. Prohibit ad hoc permission changes. | IT Director | Day 40 | **High** | Open |
| MAP-12 | F-005 | Segregate high-risk financial permissions (journal entry creation, vendor setup, payment processing) into separate roles requiring explicit department head and finance approval. | CFO / IT Director | Day 40 | **High** | Open |
| MAP-13 | F-008 | Establish semi-annual access recertification program. Schedule first full recertification for Q3 2026. Develop recertification packages using ERPNext user permission reports. | IT Director / Department Heads | Day 45 | **High** | Open |
| MAP-14 | F-006 | Create formal termination checklist with 24-hour SLA for account deactivation. Implement integration between HRIS and ERPNext for automated deprovisioning trigger. | IT Director / HR Head | Day 50 | **Medium** | Open |
| MAP-15 | F-009 | Consolidate similar custom roles. Target: reduce 47 custom roles to 25–30. Identify role consolidation candidates using permission comparison analysis. | IT Director | Day 55 | **Medium** | Open |
| MAP-16 | F-014 | Define and enforce role naming convention (format: [Module]_[Function]_[Level]). Rename all remaining custom roles. Update new role request template to include naming rules. | IT Director | Day 60 | **Low** | Open |
| MAP-17 | F-015 | Remove all orphaned roles (9) from the system after documenting them for audit trail. Establish quarterly cleanup process. | IT Director | Day 60 | **Low** | Open |

### 60-Day Milestone Review

| Deliverable | Target |
|---|---|
| Provisioning process redesigned | 100% (with acknowledgment workflow) |
| Role consolidation | 50% reduction in custom roles (47 to 25) |
| Access recertification program launched | First cycle scheduled |
| Termination process automated | Workflow implemented |
| Role naming and cleanup complete | All roles renamed, orphans removed |

---

## 90-Day Remediation Actions (Days 61–90)

| Ref ID | Finding(s) | Action Description | Responsible Owner | Target Date | Priority | Status |
|---|---|---|---|---|---|---|
| MAP-18 | F-009 | Establish Role Change Advisory Board (RCAB) with representatives from IT, Finance, Operations, and Internal Audit. Implement review and approval process for all new role requests. | IT Director / CFO | Day 65 | **Medium** | Open |
| MAP-19 | F-010, F-016 | Deploy automated SOD conflict detection tool: develop custom ERPNext scripts or integrate third-party tool to flag new conflicting role assignments in real time. Run weekly SOD conflict reports. | IT Director | Day 70 | **High** | Open |
| MAP-20 | F-018 | Integrate ERPNext audit logs with SIEM platform (evaluate Wazuh or Splunk options). Implement real-time correlation rules for SOD violations, privileged actions, and anomalous transaction patterns. | IT Director / Security Lead | Day 75 | **Medium** | Open |
| MAP-21 | F-001 to F-018 | Conduct first full access recertification cycle — all 214 users reviewed by department heads. Compile and present results to Audit Committee. | Department Heads / IT Director | Day 80 | **High** | Open |
| MAP-22 | F-016, F-017 | Implement mandatory dual authorization for: (1) all vendor master changes, (2) purchase orders over $2K, (3) supplier invoices over $1K, (4) journal entries over $2K. | CFO / IT Director | Day 85 | **High** | Open |
| MAP-23 | F-003, F-011 | Implement just-in-time (JIT) privileged access for System Manager and other critical roles. Privileged access requests must include business justification, duration, and automatic revocation. | IT Director | Day 90 | **Medium** | Open |
| MAP-24 | All | Conduct audit close-out: Internal Audit validates remediation evidence for all 18 findings. Prepare final closure report for Audit Committee. | Head of Internal Audit | Day 90 | **Medium** | Open |

### 90-Day Milestone Review

| Deliverable | Target |
|---|---|
| SOD conflict detection automated | Real-time detection deployed |
| SIEM integration complete | ERPNext logs feeding SIEM |
| Full recertification cycle | 100% of users reviewed |
| Dual authorization deployed | Vendors, POs, invoices, journal entries |
| JIT privileged access | Implemented for critical roles |
| Audit closure | All findings validated or in-progress |

---

## Long-Term Recommendations (Post 90-Day Horizon)

| Ref ID | Recommendation | Owner | Timeframe |
|---|---|---|---|
| LTR-01 | Implement Single Sign-On (SSO) with multi-factor authentication (MFA) for all ERPNext users | IT Director | Q4 2026 |
| LTR-02 | Conduct annual ERPNext role and permission review as part of the compliance calendar | IT Director / Internal Audit | Ongoing (annual) |
| LTR-03 | Implement ERPNext upgrade to latest version to leverage enhanced permission and workflow capabilities | IT Director | Q1 2027 |
| LTR-04 | Perform a third-party penetration test focused on ERP access controls | Internal Audit | Q2 2027 |
| LTR-05 | Develop and maintain a role-description catalog mapping all roles to job functions, permissions, and SOD restrictions | IT Director / HR | Q4 2026 |
| LTR-06 | Establish an ERP governance committee with quarterly meetings to review access control metrics | CFO | Q4 2026 |

---

## Risk Acceptance Process

For any remediation actions that cannot be completed within the 90-day timeline, the responsible owner must submit a Risk Acceptance Request (RAR) to the Audit Committee documenting:

1. Finding ID and risk rating
2. Reason for delay
3. Interim compensating controls
4. Revised target date
5. Owner signature and executive sponsor sign-off

All accepted risks will be tracked in the findings register with a "Risk Accepted" status and reviewed quarterly.

---

## Tracking and Reporting

| Report | Frequency | Audience |
|---|---|---|
| Remediation Status Dashboard | Bi-weekly | Steering Committee |
| Progress Update to Audit Committee | Monthly | Audit Committee / Board |
| Final Remediation Closure Report | Day 90+ | Board of Directors |

---

## Sign-Off

| Role | Name | Date | Signature |
|---|---|---|---|
| CFO (Executive Sponsor) | Sarah Chen | July 25, 2026 | |
| IT Director (Program Owner) | Marcus Webb | July 25, 2026 | |
| Head of Internal Audit | Jennifer Liu | July 25, 2026 | |
| Audit Committee Chair | Robert Kim | July 25, 2026 | |
