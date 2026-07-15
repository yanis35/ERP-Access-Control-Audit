# 03 — Role Assignment Review

**Audit Area:** Role-Based Access Control (RBAC) Design and Role Assignment Practices
**Roles Analyzed:** 87 (40 standard ERPNext + 47 custom roles)
**Standards:** ISO 27001:2022 A.9.2.2, A.9.2.3, A.9.2.5

---

## ERPNext Role Hierarchy Overview

ERPNext uses a role-based permission system where users are assigned one or more roles, and each role is granted specific permissions (read, write, create, delete, submit, cancel, amend) on individual DocTypes (data tables/modules). The system includes pre-defined roles and supports unlimited custom role creation.

### Role Categories

| Category | Count | Examples |
|---|---|---|
| **Standard ERPNext Roles** | 40 | System Manager, Accounts Manager, Purchase Manager, Stock Manager, HR Manager, Employee |
| **Pre-Defined Transaction Roles** | 18 | Purchase Officer, Sales User, Accounts User, Stock User, HR User |
| **Custom Roles (AeroLogistics)** | 47 | Finance Controller (custom), Procurement Analyst (custom), Vendor Creator (custom), Ops Viewer (custom) |
| **Privileged Roles** | 5 | System Manager, Administrator (DB), HR Admin, All Email (notification access), Support Team |

### Pre-Defined vs Custom Roles

ERPNext ships with granular roles designed around functional modules. AeroLogistics created 47 custom roles largely by cloning standard roles and modifying permission sets. This has resulted in significant role proliferation and inconsistent permission boundaries.

| Aspect | Pre-Defined Roles | Custom Roles |
|---|---|---|
| Source | ERPNext standard distribution | Created by AeroLogistics IT (2019–2026) |
| Naming Convention | Consistent (Module_Role) | Inconsistent (mix of acronyms, job titles, module names) |
| Permission Levels | Well-defined, tested | Varies widely; many cloned without review |
| Documentation | Documented in ERPNext guides | Minimal or no documentation |
| Users Assigned | ~1,200 role assignments | ~650 role assignments |
| Maintenance | Updated with ERPNext patches | Manual; no update process |

---

## Critical Role Analysis

### Role Inventory

| Role Name | Type | Users Assigned | Permission Scope | Risk Level |
|---|---|---|---|---|
| System Manager | Standard | 7 | Full system access (all DocTypes, all rights) | **Critical** |
| Accounts Manager | Standard | 4 | Full finance module: journal entries, payments, GL, budgeting | **Critical** |
| Purchase Manager | Standard | 3 | Full procurement: supplier master, purchase orders, contracts | **Critical** |
| Stock Manager | Standard | 5 | Full inventory: stock transactions, warehouse management, serial numbers | **High** |
| Finance Controller (Custom) | Custom | 2 | Accounts + budgeting + period closing + financial reports | **Critical** |
| Vendor Creator (Custom) | Custom | 14 | Create/modify supplier master records | **High** |
| Warehouse Supervisor (Custom) | Custom | 8 | Stock transfers, inventory adjustments, goods receipt, PO creation | **High** |
| Procurement Officer (Custom) | Custom | 11 | Purchase order creation, supplier communication, contract management | **High** |
| Payment Approver (Custom) | Custom | 3 | Payment entry approval, bank transaction authorization | **Critical** |
| Ops Viewer (Custom) | Custom | 22 | Read-only access to operations data (reports, dashboards) | **Low** |
| Accounts Payable User | Standard | 9 | Invoice processing, payment scheduling, supplier reconciliation | **High** |
| HR Manager | Standard | 2 | Employee records, payroll processing, leave management | **High** |

### Role Mapping to Job Functions

| Job Function | Assigned Role(s) | Appropriate? | Gap |
|---|---|---|---|
| Finance Manager | Accounts Manager + Finance Controller (Custom) | Over-permissioned | Dual role includes period closing + payment approval |
| Procurement Officer | Procurement Officer + Vendor Creator | Over-permissioned | Can create vendors and purchase orders (SOD conflict) |
| Warehouse Supervisor | Warehouse Supervisor + Stock User + Purchase User | Over-permissioned | Can create POs and receive goods |
| Accountant (AP) | Accounts Payable User + Payment Approver | Over-permissioned | Can process invoices and approve payments |
| System Administrator | System Manager + All Email | Appropriate | But no segregation from business roles |
| Sales Representative | Sales User | Appropriate | Standard, tested role |
| HR Executive | HR User | Appropriate | Limited to HR transactions |

---

## Findings — Role Assignment Issues

