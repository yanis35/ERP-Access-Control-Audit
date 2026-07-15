# 05 — Findings Register

**Total Findings:** 18  
**Critical:** 3 | **High:** 9 | **Medium:** 4 | **Low:** 2  
**Standards Referenced:** ISO 27001:2022, ITGC, COBIT 2019

---

## Findings Summary by Domain and Severity

| Domain | Critical | High | Medium | Low | Total |
|---|---|---|---|---|---|
| Access Provisioning | 2 | 4 | 2 | 0 | 8 |
| Role Assignment | 1 | 3 | 1 | 2 | 7 |
| Segregation of Duties | 0 | 2 | 0 | 0 | 2 |
| Monitoring & Oversight | 0 | 0 | 1 | 0 | 1 |
| **Total** | **3** | **9** | **4** | **2** | **18** |

---

## Summary by Severity

| Risk Rating | Count | Criteria |
|---|---|---|
| **Critical** | 3 | Control deficiency presents immediate and material risk of financial loss, regulatory penalty, or systemic fraud |
| **High** | 9 | Control deficiency significantly increases risk of unauthorized access, transaction errors, or undetected fraud |
| **Medium** | 4 | Control deficiency represents a process weakness that could be exploited in combination with other gaps |
| **Low** | 2 | Control deficiency is administrative or procedural with limited direct risk to financial reporting or data integrity |

---

## Detailed Findings Register

### Domain: Access Provisioning

---

#### F-001 — Over-Provisioning of Access — Users Receive Unapproved Roles

| Attribute | Detail |
|---|---|
| **Risk Rating** | **High** |
| **ISO Reference** | ISO 27001:2022 A.9.2.1 (User registration and de-registration), A.9.2.3 (Management of privileged access rights) |
| **Description** | Testing of 25 access requests revealed that in 10 cases (40%), users were granted roles that were not included in the approved access request. Common examples include warehouse staff receiving Vendor Creator rights and AP clerks receiving Payment Approver permissions. The provisioning process does not include a verification step where the requestor confirms the access granted matches the request. |
| **Root Cause** | No post-provisioning verification process; IT staff have discretion to add roles they consider "helpful"; no system-enforced limitation preventing role assignments beyond those approved. |
| **Business Impact** | Users accumulate permissions beyond their job requirements, increasing the risk of unauthorized transactions, data exposure, and fraud. The procurement fraud incident was facilitated by this exact weakness. |
| **Recommendation** | Implement a two-step provisioning process: (1) IT provisions only the requested roles, (2) Department head must formally acknowledge and confirm access within 5 business days. Deploy ERPNext workflow rules to restrict role assignment to approved role profiles per job function. |

---

#### F-002 — Role Scope Creep — Multiple Extra Roles Assigned Without Approval

| Attribute | Detail |
|---|---|
| **Risk Rating** | **Medium** |
| **ISO Reference** | ISO 27001:2022 A.9.2.1 |
| **Description** | Three tested access requests (AR-002, AR-012, AR-017) showed users receiving 2–3 additional roles beyond what their job function required, such as an Accounts Receivable user also receiving Accountant modify permissions and a Customer Account Manager also receiving Sales Representative role. |
| **Root Cause** | IT staff use template-based provisioning where some templates include multiple bundled roles; no role-mining exercise has been performed to identify minimum necessary permissions per job role. |
| **Business Impact** | Gradual privilege creep across the user population; difficult to detect without recertification. |
| **Recommendation** | Decompose role templates into atomic role definitions. Require separate approval for each incremental role module. Perform a quarterly role-mining exercise. |

---

#### F-003 — Privileged Access Granted Without Authorization

| Attribute | Detail |
|---|---|
| **Risk Rating** | **Critical** |
| **ISO Reference** | ISO 27001:2022 A.9.2.3 (Management of privileged access rights) |
| **Description** | User account for David Kim (IT Support) was assigned the "System Manager (read-only)" role in addition to IT Support role without any documented approval. No privileged access request process exists. David Kim was granted System Manager (read-only) role without written authorization (sample AR-004). Create a separate finding (e.g., F-019) for periodic privileged access recertification if needed. |
| **Root Cause** | No privileged access management (PAM) policy or process; IT staff self-provision privileged roles without approval. |
| **Business Impact** | Unauthorized privileged access enables IT staff to modify system configurations, access all data, and alter or delete audit logs without detection or accountability. |
| **Recommendation** | Implement a privileged access management process: (1) Separate request and approval workflow for all privileged roles, (2) Enable ERPNext audit trail for all privileged actions, (3) Quarterly recertification of all privileged role assignments, (4) Consider implementing just-in-time (JIT) privileged access. |

