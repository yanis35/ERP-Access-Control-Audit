# 04 — Segregation of Duties Conflict Matrix

**Audit Area:** Incompatible Role Combinations in Finance, Procurement, and Operations
**Users Analyzed:** 214  
**SOD Conflict Pairs Defined:** 12  
**User-Level Conflicts Detected:** 10  
**Standards:** ISO 27001:2022 A.9.1.1, A.9.2.3, A.9.4.1; ITGC General IT Controls

---

## SOD Conflict Definition Matrix

Segregation of duties conflicts were defined based on the principle that no single user should hold roles that enable them to execute and conceal unauthorized transactions across the procure-to-pay, order-to-cash, and financial reporting cycles. The following incompatible pair combinations were defined for the ERPNext environment:

### Procure-to-Pay Cycle Conflicts

| Pair ID | Role A | Role B | Incompatible Description | Risk |
|---|---|---|---|---|
| SOD-01 | Vendor Creator | Purchase Order Creator/Approver | User can create a fake vendor and then create POs to that vendor | **Critical** |
| SOD-02 | Purchase Order Creator | Goods Receipt Creator | User can create a PO and then receive goods against it (phantom receipt) | **High** |
| SOD-03 | Purchase Order Approver | Invoice Creator | User can approve their own PO and then create the related supplier invoice | **High** |
| SOD-04 | Invoice Creator | Invoice Approver | User can create an invoice and approve it without independent review | **Critical** |
| SOD-05 | Invoice Approver | Payment Processor | User can approve an invoice and then authorize payment to themselves / fake vendor | **Critical** |

### Financial Reporting Cycle Conflicts

| Pair ID | Role A | Role B | Incompatible Description | Risk |
|---|---|---|---|---|
| SOD-06 | Journal Entry Creator | Journal Entry Approver | User can create and approve journal entries without independent check | **High** |
| SOD-07 | Journal Entry Creator | Bank Reconciliation User | User can create adjusting entries and reconcile bank accounts to conceal discrepancies | **High** |
| SOD-08 | Payment Entry Creator | Bank Reconciliation User | User can create payments and manipulate bank reconciliations to hide fraud | **High** |

### Order-to-Cash Cycle Conflicts

| Pair ID | Role A | Role B | Incompatible Description | Risk |
|---|---|---|---|---|
| SOD-09 | Sales Order Creator | Credit Note Approver | User can create sales orders and approve credit notes, enabling revenue manipulation | **Medium** |
| SOD-10 | Customer Master Editor | Sales Order Creator | User can create fake customers and sell to them at unauthorized terms | **Medium** |

### User Administration Conflicts

| Pair ID | Role A | Role B | Incompatible Description | Risk |
|---|---|---|---|---|
| SOD-11 | User Creator | Role Assigner | User can create new accounts and grant themselves additional privileges | **Critical** |
| SOD-12 | System Manager | Business Role Holder | User has full system administration rights while also holding business transaction roles | **Critical** |

---

## User-Level SOD Conflict Analysis

All 214 users were analyzed against the defined incompatible pair matrix. The following users were identified with one or more SOD conflicts:

### High-Risk Conflicts

| User | Department | Conflicting Roles | Pair IDs | Modules Affected | Risk | Compensating Controls | Residual Risk |
|---|---|---|---|---|---|---|---|
| **Robert Chen** | Warehouse | Warehouse Supervisor + Vendor Creator + Purchase Order Creator | SOD-01, SOD-02 | Procurement / Warehouse | **Critical** | None in place. This is the exact combination exploited in the fraud incident. | **Critical** |
| **Maria Santos** | Finance | Invoice Approver + Accounts Payable User (Payment Processor) | SOD-05 | Finance / Accounts Payable | **Critical** | Weekly supervisor review of payment batch reports. However, Maria's supervisor has not performed a detailed review in 4 months. | **High** |
| **James Okonkwo** | Operations | Purchase Order Creator + Goods Receipt Creator | SOD-02 | Procurement / Warehouse | **High** | Goods receipt requires warehouse confirmation email. However, James can create the confirmation himself. | **High** |
| **Priya Sharma** | Finance | Journal Entry Creator + Bank Reconciliation User | SOD-07 | Finance / Treasury | **High** | Monthly financial controller review of GL entries over $10K. No review of entries below $10K. | **Medium** |
| **David Kim** | IT | System Manager + Purchase Manager | SOD-12 | IT / Procurement | **Critical** | No compensating controls. IT policy does not prohibit business role assignments. | **Critical** |
| **Lisa Thompson** | Procurement | Vendor Creator + Purchase Order Creator | SOD-01 | Procurement | **Critical** | Purchase orders over $5K require manager approval. Lisa's PO creation limit is $4,999. | **High** (limit can be circumvented via split POs) |
| **Ahmed Hassan** | Warehouse | Goods Receipt Creator + Inventory Adjustor | SOD-02 (partial) | Warehouse / Inventory | **High** | None. Inventory adjustments are logged but not reviewed. | **High** |
| **Elena Popescu** | Finance | Invoice Approver + Payment Processor | SOD-05 | Accounts Payable | **Critical** | No compensating controls. Elena is the senior finance controller but also processes payments. | **Critical** |
| **Carlos Mendez** | Procurement | Vendor Creator + Purchase Order Approver | SOD-01 | Procurement | **High** | Purchase orders require CFO approval over $10K. However, Carlos can approve POs under $10K. | **Medium** |
| **Yuki Tanaka** | Sales | Sales Order Creator + Credit Note Approver | SOD-09 | Sales / Revenue | **Medium** | Credit notes over $1K require sales director approval. Yuki can approve credit notes under $1K. | **Low** |

