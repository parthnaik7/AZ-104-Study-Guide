## Domain 1: Manage Azure Identities and Governance (Questions 1 - 15)

### Q1. [EASY] Microsoft Entra ID Free vs Premium
**Scenario:** Your company wants to enforce Multi-Factor Authentication (MFA) conditional access policies based on user location and device state. You currently use Microsoft Entra ID Free. Which of the following is true?

**Options:**
- A) You can enable Conditional Access using Microsoft Entra ID Free as long as you are an independent consultant.
- B) You must upgrade to Microsoft Entra ID P1 or P2 to use Conditional Access.
- C) You can enable Conditional Access with Microsoft Entra ID Free by applying an Azure Policy.
- D) You must purchase Microsoft Entra ID Domain Services.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** Conditional Access policies require at least Microsoft Entra ID P1. Microsoft Entra ID Free only provides basic security features (like security defaults) but does not support granular Conditional Access policies. 
</details>

### Q2. [MEDIUM] Custom RBAC Roles
**Scenario:** You need to create a custom Role-Based Access Control (RBAC) role that allows users to start and stop Virtual Machines but denies them the ability to delete Virtual Machines. Under which section of the custom role definition should you place the `Microsoft.Compute/virtualMachines/delete` action?

**Options:**
- A) Actions
- B) NotDataActions
- C) NotActions
- D) AssignableScopes

<details>
<summary><b>Show Answer</b></summary>

**Answer:** C

**Explanation:** In Azure RBAC custom role definitions, the `NotActions` array is used to specify the operations that are subtracted or excluded from the operations allowed in the `Actions` array. 
</details>

### Q3. [MEDIUM] Azure Policy Evaluation
**Scenario:** You have an Azure Policy applied at the Management Group level that strictly enforces the location to be `UK South` for all resources. A subscription under this Management Group has a Resource Group deployed in `UK West`. What happens when a user tries to deploy a Virtual Machine in the `UK West` Resource Group using the `UK West` location?

**Options:**
- A) The deployment succeeds because the Resource Group is already in UK West.
- B) The deployment fails because Azure Policy inherits downward and prevents non-compliant resources.
- C) The deployment partially succeeds but flags a warning in Azure Advisor.
- D) The deployment succeeds because subscription-level locks override Management Group policies.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** Azure Policy assignments inherit strongly downward from the scope they are applied to. A policy at the Management Group level affects all subscriptions beneath it. The deployment will actively be blocked. 
</details>

### Q4. [HARD] Self-Service Password Reset (SSPR)
**Scenario:** You have 500 users synced from on-premises Active Directory to Microsoft Entra ID using Azure AD Connect. You want to allow users to reset their passwords from the cloud and have those changes write back to on-premises AD. What must you configure?

**Options:**
- A) Enable Cloud Authentication and disable Password Hash Sync.
- B) Enable SSPR in Microsoft Entra ID, enable Password Writeback in Azure AD Connect, and ensure Entra ID P1/P2 licensing.
- C) Give all users the User Administrator role in Entra ID.
- D) Update the domain schema to support Entra ID Pass-through Authentication.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** Password Writeback is required to sync passwords reset in the cloud back to on-premises AD. This requires Azure AD Connect to be configured for Password Writeback and requires at least Entra ID P1 licensing. 
</details>

### Q5. [EASY] Resource Locks
**Scenario:** You apply a `ReadOnly` lock to a Resource Group containing a SQL Database and a Storage Account. A user with the "Owner" role at the subscription level attempts to upload a new blob to the Storage Account. What happens?

**Options:**
- A) The upload fails because the Resource Group is strictly read-only.
- B) The upload succeeds because locks only prevent control plane operations (like deleting or modifying the account), not data plane operations (like uploading a blob).
- C) The upload succeeds because the "Owner" role bypasses resource locks.
- D) The upload fails because an Owner cannot modify storage environments.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** Resource locks operate on the Azure Resource Manager (control plane) level. They prevent deletion or modification of the *resource itself* (e.g., resizing a VM). They do not prevent data operations (data plane) like reading/writing SQL data or uploading blobs, unless the specific resource provider implements it differently (Storage does not block blobs via ReadOnly lock, but it might block key access). *Note: A read-only lock actually prevents the listing of access keys for storage, which can prevent some portal upload methods, but direct data plane access via SAS/Entra works.*
</details>

### Q6. [MEDIUM] Hybrid Identity - Pass-through Authentication
**Scenario:** Your organization has strict security requirements ensuring passwords must NEVER be synchronized or stored in the cloud, even in hashed form. You need seamless single sign-on. Which Microsoft Entra ID authentication method should you use?

**Options:**
- A) Password Hash Synchronization (PHS)
- B) Active Directory Federation Services (AD FS) or Pass-through Authentication (PTA)
- C) Azure AD Domain Services
- D) ExpressRoute Identity Sync

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** Pass-through Authentication (PTA) and AD FS both validate user passwords directly against the on-premises Active Directory without storing hashes in the cloud. PHS stores hashes, which violates the requirement.
</details>

### Q7. [EXPERT] Dynamic User Groups
**Scenario:** You are creating a Dynamic User Group in Microsoft Entra ID to automatically group users whose display name starts with "EXT-". The group is created, but no users are being added, even though several users match the criteria. What is the most likely cause?

**Options:**
- A) Dynamic groups can only evaluate membership rules once every 48 hours.
- B) The users do not have Entra ID P1 licenses assigned to them.
- C) The syntax of the membership rule is invalid, or the system is still processing the initial evaluation (which can take up to 24 hours depending on tenant size).
- D) Dynamic groups can only be used for Devices, not Users.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** C