---

#### F-004 — Higher Privilege Level Than Job Function Requires

| Attribute | Detail |
|---|---|
| **Risk Rating** | **High** |
| **ISO Reference** | ISO 27001:2022 A.9.2.1 (User registration and de-registration), A.9.2.3 (Management of privileged access rights) |
| **Description** | Two users (Kevin Wilson — AR-014 (Access Review sample identifier — see 02-User-Access-Provisioning.md), Amanda Scott — AR-021) were assigned roles with modify-level permissions when their access requests specified read-only or create-only access. Kevin, a Buyer, received Procurement Officer with modify rights; Amanda, an Ops Analyst, received Inventory Adjustor rights. |
| **Root Cause** | ERPNext permission levels are not mapped to job function tiers; IT assigns the closest available role rather than tailoring permissions. |
| **Business Impact** | Users can modify or delete records that they should only be able to view or create, undermining data integrity. |
| **Recommendation** | Define standard permission tiers (Read, Create, Modify, Full) for each DocType per job role. Map roles to specific permission tiers and prohibit ad hoc changes. |

---

#### F-005 — Financial Transaction Permissions Assigned Without Approval

| Attribute | Detail |
|---|---|
| **Risk Rating** | **High** |
| **ISO Reference** | ISO 27001:2022 A.9.2.1, A.9.4.1 (Information access restriction) |
| **Description** | User Emily White (AR-019) was assigned "Journal Entry Creator" permissions in addition to her Accountant role. Journal entry creation is a high-risk financial function that should require explicit approval. No evidence of approval for this additional permission was found. |
| **Root Cause** | IT team does not differentiate between standard and high-risk permissions during provisioning. All permissions within a role are assigned uniformly. |
| **Business Impact** | Users gain access to financial transaction capabilities that can be used to manipulate financial records or conceal unauthorized activities. |
| **Recommendation** | Prohibit assignment of high-risk financial permissions (journal entry creation, vendor setup, payment processing) in combination with standard accounting roles unless explicitly approved by the CFO. |

---

#### F-006 — Delayed Account Deactivation After Employee Termination

| Attribute | Detail |
|---|---|
| **Risk Rating** | **Medium** |
| **ISO Reference** | ISO 27001:2022 A.9.2.6 (Removal or adjustment of access rights) |
| **Description** | Testing of 5 terminated employees revealed that account deactivation occurred an average of 15.7 days after termination, with a range of 13 to 18 days. No formal termination checklist exists requiring HR to notify IT of terminations, and IT has no automated trigger for account deactivation. |
| **Root Cause** | Manual termination process relies on ad hoc email notifications from HR to IT; no SLA for deactivation; no automated integration between HR system and ERPNext. |
| **Business Impact** | Former employees retain system access for extended periods, creating opportunity for unauthorized access, data exfiltration, or retaliatory actions. |
| **Recommendation** | Establish a formal termination checklist with a 24-hour SLA for account deactivation. Implement automated deprovisioning by connecting the HRIS to ERPNext via API or intermediate middleware. Require IT to confirm deactivation within 1 business day. |

---

#### F-007 — Terminated Employee Accounts Remain Active

| Attribute | Detail |
|---|---|
| **Risk Rating** | **Critical** |
| **ISO Reference** | ISO 27001:2022 A.9.2.6 |
| **Description** | Two terminated employees — Patricia Nguyen (Procurement, terminated Apr 3, 2025) and Rebecca Turner (Finance, terminated Nov 1, 2025) — still have active ERPNext accounts as of the audit date (Jul 2026). Patricia's account has been active for 458+ days post-termination, Rebecca's for 261+ days. No one has logged into either account, but both retain full access to procurement and finance modules respectively. |
| **Root Cause** | HR does not consistently notify IT of terminations; no periodic reconciliation between HR termination records and ERPNext user account status. |
| **Business Impact** | Terminated employees with active accounts represent a significant security risk — accounts can be compromised, used for unauthorized access, or exploited by remaining employees who know the credentials. |
| **Recommendation** | Immediately deactivate both accounts. Implement monthly reconciliation between HR termination reports and ERPNext active user list. Automate the deactivation process via HR-ERPNext integration. |