### F-009: Role Proliferation — 47 Custom Roles with Minimal Differentiation

**Rating:** Medium  
**ISO Reference:** A.9.2.2  
**Description:** Analysis of the 47 custom roles revealed that 28 roles (60%) are near-duplicates of standard ERPNext roles with only minor permission differences (e.g., "Procurement Analyst (Custom)" differs from "Purchase User (Standard)" by only two additional read permissions). Numerous roles have overlapping permission sets, making the role matrix confusing to maintain.  
**Root Cause:** No role governance process; roles created on-demand by IT without approval from a role management committee.  
**Business Impact:** Increased administrative overhead, higher risk of mis-assignment, difficult to audit.

### F-010: Warehouse Supervisor Role Over-Permissioned with Procurement Rights

**Rating:** Critical  
**ISO Reference:** A.9.2.2, A.9.2.3  
**Description:** The custom Warehouse Supervisor role includes permissions to create purchase orders, receive goods against POs, and create supplier invoices — all procurement functions that should be segregated from warehouse operations. The fraud incident exploited this exact control weakness.  
**Root Cause:** The role was originally designed for a "super-user" warehouse lead with no consideration for SOD principles.  
**Business Impact:** Directly enabled the $250K procurement fraud. Warehouse staff can perpetrate procure-to-pay fraud without detection.

### F-011: System Administrator Role Not Segregated from Business Roles

**Rating:** High  
**ISO Reference:** A.9.2.3  
**Description:** Four of seven System Manager users also hold business roles (e.g., Finance Manager, Purchase Manager) on the same account. This allows IT administrators to execute business transactions and then alter audit logs or modify system settings to conceal their actions.  
**Root Cause:** No policy prohibiting IT staff from holding business roles; IT was historically understaffed and wore multiple hats.  
**Business Impact:** Loss of audit trail integrity; IT staff can bypass controls on financial transactions.

### F-012: Legacy Accounts Combine Finance Manager and Procurement Officer Roles

**Rating:** High  
**ISO Reference:** A.9.2.5, A.9.2.2  
**Description:** Three legacy user accounts (pre-2020) still combine Finance Manager and Procurement Officer role assignments. These accounts belong to senior employees who changed departments but retained their original role assignments.  
**Root Cause:** No recertification program to review and clean up role assignments after role changes or promotions.  
**Business Impact:** Users with dual finance-procurement roles can execute incompatible transactions, increasing fraud risk.

### F-013: Read-Only Roles Granted Modify Permissions on Sensitive Tables

**Rating:** High  
**ISO Reference:** A.9.2.2, A.9.4.1  
**Description:** The "Ops Viewer (Custom)" role, intended for read-only operational reporting, was found to have write permissions on the "Stock Ledger Entry" and "Serial No" DocTypes. This affects 22 users who were assumed to have read-only access.  
**Root Cause:** Improper permission configuration when the role was cloned from a standard role; permissions were not stripped down to read-only for all DocTypes.  
**Business Impact:** Unauthorized inventory adjustments possible; 22 users with assumed read-only access can modify stock records.

### F-014: Inconsistent Role Naming Conventions

**Rating:** Low  
**ISO Reference:** A.9.2.2  
**Description:** Custom roles follow no naming standard. Examples include "WH_Sup_Access", "FinCtrl_v2", "PO-Officer-New", and "ProcureViewAccess2024". This makes role identification, audit, and management unnecessarily difficult.  
**Root Cause:** No role naming policy; roles created by multiple IT staff over several years.  
**Business Impact:** Increased risk of role mis-assignment and reduced auditability.

### F-015: Orphaned Roles with No Active Users

**Rating:** Low  
**ISO Reference:** A.9.2.2  
**Description:** 9 custom roles have zero active user assignments, including "Legacy_Finance_Admin", "Old_Purchase_Supervisor", and "Test_Role_Dev". These roles remain in the system and could be assigned in error.  
**Root Cause:** No process for removing deprecated roles after role redesign or user migration.  
**Business Impact:** Unnecessary complexity in role matrix; risk of unintended permission grants if assigned.

---

## Role Assignment — Summary Statistics

| Metric | Count |
|---|---|
| Total Roles | 87 |
| Active Roles (with ≥1 user) | 78 |
| Orphaned Roles (0 users) | 9 |
| Custom Roles with Nearly Identical Permissions | 28 (of 47 custom) |
| Users with Privileged Role Combinations | 12 |
| Role Assignments Needing Review (User-Role Mappings) | 1,850+ |
| Critical Roles Needing Immediate SOD Review | 5 |
