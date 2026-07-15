# 02 — User Access Provisioning Review

**Audit Area:** Access Request, Authorization, Provisioning, and Deprovisioning
**Sample Size:** 25 access requests tested
**Termination Tests:** 5 terminated users
**Standards:** ISO 27001:2022 A.9.2 (User Access Provisioning), A.9.3 (User Responsibilities)

---

## Access Request Process Description

AeroLogistics Solutions uses a semi-formal process for granting ERPNext access. The process is documented in the IT Security Policy (ISP-017) but is not consistently enforced:

1. **Request Initiation** — The hiring manager or department head submits an Access Request Form (paper-based or email) to the IT Service Desk
2. **Department Head Approval** — The request must be approved by the requester's department head (no secondary approval required for cross-module access)
3. **IT Provisioning** — The IT support team creates the user account in ERPNext and assigns role profiles based on the request form
4. **Notification** — IT notifies the requestor upon completion; no formal confirmation of access accuracy is obtained from the department head post-provisioning
5. **Periodic Review** — Access recertification is performed on an ad hoc basis; no annual recertification cycle exists

---

## Sample Testing — Access Request Verification

A statistical sample of 25 access requests was selected from 147 access request records logged between January 2025 and June 2026. Each request was tested for:
- Evidence of authorized approval (signed form or email approval)
- Access granted matches requested access (role and module scope)
- Timely provisioning (within 5 business days of approval)
- Proper documentation retention

### Test Results Table

| Req ID | User | Requested Access | Approved By | Actual Access Granted | Basis of Testing | Finding |
|---|---|---|---|---|---|---|
| AR-001 | Robert Chen | Procurement + Warehouse roles | Warehouse Manager | Procurement Officer + Warehouse Supervisor + Vendor Creator | Role mismatch — extra role (Vendor Creator) not requested | F-001 |
| AR-002 | Maria Santos | Accounts Payable role | Finance Manager | Accounts Payable + Payment Approver | Role mismatch — extra role assigned | F-002 |
| AR-003 | James Okonkwo | Operations Analyst role | Operations Director | Operations Analyst (correct) | Matched | Pass |
| AR-004 | Priya Sharma | Accountant role | Finance Manager | Accountant (correct) | Matched | Pass |
| AR-005 | David Kim | IT Support role | IT Director | IT Support + System Manager (read-only) | Extra role assigned; no approval for SysAdmin access | F-003 |
| AR-006 | Lisa Thompson | Procurement Officer role | Procurement Manager | Procurement Officer + Buyer | Extra role assigned, not requested | F-001 |
| AR-007 | Ahmed Hassan | Inventory Clerk role | Warehouse Manager | Inventory Clerk (correct) | Matched | Pass |
| AR-008 | Elena Popescu | Finance Controller role | CFO | Finance Controller (correct) | Matched | Pass |
| AR-009 | Carlos Mendez | Procurement Manager role | Procurement Manager | Procurement Manager + Vendor Creator | Extra permission (vendor master create) not approved | F-001 |
| AR-010 | Yuki Tanaka | Sales Analyst role | Sales Director | Sales Analyst (correct) | Matched | Pass |
| AR-011 | Sarah Johnson | HR Executive role | HR Director | HR Executive (correct) | Matched | Pass |
| AR-012 | Michael Brown | Accounts Receivable role | Finance Manager | Accounts Receivable + Accountant (modify) | Extra role; no seg approval for dual accounting roles | F-002 |
| AR-013 | Laura Davis | Logistics Coordinator role | Operations Director | Logistics Coordinator (correct) | Matched | Pass |
| AR-014 | Kevin Wilson | Buyer role | Procurement Manager | Buyer + Procurement Officer (modify) | Higher privilege level than requested | F-004 |
| AR-015 | Angela Martinez | Fleet Supervisor role | Fleet Manager | Fleet Supervisor (correct) | Matched | Pass |
| AR-016 | Thomas Lee | Payroll Officer role | HR Director | Payroll Officer (correct) | Matched | Pass |
| AR-017 | Rachel Green | Customer Account Manager role | Sales Director | Customer Account Manager + Sales Representative | Extra role assigned | F-002 |
| AR-018 | Daniel Taylor | Warehouse Supervisor role | Warehouse Manager | Warehouse Supervisor (correct) | Matched | Pass |
| AR-019 | Emily White | Accountant role | Finance Manager | Accountant + Journal Entry Creator | Extra permission (journal entry create) not requested | F-005 |
| AR-020 | Christopher Hall | Procurement Officer role | Procurement Manager | Procurement Officer (correct) | Matched | Pass |
| AR-021 | Amanda Scott | Operations Analyst role | Operations Director | Operations Analyst + Inventory Adjustor | Extra role (inventory adjustments) not per request | F-004 |
| AR-022 | Brian Adams | IT Support role | IT Director | IT Support (correct) | Matched | Pass |
| AR-023 | Nicole Baker | HR Executive role | HR Director | HR Executive (correct) | Matched | Pass |
| AR-024 | Jason Wright | Accounts Payable role | Finance Manager | Accounts Payable (correct) | Matched | Pass |
| AR-025 | Stephanie Hill | Logistics Coordinator role | Operations Director | Logistics Coordinator (correct) | Matched | Pass |