---

#### F-008 — No Formal Access Recertification Program

| Attribute | Detail |
|---|---|
| **Risk Rating** | **High** |
| **ISO Reference** | ISO 27001:2022 A.9.2.5 (Review of user access rights) |
| **Description** | AeroLogistics has never performed a complete, organization-wide access recertification. The post-incident review in December 2025 covered only 20% of users (Finance and Procurement departments only) and did not result in remediation actions until June 2026. No scheduled annual recertification cycle exists. |
| **Root Cause** | Access recertification is not included in the annual compliance calendar; no owner assigned for this process. |
| **Business Impact** | Without recertification, privilege creep goes undetected, orphaned accounts persist, and inappropriate role combinations remain active. This is a direct non-compliance with ISO 27001:2022 A.9.2.5. |
| **Recommendation** | Implement a semi-annual recertification program where department heads review and certify user access for their teams. Use ERPNext's built-in user permission reporting to generate recertification packages. Report results to the Audit Committee. |

---

### Domain: Role Assignment

---

#### F-009 — Role Proliferation — 47 Custom Roles with Minimal Differentiation

| Attribute | Detail |
|---|---|
| **Risk Rating** | **Medium** |
| **ISO Reference** | ISO 27001:2022 A.9.2.2 |
| **Description** | Analysis of the 47 custom roles revealed that 28 roles (60%) are near-duplicates of standard ERPNext roles with only minor permission differences. Examples include "Procurement Analyst" (differs from "Purchase User" by two read permissions) and three nearly identical warehouse viewer roles. |
| **Root Cause** | No role governance or review board; roles created on-demand by IT without approval, consolidation, or impact analysis. |
| **Business Impact** | Role matrix is complex and difficult to maintain, increasing the risk of mis-assignment and making audits resource-intensive. 1,850+ role assignments must be manually reviewed. |
| **Recommendation** | Consolidate similar roles into standardized role templates. Establish a Role Change Advisory Board (RCAB) to review and approve all new role requests. Deploy role-mining tools to identify consolidation opportunities. |

---

#### F-010 — Warehouse Supervisor Role Over-Permissioned with Procurement Rights

| Attribute | Detail |
|---|---|
| **Risk Rating** | **Critical** |
| **ISO Reference** | ISO 27001:2022 A.9.2.2, A.9.2.3 |
| **Description** | The custom Warehouse Supervisor role includes create and modify permissions on purchase orders, goods receipt, supplier invoices, and inventory adjustments. This role allows any of the 8 warehouse supervisors to execute the full procure-to-pay cycle without segregation. The $250K fraud incident directly exploited this role. |
| **Root Cause** | The role was designed for operational convenience without SOD analysis. Role permissions were expanded over time without governance. |
| **Business Impact** | Eight warehouse supervisors currently hold the capability to perpetrate a similar fraud. The control weakness has existed since the role was created in 2021. |
| **Recommendation** | Immediately restructure the Warehouse Supervisor role to remove all procurement-related permissions (PO creation, vendor management, invoice processing). Create separate roles for procurement functions that require dual authorization. Implement a mandatory weekly procurement control report reviewed by the CFO. |

---

#### F-011 — System Manager Role Not Segregated from Business Roles

| Attribute | Detail |
|---|---|
| **Risk Rating** | **High** |
| **ISO Reference** | ISO 27001:2022 A.9.2.3 |
| **Description** | Four of seven System Manager users also hold business roles (Finance Manager, Purchase Manager, Stock Manager). This combination allows IT administrators to execute business transactions and then alter system configurations or audit logs to conceal unauthorized activity. |
| **Root Cause** | No policy prohibiting IT staff from holding business roles; historically understaffed IT team personnel performed overlapping functional roles. |
| **Business Impact** | Inability to rely on audit trails for IT staff activity. Potential for undetected fraudulent transactions performed by IT personnel. |
| **Recommendation** | Remove all business roles from IT staff accounts immediately. Create separate "break-glass" accounts for emergency business transactions by IT, with full logging and mandatory periodic review. Implement a formal policy prohibiting business-role assignment to IT administrators. |

---

