# azure-governance-security-lab
Azure governance with RBAC, Azure Policy, and cost management budgets

# Azure Governance & Security Hardening Lab

## Overview
Implemented Azure governance controls including Role-Based Access Control (RBAC), Azure Policy enforcement, and cost management budgets. This lab demonstrates enterprise security practices for managing cloud access and preventing configuration drift.

## What I Built

**Governance Components:**
- Resource Group: `rg-lab05-gov-marco`
- Test User: `junior-dev-marco` with Reader role (view-only access)
- Azure Policy: Restricted VM sizes to Standard_B1s and Standard_B1ms only
- Budget Alert: $50 monthly budget with 80% threshold notification

## Implementation Steps

### Phase 1: Resource Group Setup
```bash
Created resource group: rg-lab05-gov-marco
Region: Central US
Purpose: Isolated environment for governance testing
```

### Phase 2: Role-Based Access Control (RBAC)

**Created Test User:**
- Navigated to Microsoft Entra ID (Azure Active Directory)
- Created user: junior-dev-marco
- Display name: Junior Developer

**Assigned Reader Role:**
- Scope: rg-lab05-gov-marco resource group only
- Role: Reader (view all resources, cannot modify)
- Method: Access Control (IAM) role assignment

**Result:** User can view resources but cannot create, modify, or delete anything in the resource group.

### Phase 3: Azure Policy Implementation

**Policy Assignment:**
- Policy: "Allowed virtual machine size SKUs"
- Scope: rg-lab05-gov-marco
- Assignment name: Restrict-VM-Sizes

**Parameters:**
- Allowed sizes: Standard_B1s, Standard_B1ms only
- All other VM sizes blocked (D-series, F-series, etc.)

**Enforcement:**
- Prevents deployment of expensive VM sizes
- Applies to all users including administrators
- Policy evaluation occurs during resource creation

**Note:** Azure Policy takes 10-15 minutes to propagate after creation.

### Phase 4: Cost Management

**Budget Configuration:**
- Name: Monthly-Lab-Budget
- Amount: $50
- Reset period: Monthly
- Duration: 1 year

**Alert Settings:**
- Type: Actual spend (not forecasted)
- Threshold: 80% ($40)
- Notification: Email alert to administrator

## Real-World Enterprise Applications

### Role-Based Access Control
**Problem:** Granting full access to all team members creates security risks and compliance violations.

**Solution:** RBAC provides granular permission management:
- Developers get Contributor access to dev environments only
- Operations teams get Reader access to production for troubleshooting
- Finance teams get Cost Management Reader for billing visibility
- External contractors get time-limited, scope-restricted access

**Enterprise Example:**
A company with 500 employees might have:
- 50 Azure AD groups (by department/role)
- 20 custom roles for specific job functions
- Permissions assigned at management group level (cascades to all subscriptions)

### Azure Policy Enforcement
**Problem:** Without guardrails, users can deploy non-compliant or expensive resources causing security risks and cost overruns.

**Solution:** Azure Policy enforces organizational standards:
- **Compliance:** Require encryption on all storage accounts (HIPAA, PCI-DSS)
- **Cost Control:** Block expensive VM SKUs in non-production environments
- **Security:** Enforce network security groups on all subnets
- **Naming Standards:** Require resources follow naming conventions
- **Region Restrictions:** Prevent deployment in non-approved regions (data sovereignty)

**Enterprise Example:**
Financial services company enforces 50+ policies including:
- All data must stay in US regions (regulatory compliance)
- All VMs must have backup enabled (disaster recovery)
- Public IP addresses denied in production (security)
- Tags required for cost allocation (chargeback)

### Cost Management & Budgets
**Problem:** Cloud costs can spiral out of control without monitoring and alerts.

**Solution:** Proactive budget management:
- Department-level budgets with automatic alerts
- Action groups trigger automation (e.g., scale down resources at 90% threshold)
- Regular cost reviews prevent surprise bills
- Showback/chargeback to business units

**Enterprise Example:**
E-commerce company sets:
- $100K/month overall budget
- $20K per business unit
- Alerts at 70%, 90%, 100% thresholds
- Auto-shutdown of dev/test VMs at 100%

## Production Best Practices

### Group-Based Access Management
In production environments, permissions should be assigned to Azure AD groups rather than individual users:

**Why Groups?**
- Add/remove users from groups instead of managing individual role assignments
- Consistent permissions across team members in same role
- Easier auditing (one group to review vs. hundreds of individual assignments)
- Automated provisioning when integrated with HR systems

**Example Structure:**
```
Azure AD Groups:
├── Cloud-Admins (Owner role on subscription)
├── Network-Engineers (Network Contributor on hub VNet)
├── App-Developers-Prod (Contributor on prod resource groups)
├── App-Developers-Dev (Owner on dev resource groups)
└── Finance-Viewers (Cost Management Reader on subscription)
```

### Policy Inheritance & Management Groups
Production environments use management group hierarchies:
```
Root Management Group
├── Production (strict policies)
│   ├── Subscription A
│   └── Subscription B
└── Non-Production (relaxed policies)
    ├── Dev Subscription
    └── Test Subscription
```

Policies assigned at management group level cascade to all child subscriptions and resource groups.

### Budget Automation
Enterprise budgets integrate with automation:
- 70% threshold: Email notification
- 90% threshold: Webhook triggers Azure Function to scale down resources
- 100% threshold: Auto-shutdown of non-critical workloads

## Skills Demonstrated
- Role-Based Access Control (RBAC) configuration
- Azure Policy creation and assignment
- Cost management and budget alerts
- Microsoft Entra ID user management
- Access control best practices
- Governance framework implementation
- Compliance enforcement through policy
- Security hardening methodology

## Technologies Used
Microsoft Entra ID | Azure RBAC | Azure Policy | Cost Management | Access Control (IAM)

## Key Learnings

**Principle of Least Privilege:**
Always grant minimum permissions required for job function. Reader role for viewing, Contributor for management, Owner only when subscription-level control needed.

**Policy as Guardrails:**
Azure Policy prevents mistakes before they happen. Better to block non-compliant deployments than remediate after the fact.

**Proactive Cost Management:**
Budget alerts prevent surprise bills. Set thresholds based on historical spending patterns with buffer for growth.

**Governance at Scale:**
Use groups and management group hierarchies instead of managing individual user permissions. This approach scales from 10 users to 10,000 users.

---

**Marco Asad** - Cloud Engineer  
[LinkedIn](https://www.linkedin.com/in/marco-asad-6a56a03ab/) | [GitHub](https://github.com/marco28asad)