### Conflict Summary by Department

| Department | Users Analyzed | Users with Conflicts | High/Critical Conflicts |
|---|---|---|---|
| Finance & Accounts | 28 | 3 | 3 |
| Procurement | 15 | 2 | 2 |
| Warehouse & Logistics | 42 | 2 | 2 |
| Operations | 53 | 1 | 1 |
| Sales & Marketing | 36 | 1 | 0 |
| IT / System Administration | 17 | 1 | 1 |
| Human Resources | 12 | 0 | 0 |
| Executive | 11 | 0 | 0 |
| **Total** | **214** | **10** | **9** |

---

## Compensating Controls Assessment

### Existing Compensating Control Inventory

| Control ID | Control Description | Users Affected | Adequacy Rating |
|---|---|---|---|
| CC-01 | Weekly payment batch review by Finance Director | Maria Santos | **Inadequate** — Reviews are not being performed regularly (4-month gap) |
| CC-02 | Monthly GL entry review (entries > $10K threshold) | Priya Sharma | **Partial** — Entries under $10K not reviewed; threshold can be exploited |
| CC-03 | Purchase order approval required for orders > $5K | Lisa Thompson | **Partial** — PO splitting (multiple POs under $5K) is not prevented |
| CC-04 | CFO approval required for POs > $10K | Carlos Mendez | **Partial** — $10K threshold may be circumvented; no automated enforcement |
| CC-05 | Credit note approval threshold ($1K) with director sign-off | Yuki Tanaka | **Adequate** — Tolerable residual risk with quarterly monitoring |
| CC-06 | Audit logging for all procurement transactions | All | **Inadequate** — Logs are collected but not reviewed by management |
| CC-07 | Segregation of IT and business roles policy | David Kim | **Inadequate** — Policy does not exist; no compensating controls in place |

### Compensating Control Recommendations

1. **Implement transaction-level approval workflows** in ERPNext for all POs, invoices, and journal entries above defined materiality thresholds
2. **Deploy automated SOD monitoring** using ERPNext permission reporting or third-party tools to flag new conflicting role assignments in real time
3. **Establish a weekly Financial Control Review** where the CFO or delegated authority reviews payment batches, new vendor additions, and journal entries above $2K
4. **Implement dual authorization** for vendor master changes (any create/modify of supplier records requires secondary approval)
5. **Enforce a clean desk policy** for all roles with critical permissions — no single user should hold both execution and approval rights for the same transaction type

---

## Initial Risk Assessment

| Risk Level | Count | Action Required |
|---|---|---|
| **Critical** — Immediate remediation required | 3 conflicts | Disable conflicting role combinations within 7 days; implement compensating controls |
| **High** — Remediation within 30 days | 4 conflicts | Restructure role assignments; implement monitoring controls |
| **Medium** — Remediation within 60 days | 2 conflicts | Strengthen compensating controls; implement threshold monitoring |
| **Low** — Acceptable with monitoring | 1 conflict | Continue periodic review |

---

## ERPNext-Specific SOD Considerations

ERPNext does not natively enforce segregation of duties at the role assignment level. The following technical limitations were noted:

- No built-in conflict detection when assigning roles to users
- No workflow enforcement preventing a user from holding two conflicting roles
- Permission inheritance through role nesting is opaque — roles can inherit permissions from other roles, making it difficult to determine effective permissions
- Audit trail logs exist in Frappe's "Activity Log" but are not designed for SOD monitoring
- Custom scripts or third-party tools are required for automated SOD monitoring