#### F-012 — Legacy Accounts Combine Finance Manager and Procurement Officer Roles

| Attribute | Detail |
|---|---|
| **Risk Rating** | **High** |
| **ISO Reference** | ISO 27001:2022 A.9.2.5, A.9.2.2 |
| **Description** | Three legacy user accounts (employees hired before 2020) still combine Finance Manager and Procurement Officer role assignments. These employees have changed departments over time but their original role assignments were never adjusted. Combined roles enable cross-functional SOD violations. |
| **Root Cause** | No internal transfer/re-org process that triggers access review and role adjustment. No periodic recertification. |
| **Business Impact** | Three senior employees retain incompatible role combinations that enable unauthorized procure-to-pay transactions. |
| **Recommendation** | Immediately review and restructure the three legacy accounts. Implement an employee transfer/role-change process that triggers an access review within 5 business days of any department change. |

---

#### F-013 — Read-Only Roles Granted Modify Permissions on Sensitive Tables

| Attribute | Detail |
|---|---|
| **Risk Rating** | **High** |
| **ISO Reference** | ISO 27001:2022 A.9.4.1 |
| **Description** | The "Ops Viewer" role, intended for read-only access to operational reports, was found to have write permissions on Stock Ledger Entry and Serial No DocTypes. This affects 22 Operations users who were under the assumption their access was read-only. |
| **Root Cause** | Role was cloned from a standard role with broader permissions; the permission reduction was incomplete. |
| **Business Impact** | 22 users can modify stock records and serial numbers, potentially enabling unauthorized inventory adjustments, asset misappropriation, or data integrity issues in inventory reporting. |
| **Recommendation** | Immediately audit and correct the Ops Viewer role permissions to enforce true read-only access. Perform a comprehensive review of all custom role permissions to ensure they match their intended purpose. |

---

#### F-014 — Inconsistent Role Naming Conventions

| Attribute | Detail |
|---|---|
| **Risk Rating** | **Low** |
| **ISO Reference** | ISO 27001:2022 A.9.1.1 |
| **Description** | Custom roles follow no standardized naming convention. Examples observed include "WH_Sup_Access", "FinCtrl_v2", "PO-Officer-New", "ProcureViewAccess2024". Recommended: FIN_AccountsManager_Full, WH_WarehouseSupervisor_Modify, PROC_ProcurementOfficer_Create. This makes it difficult for auditors, IT staff, and department heads to identify the purpose and scope of each role. |
| **Root Cause** | No role naming policy established; roles created by multiple IT team members over several years without coordination. |
| **Business Impact** | Increased effort for role management and auditing. Higher probability of role mis-assignment. |
| **Recommendation** | Define and enforce a role naming convention (e.g., [Module]_[Function]_[Level]). Rename all 47 custom roles to conform. Include naming convention requirements in any new role request process. |

---

#### F-015 — Orphaned Roles with No Active Users

| Attribute | Detail |
|---|---|
| **Risk Rating** | **Low** |
| **ISO Reference** | ISO 27001:2022 A.9.2.5 |
| **Description** | Nine custom roles have zero active user assignments, including "Legacy_Finance_Admin", "Old_Purchase_Supervisor", "Test_Role_Dev", and six others. These roles remain in the ERPNext role table and could be inadvertently assigned to users. |
| **Root Cause** | No process to remove deprecated roles; no automated cleanup of orphaned role definitions. |
| **Business Impact** | Unnecessary system complexity; risk of assigning obsolete or test roles to users with unpredictable permission consequences. |
| **Recommendation** | Remove all 9 orphaned roles from the system after documenting them for historical reference. Establish a quarterly cleanup process to identify and remove unused roles. |

---

### Domain: Segregation of Duties

---

#### F-016 — Critical SOD Conflict — Vendor Setup and Purchase Order Creation Combined