**Explanation:** Dynamic group evaluation isn't always instant. It can take some time to evaluate initially, and invalid syntax (which isn't always caught immediately in raw rules) can cause silent failures. (B is tricky: while the tenant needs Entra ID P1, evaluating it isn't blocked per user if the tenant has P1). D is false.
</details>

### Q8. [EASY] Moving Resources
**Scenario:** You have a Virtual Machine and its associated Network Interface Card (NIC) in Resource Group A. You want to move the VM to Resource Group B. Which of the following is true?

**Options:**
- A) You must also move the NIC and Virtual Network to Resource Group B.
- B) You can move the VM to Resource Group B, but its dependent resources (like the NIC) can stay in Resource Group A, or be moved together.
- C) VMs cannot be moved between Resource Groups without downtime.
- D) You must copy the VM disk, create a new VM, and delete the old one.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** Azure allows you to move resources between Resource Groups. The VM and its NIC do not have to be in the same Resource Group, although moving them together is best practice for organization.
</details>

### Q9. [MEDIUM] RBAC Inheritance
**Scenario:** User `Dev1` is assigned the "Reader" role at the Subscription scope. They are also assigned the "Contributor" role at the scope of `Resource Group X`. What actions can `Dev1` perform on a Virtual Machine inside `Resource Group X`?

**Options:**
- A) Only Read, because Subscription-level denying/limiting rules take precedence.
- B) Stop, Start, and Modify the VM, because RBAC permissions are additive and they have Contributor access at the nearer scope.
- C) They cannot access the VM due to conflicting role assignments.
- D) Only Read, because RBAC evaluates the strictest permission first.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** Azure RBAC is an additive model. There is no implicit "deny". If a user is granted Reader at a higher level and Contributor at a lower level, their effective permission at the lower level is Contributor.
</details>

### Q10. [HARD] Azure Policy Exclusions
**Scenario:** You have a policy at the management group ensuring all Storage Accounts force HTTPS traffic. However, one legacy application in `Sub-A`, within a specific resource group `Legacy-RG`, requires HTTP. How do you allow HTTP for this specific RG without removing the policy?

**Options:**
- A) Create an Azure Policy Override lock on the `Legacy-RG`.
- B) Add `Legacy-RG` to the "Exclusions" scope of the Azure Policy assignment.
- C) Assign a custom RBAC role to the application to bypass policy.
- D) Change the policy effect to `Audit` only for all subscriptions.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** When assigning an Azure Policy, you can specify Exclusions to exempt specific child scopes (like a subscription or resource group) from the policy assignment.
</details>

### Q11. [MEDIUM] Entra ID Device Management
**Scenario:** A company provides laptops to its employees. The laptops are managed via Microsoft Intune and users sign into the laptops using their Entra ID credentials. What type of device identity are these laptops?

**Options:**
- A) Microsoft Entra Registered
- B) Microsoft Entra Joined
- C) Microsoft Entra Hybrid Joined
- D) Azure Arc Managed

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** Because the company owns the devices, provisions them, and users sign in with their Entra ID credentials (with no mention of on-prem AD), they are Microsoft Entra Joined. Registered is typically for personal BYOD devices.
</details>

### Q12. [HARD] Access Reviews
**Scenario:** You want to routinely verify that members of an "External Guest" group still need access to your Azure tenant every 90 days. What feature should you use?

**Options:**
- A) Azure Advisor
- B) Microsoft Entra ID Identity Protection
- C) Microsoft Entra Access Reviews
- D) Azure Policy Audit

<details>
<summary><b>Show Answer</b></summary>

**Answer:** C

**Explanation:** Access Reviews allow organizations to efficiently manage group memberships, access to enterprise applications, and role assignments by routinely asking users or managers to justify continued access.
</details>

### Q13. [EASY] Tags in Azure
**Scenario:** If you apply a tag `Environment: Prod` to a Resource Group, what happens to the virtual machines inside that resource group?

**Options:**
- A) They automatically receive the tag `Environment: Prod` via inheritance.
- B) They do not inherit the tag. Tags applied to a resource group are not applied to resources automatically.
- C) They inherit the tag only if Azure Policy enforces it.
- D) Both B and C are correct.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** D

**Explanation:** By default (B), tags do NOT inherit from resource groups to resources. However, you can use Azure Policy (C) to enforce tag inheritance. Therefore D is the most complete correct answer.
</details>

### Q14. [MEDIUM] Management Groups limit
**Scenario:** Your enterprise is setting up a governance structure using Management Groups. What is the maximum depth of a Management Group hierarchy you can create?

**Options:**
- A) 4 levels
- B) 6 levels
- C) 8 levels
- D) Unlimited

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** Management groups can support a hierarchy up to 6 levels deep (not including the Root management group or the subscription level).
</details>

### Q15. [EXPERT] Custom Domain Names
**Scenario:** You want to add `contoso.com` as a custom domain name to your Microsoft Entra ID tenant. You add it in the portal. What is the immediate next step you MUST perform before you can use it for users?

**Options:**
- A) Change the primary domain in Microsoft Entra ID.
- B) Verify the domain by adding a TXT or MX record to the DNS zone of `contoso.com`.
- C) Purchase an Azure App Service certificate.
- D) Create an Entra ID conditional access policy to secure the domain.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** To prove you own the domain, Microsoft Entra ID requires you to verify ownership by creating a specific TXT or MX record in your DNS provider's configuration.

---