**Result:** 10 of 25 samples (40%) showed control exceptions where users received greater access than approved.

---

## Access Recertification Review

### Recertification History

| Recertification Event | Date | Scope | Completeness | Outcome |
|---|---|---|---|---|
| Planned Annual Recertification | N/A | All users | Never performed | No formal recertification program exists |
| Ad hoc Review (Post-Incident) | Dec 2025 | Finance & Procurement only | Partial — 43 of 214 users reviewed | 5 inappropriate access rights identified but not corrected until June 2026 |
| IT Self-Review (User List) | Mar 2026 | All users (system-level) | User list exported but no role verification | No action taken |

### Recertification Findings

- No scheduled recertification cycle exists — ISO 27001:2022 A.9.2.5 requires periodic review of user access rights
- The post-incident review in Dec 2025 covered only 20% of the user population
- No evidence that recertification results were formally reported to management
- No process exists for managers to certify access appropriateness for their direct reports

---

## Termination / Deactivation Testing

Five terminated employees were selected from the HR termination log (18-month period: Jan 2025 – Jun 2026) to test the timeliness of ERPNext account deactivation.

### Terminated User Test Results

| Terminated User | Termination Date | Department | Account Deactivation Date | Days to Deactivate | Finding |
|---|---|---|---|---|---|
| John Matthews | Feb 15, 2025 | Sales | Feb 28, 2025 | 13 days | F-006 |
| Patricia Nguyen | Apr 3, 2025 | Procurement | Not deactivated (active as of audit date) | 458+ days | F-007 |
| Steven Garcia | Aug 20, 2025 | Operations | Sep 5, 2025 | 16 days | F-006 |
| Rebecca Turner | Nov 1, 2025 | Finance | Not deactivated (active as of audit date) | 261+ days | F-007 |
| Mark Anderson | Mar 10, 2026 | Warehouse | Mar 28, 2026 | 18 days | F-006 |

**Result:** 2 of 5 terminated users (40%) had accounts that were never deactivated. The remaining 3 users experienced delays averaging 15.7 days. No formal termination checklist exists requiring IT to deactivate ERP access.

---

## Findings Summary (Access Provisioning)

| Finding ID | Title | Risk Rating | ISO Reference |
|---|---|---|---|
| F-001 | Over-Provisioning of Access — Users Receive Unapproved Roles | High | A.9.2.1, A.9.2.3 |
| F-002 | Role Scope Creep — Multiple Extra Roles Assigned Without Approval | Medium | A.9.2.1 |
| F-003 | Privileged Access Granted Without Authorization | Critical | A.9.2.3 |
| F-004 | Higher Privilege Level Than Job Function Requires | High | A.9.2.1, A.9.2.3 |
| F-005 | Financial Transaction Permissions Assigned Without Approval | High | A.9.2.1, A.9.4.1 |
| F-006 | Delayed Account Deactivation After Employee Termination | Medium | A.9.2.6 |
| F-007 | Terminated Employee Accounts Remain Active | Critical | A.9.2.6 |
| F-008 | No Formal Access Recertification Program | High | A.9.2.5 |