| Attribute | Detail |
|---|---|
| **Risk Rating** | **High** (design gap with partial compensating controls — see F-017 for compensating control reliability) |
| **ISO Reference** | ISO 27001:2022 A.9.1.1, A.9.4.1 |
| **Description** | Multiple users across Procurement and Warehouse departments hold both Vendor Creator and Purchase Order Creator roles, enabling them to create fictitious vendors and generate purchase orders to those vendors. This combined capability directly mirrors the mechanism used in the $250K fraud incident. While some compensating controls exist (PO approval thresholds), they are insufficient to prevent split-PO circumvention. |
| **Root Cause** | No SOD conflict matrix defined for ERPNext role design. Roles were designed around job convenience rather than control principles. |
| **Business Impact** | Material fraud risk: users can create shell vendors, submit fraudulent purchase orders, and generate false invoices. Estimated fraud potential: $500K+ annually if uncontrolled. |
| **Recommendation** | Immediately remove either Vendor Creator or Purchase Order Creator from the affected users' profiles. Redesign procurement roles to enforce a minimum of two-person approval for all vendor creation and PO transactions. Implement automated real-time SOD conflict detection in ERPNext using custom scripts or third-party tools. |

---

#### F-017 — Critical SOD Conflict — Invoice Approval and Payment Processing Not Segregated

| Attribute | Detail |
|---|---|
| **Risk Rating** | **High** (compensating control inoperative — residual risk is critical). Compensating control CC-02 (weekly payment batch review) has not been performed in 4 months. Until restored, this control cannot be relied upon for risk reduction. |
| **ISO Reference** | ISO 27001:2022 A.9.1.1, A.9.2.3 |
| **Description** | Two Finance users (Maria Santos and Elena Popescu) hold both Invoice Approval and Payment Processing roles, creating a scenario where a single individual can approve a fraudulent invoice and authorize the corresponding payment. The compensating control — weekly payment batch review — has not been performed for 4 months. |
| **Root Cause** | Finance department understaffing led to role consolidation; the compensating control (supervisory review) was relied upon but not enforced. |
| **Business Impact** | Unauthorized payments could be processed without detection. With the compensating control ineffective, the residual risk is critical. |
| **Recommendation** | Immediately separate invoice approval and payment processing roles across three user accounts. Assign approval to the Finance Manager role and payment processing to the Accounts Payable role. Reinstate the weekly payment review with mandatory sign-off and escalation for missed reviews. |

---

### Domain: Monitoring & Oversight

---

#### F-018 — No Audit Log Monitoring for Privileged or Sensitive Operations

| Attribute | Detail |
|---|---|
| **Risk Rating** | **Medium** |
| **ISO Reference** | ISO 27001:2022 A.12.4.1 (Event logging), A.12.4.3 (Administrator and operator logs) |
| **Description** | ERPNext generates audit logs for user activity via the Frappe Activity Log, but these logs are not actively monitored by IT management or the security team. No automated alerts exist for high-risk events such as vendor master changes, journal entries above $10K, payment batch modifications, or privileged role assignments. The procurement fraud went undetected for 8 months despite audit log data being available. |
| **Root Cause** | Security monitoring processes are focused on network infrastructure; no dedicated ERP security monitoring or SIEM integration. |
| **Business Impact** | Fraud and unauthorized activity can persist for extended periods without detection. The $250K fraud could have been identified earlier with active log monitoring. Estimated detection window: 8+ months (as demonstrated by the incident). |
| **Recommendation** | Implement daily review of ERPNext activity logs for high-risk events. Configure automated alerts for (1) vendor master creation or modification, (2) journal entries above $5K, (3) payment batch modifications, (4) new user creation with privileged roles, (5) failed login attempts exceeding threshold. Consider integrating ERPNext logs with a SIEM platform (e.g., Wazuh, Splunk) for real-time correlation and alerting. |

---

## Cross-Reference Index

| Finding ID | Document | Section |
|---|---|---|
| F-001 to F-008 | 02-User-Access-Provisioning.md | Sample Testing, Termination Testing |
| F-009 to F-015 | 03-Role-Assignment-Review.md | Findings — Role Assignment Issues |
| F-016 | 04-SOD-Conflict-Matrix.md | User-Level Conflicts (SOD-01) |
| F-017 | 04-SOD-Conflict-Matrix.md | User-Level Conflicts (SOD-05) |
| F-018 | N/A (Discussed in 04-SOD-Conflict-Matrix.md Compensating Controls) | |

---

## Distribution

| Stakeholder | Purpose |
|---|---|
| Board of Directors | Oversight and governance |
| Audit Committee | Risk acceptance decisions |
| CFO / Finance Director | Remediation ownership for finance findings |
| COO / Operations Director | Remediation ownership for operations findings |
| IT Director | Remediation ownership for technical findings |
| Head of Internal Audit | Tracking and verification of remediation |
