# AZ-104 Microsoft Azure Administrator: 100-Question Practice Exam

Welcome to the ultimate 100-question practice exam for the AZ-104 certification! 
This exam is designed to help you prepare by testing your knowledge across all major domains: Identities, Governance, Storage, Compute, Networking, Monitoring, Backup, Hybrid Identity, Load Balancing, and Automation.

**How to use this exam:**
- Questions are grouped by domain.
- Each question has a difficulty tag: `[EASY]`, `[MEDIUM]`, `[HARD]`, or `[EXPERT]`.
- Answers and explanations are hidden inside collapsible blocks. Think about the scenario first, pick your answer, and then expand the block to see the correct answer and a detailed explanation.

---

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

## Domain 2: Implement and Manage Storage (Questions 16 - 25)

### Q16. [EASY] Storage Account Redundancy
**Scenario:** You need a storage account to survive the total physical loss of an entire Azure region. Which redundancy option must you choose?

**Options:**
- A) Locally Redundant Storage (LRS)
- B) Zone Redundant Storage (ZRS)
- C) Geo-Redundant Storage (GRS)
- D) Premium Block Blob

<details>
<summary><b>Show Answer</b></summary>

**Answer:** C

**Explanation:** GRS copies your data to a secondary geographic region miles away, protecting against regional disasters. ZRS only protects against data center failures within the same region.
</details>

### Q17. [MEDIUM] Blob Tiers
**Scenario:** Your company stores large video archives that are accessed once every six months for audits. Storage costs are high, and retrieving the files takes an hour. Which blob access tier is most cost-effective and appropriate?

**Options:**
- A) Hot
- B) Cool
- C) Cold
- D) Archive

<details>
<summary><b>Show Answer</b></summary>

**Answer:** D

**Explanation:** The Archive tier is the cheapest for data storage, designed for data accessed very rarely. Retrieval times can take hours (rehydration), making it perfect for an audit archive scenario. 
</details>

### Q18. [MEDIUM] Azure File Sync
**Scenario:** An on-premises file server is running out of disk space. You want to keep frequently accessed files locally for fast access, while older files automatically tier to Azure Files. What should you configure?

**Options:**
- A) Storage Replica
- B) DFS Namespace
- C) Azure File Sync with Cloud Tiering enabled
- D) A Blob Lifecycle Management Policy

<details>
<summary><b>Show Answer</b></summary>

**Answer:** C

**Explanation:** Azure File Sync seamlessly syncs your on-premises Windows Server with Azure Files. Enabling "Cloud Tiering" ensures that heavily used files are cached locally on the server, while long-unused files are sent to Azure to free up local disk space.
</details>

### Q19. [HARD] Storage Account Security
**Scenario:** You have an Azure Storage Account that should ONLY be accessible by Virtual Machines originating from inside `VNetA`. No public internet access should be allowed. How do you configure this?

**Options:**
- A) Configure the Storage firewall to block 0.0.0.0/0.
- B) Create a Private Endpoint for the Storage Account linked to `VNetA`. Disable "Allow public access from all networks".
- C) Use an NSG to block port 443 outbound from `VNetA`.
- D) Use a SAS token with IP restrictions.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** A Private Endpoint brings the Storage Account inside the VNet by giving it a private IP. Disabling public access ensures that the data can only be hit via that internal private endpoint. Service Endpoints are also an option, but Private Endpoint completely removes it from public DNS routing space, making it the most secure method.
</details>

### Q20. [EASY] Blob Lifecycle Management
**Scenario:** You accumulate thousands of log files daily. You want to automatically delete logs that are older than 30 days without writing custom scripts. What feature do you use?

**Options:**
- A) Azure Automation Runbooks
- B) Azure Logic Apps
- C) Data Lake Storage Gen2 properties
- D) Lifecycle Management Policy in the Storage Account

<details>
<summary><b>Show Answer</b></summary>

**Answer:** D

**Explanation:** Lifecycle management rules automatically transition blobs to cooler tiers or delete them entirely based on the blob's age (days since last modification or creation).
</details>

### Q21. [EXPERT] SAS Tokens
**Scenario:** You generate an Account Shared Access Signature (SAS) token to give an external vendor upload access. You realize the vendor's machine is compromised and you need to revoke the SAS token immediately. What is the FASTEST way to invalidate the SAS token?

**Options:**
- A) Delete the user account of the person who generated the token.
- B) Change the Azure policy to deny external connections.
- C) Regenerate the Storage Account Key that was used to create the SAS token.
- D) Revoke the token using an Azure CLI `az storage account revoke-sas` command.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** C

**Explanation:** Account SAS tokens are cryptographically signed using one of the two Storage Account Keys (Key1 or Key2). Regenerating the key immediately breaks any SAS token signed with the old key. (Note: Using an Access Policy on a container is better practice for revoking later without breaking other apps, but for an Account SAS, key rotation is the only way).
</details>

### Q22. [MEDIUM] Storage Object Replication
**Scenario:** You want to automatically copy blobs as they are written from `StorageAccount_A` in East US to `StorageAccount_B` in West US. You enable Object Replication. What prerequisite MUST factor into your design?

**Options:**
- A) Both storage accounts must use the Archive tier.
- B) Storage Account A must have Versioning enabled.
- C) It only works for Azure Files, not Blob Storage.
- D) Both storage accounts must have network firewalls disabled.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** Object Replication asynchronously copies block blobs between a source storage account and a destination account. A prerequisite for Object Replication is that Blob Versioning (and Change Feed) must be enabled on both accounts.
</details>

### Q23. [HARD] Data Lake Gen2
**Scenario:** You are analyzing massive big data models and require hierarchical namespaces (true directory structures) on your Azure Storage account to speed up analytical workloads. How do you achieve this?

**Options:**
- A) Upgrade to Premium SSD.
- B) Check the 'Enable Hierarchical Namespace' box during Storage Account creation.
- C) Connect the account to Azure SQL.
- D) Use Azure Files instead of Blob storage.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** Azure Data Lake Storage Gen2 is essentially Blob storage with a hierarchical namespace enabled. This gives it true folder/directory structures, which drastically improves big data analytics tools (like Apache Spark) processing speeds.
</details>

### Q24. [MEDIUM] Azure Storage Explorer
**Scenario:** A developer uses Azure Storage Explorer to view Blob containers. They sign in using their Entra ID account, which has 'Reader' access to the Storage Account. They receive an "Access Denied" error when trying to view the blobs. Why?

**Options:**
- A) Storage Explorer requires port 3389 open.
- B) The 'Reader' role only grants access to view the Storage Account management plane settings, not the data plane (blobs) inside it.
- C) Entra ID authentication is not supported in Storage Explorer.
- D) They must use a VPN.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** The "Reader" RBAC role is a management plane role. To view blobs, the user needs a data plane role like "Storage Blob Data Reader".
</details>

### Q25. [EASY] Moving Storage Accounts
**Scenario:** You attempt to move a Storage Account from Azure Subscription A to Subscription B. Which statement is correct?

**Options:**
- A) This action will cause the Storage Account's access keys to change.
- B) Moving a Storage Account is not supported.
- C) The data in the Storage Account will remain unaffected and accessible during the move.
- D) You must copy all blobs using AzCopy before initiating the move.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** C

**Explanation:** When moving a storage account across resource groups or subscriptions, the underlying data remains intact and available. Keys do not regenerate automatically. 
</details>


---

## Domain 3: Deploy and Manage Azure Compute Resources (Questions 26 - 45)

### Q26. [EASY] Virtual Machine Sizing
**Scenario:** A newly deployed Virtual Machine (VM) running a heavy SQL database is performing extremely slowly. Upon checking Metrics, you see the CPU is at 20%, but Disk I/O is maxed out. You need to fix this with minimal downtime. What do you do?

**Options:**
- A) Add more network interfaces to the VM.
- B) Resizes the VM to a size that supports higher IOPS, or upgrade the disk type to Premium SSD.
- C) Re-deploy the VM to a different region.
- D) Assign a Public IP address.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** Disk throttling occurs when the VM size limitations or the Disk type limitations are hit. Resizing the VM to a tier that supports higher IOPS (like the Ds-series) or upgrading standard disks to Premium SSDs resolves this.
</details>

### Q27. [MEDIUM] Availability Sets
**Scenario:** You have two web servers in an Availability Set. Microsoft initiates a planned maintenance event that requires rebooting underlying hypervisors. How does the Availability Set protect your servers?

**Options:**
- A) It ensures the VMs are in different geographical regions.
- B) It places the VMs in different Update Domains, ensuring only one VM is rebooted at a time.
- C) It places the VMs in different Fault Domains to protect against rack power failures during maintenance.
- D) It automatically fails over traffic to a backup storage account.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** For planned maintenance (reboots), Update Domains are used. An Availability Set ensures VMs are spread across different Update Domains, so Microsoft will not reboot both web servers at the same time. Fault domains protect against unplanned hardware failures.
</details>

### Q28. [HARD] VM Scale Sets (VMSS) Scaling Rules
**Scenario:** You configure an Azure Virtual Machine Scale Set (VMSS) to autoscale based on CPU. 
Rule 1: If CPU > 75% for 10 minutes, Increase instances by 1.
Rule 2: If CPU < 25% for 10 minutes, Decrease instances by 1.
The current CPU goes to 85% for 15 minutes, then drops to 10% for 15 minutes. What happens?

**Options:**
- A) It scales out by 1, and immediately scales in by 1.
- B) It scales out by 1, waits for the "cool down" period, computes the 10 min average, and then scales in by 1.
- C) It scales out infinitely until blocked by limits.
- D) It ignores Rule 2 because scale-out rules take permanent precedence.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** Autoscale relies on cool-down minutes (usually 5 mins by default) after a scale event before it triggers another. It scales out, waits, then once the threshold is met again, it scales in.
</details>

### Q29. [HARD] ARM Templates
**Scenario:** You are deploying a 3-tier architecture using an Azure Resource Manager (ARM) JSON template. The Web VM depends on the Database VM being fully built and generating a connection string first. How do you ensure the Web VM waits for the Database VM in the template?

**Options:**
- A) Put the Web VM resource block physically lower down in the JSON file.
- B) Use the `dependsOn` element in the Web VM resource block referencing the Database VM.
- C) Run the template twice.
- D) Add a 10-minute timeout script in a custom script extension.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** ARM templates execute deployment of resources in parallel by default to be fast. To enforce a sequential deployment order, you must explicitly declare `dependsOn: ["[resourceId('Microsoft.Compute/virtualMachines', 'DatabaseVM')]"]` in the dependent resource.
</details>

### Q30. [EASY] Spot Instances
**Scenario:** You need to run a massive batch processing job overnight. It doesn't matter if the job stops and starts or gets delayed, but it needs to be as cheap as possible. What Azure VM feature should you use?

**Options:**
- A) Reserved Instances
- B) B-Series Burstable VMs
- C) Spot VMs
- D) Azure Container Instances

<details>
<summary><b>Show Answer</b></summary>

**Answer:** C

**Explanation:** Azure Spot VMs let you take advantage of unused Azure compute capacity at a massive discount (up to 90%). The catch is that Azure can evict your VM at any time if it needs the capacity back. It's perfect for interruptible batch jobs.
</details>

### Q31. [MEDIUM] Custom Script Extension
**Scenario:** You deploy a Linux VM and need it to automatically run an `apt-get install nginx` command during provisioning. Which Azure feature helps you do this without needing to SSH into the box?

**Options:**
- A) Azure Bastion
- B) Network Security Group rules
- C) Custom Script Extension
- D) Azure Backup

<details>
<summary><b>Show Answer</b></summary>

**Answer:** C

**Explanation:** Virtual Machine Extensions are small applications that provide post-deployment configuration and automation tasks. The Custom Script Extension downloads and executes scripts on Azure virtual machines automatically after deployment.
</details>

### Q32. [MEDIUM] Azure Kubernetes Service (AKS)
**Scenario:** Your company runs an Azure Kubernetes Service (AKS) cluster. You need to ensure the worker nodes (the VMs running the containers) are automatically patched with the latest OS security updates. What is the default behavior?

**Options:**
- A) AKS automatically reboots nodes nightly for patches.
- B) The base OS patches are downloaded automatically to the nodes, but they are NOT automatically rebooted. You must use tools like Kured to handle the reboots appropriately.
- C) You are not allowed to patch AKS nodes; you must redeploy the cluster.
- D) Patches are pushed immediately, draining all containers instantly.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** By default, Linux OS security patches are downloaded and installed on AKS nodes nightly. However, if a patch requires a reboot, it will *not* reboot automatically (to save your workloads from crash). You must safely drain and reboot them using automated daemonsets like Kured or node image upgrades.
</details>

### Q33. [EASY] App Service Plans
**Scenario:** You are deploying an Azure App Service (Web App). You want to run a staging slot alongside your production slot so you can swap them instantly without downtime. Which App Service Plan tier is the minimum required?

**Options:**
- A) Free
- B) Shared
- C) Basic
- D) Standard

<details>
<summary><b>Show Answer</b></summary>

**Answer:** D

**Explanation:** Deployment slots (Staging slots) are a feature used for blue-green deployments. They are only available starting at the Standard tier (and higher like Premium). 
</details>

### Q34. [HARD] App Service Backups
**Scenario:** You have a Standard App Service plan. How do you configure it so that the application code and the connected Azure SQL database are backed up together every day?

**Options:**
- A) App Service does not support database backups.
- B) You configure a Custom Backup in the App Service and provide the connection string to the SQL Database.
- C) You must write a PowerShell script to run via Windows Task Scheduler.
- D) You back up the App Service natively, but must back up the SQL VM manually.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** Azure App Service custom backups can back up app content, configuration, and a linked database (SQL Database, MySQL, PostgreSQL) together in one zip file if you include the connection string in the backup configuration.
</details>

### Q35. [EXPERT] Azure Container Instances (ACI) vs AKS
**Scenario:** You have a Python script packaged into a Docker container. The script runs for 5 minutes, scrapes a website, saves data to a blob, and exits. It runs once a day. Which Azure service is the cheapest and requires the absolute least amount of management overhead to run this?

**Options:**
- A) Azure Kubernetes Service (AKS)
- B) An Azure VM with Docker installed.
- C) Azure Container Instances (ACI)
- D) Azure App Service

<details>
<summary><b>Show Answer</b></summary>

**Answer:** C

**Explanation:** ACI is severely lightweight and bills by the second. For a short-lived containerized task that runs and stops, ACI provides the fastest execution with zero cluster management overhead, making it drastically cheaper and easier than provisioning an AKS cluster or VM.
</details>

### Q36. [EASY] Azure Bastion
**Scenario:** Your security team mandates that RDP/SSH ports (3389/22) MUST NOT be exposed to the public internet. However, you still need to log into your VMs to manage them. What securely solves this?

**Options:**
- A) Azure Bastion
- B) Network Security Group allowing port 80.
- C) Azure Firewall
- D) ExpressRoute

<details>
<summary><b>Show Answer</b></summary>

**Answer:** A

**Explanation:** Azure Bastion is a fully managed PaaS service that provides secure and seamless RDP/SSH connectivity to VMs directly from the Azure portal over TLS (port 443). The VMs do not need public IP addresses.
</details>

### Q37. [MEDIUM] Updating VM SSH Keys
**Scenario:** A developer loses their private SSH key to access a Linux Virtual Machine. How can you restore access for them without rebuilding the VM?

**Options:**
- A) Reboot the VM.
- B) Use the "Reset Password" blade in the Azure Portal to inject a new SSH public key into the VM.
- C) You must delete the OS disk and recreate it.
- D) Create an RDP connection instead.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** The "Reset Password" tool in the Azure portal for VMs utilizes the VMAccess extension to reset the local administrator password or inject a new SSH public key, bypassing the need to log in.
</details>

### Q38. [HARD] Ephemeral OS Disks
**Scenario:** You are deploying a massive cluster of stateless web servers as a VM Scale Set. You want the fastest possible boot times and re-image times while saving costs on OS disk storage. What feature should you use?

**Options:**
- A) Ultra Disks
- B) Ephemeral OS Disks
- C) Premium Block Blobs
- D) Standard HDD

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** Ephemeral OS disks are created directly on the local physical host hardware (the VM's cache or temp storage) rather than remotely in Azure Storage. This makes reading/writing to the OS disk incredibly fast and removes the billing cost of a managed disk, perfect for stateless nodes but terrible for anything storing persistent data.
</details>

### Q39. [MEDIUM] Virtual Machine Data Disks
**Scenario:** You attach a new 1TB managed data disk to an existing running Windows VM in the Azure Portal. You RDP into the VM, open File Explorer, but the disk doesn't show up. Why?

**Options:**
- A) You need to reboot the VM.
- B) You cannot attach disks to running VMs.
- C) You must initialize, format (NTFS), and assign a drive letter to the disk inside Windows Disk Management first.
- D) The disk is read-only by default.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** C

**Explanation:** Attaching a disk in the Azure portal is like plugging a new physical hard drive into a PC's motherboard. The OS sees raw hardware. You still must initialize it, partition it, and format it inside the guest OS before it is usable.
</details>

### Q40. [HARD] Disks and Subscriptions
**Scenario:** You have an unattached Managed Disk in Subscription A. You want to attach it to a Virtual Machine running in Subscription B. Is this possible?

**Options:**
- A) Yes, you can attach it directly across subscriptions.
- B) No, a managed disk must be in the same Subscription and Region as the VM it is attached to. You must move the disk to Subscription B first.
- C) Yes, if you use a VNet Peering connection.
- D) Only if the disk is an Unmanaged disk.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** Managed disks are regional resources that must reside in the exact same region and the exact same subscription as the Virtual Machine they are attached to.
</details>

### Q41. [EASY] Scale Sets vs Availability Sets
**Scenario:** You need 5 identically configured web VMs that sit behind a load balancer. You want Azure to automatically add a 6th VM if traffic gets high. Which service should you use?

**Options:**
- A) Availability Sets
- B) Virtual Machine Scale Sets (VMSS)
- C) Availability Zones
- D) Azure Load Balancer rules

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** Only VMSS supports automated scaling (Autoscale) where identical instances are deployed and removed dynamically based on metric rules. Availability sets only provide fault tolerance, not autoscaling.
</details>

### Q42. [MEDIUM] Host Level Backup
**Scenario:** You notice Azure Backup is creating recovery points for your VM every night natively without you having to install a backup agent manually into the Windows OS. How does Azure Backup accomplish this?

**Options:**
- A) By taking a screenshot of the memory.
- B) By utilizing the VM Backup Extension pushed via the Azure fabric to take snapshots of the VHDs.
- C) By cloning the VM to another VNet.
- D) It actually doesn't; you must install an agent.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** Azure Backup installs an extension (VMSnapshot) to the VM on the fly via the Azure VM Agent. This extension coordinates with the hypervisor to take a consistency snapshot of the disks without manual intervention.
</details>

### Q43. [HARD] App Service Networking
**Scenario:** You have an Azure App Service that needs to securely pull data from a SQL database located inside your company's private Virtual Network (VNet). By default, App Services are public-facing. How do you allow the App Service to talk to the VNet?

**Options:**
- A) Set up VNet Integration in the App Service, allocating a dedicated subnet for the App Service to "plug into" the VNet.
- B) Route all traffic through the public internet.
- C) Enable NSG Flow Logs.
- D) Move the App Service into an Availability Set.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** A

**Explanation:** VNet Integration allows your App Service to make outbound calls into a private VNet. It requires a dedicated, empty subnet in the VNet for the App Service instances to route their outbound traffic through.
</details>

### Q44. [MEDIUM] Container Registry
**Scenario:** You built a custom Docker image for a web app and want to deploy it to Azure Kubernetes Service (AKS). Where should you securely store this Docker image prior to deployment?

**Options:**
- A) Azure Blob Storage
- B) Azure Container Registry (ACR)
- C) Azure Key Vault
- D) Azure File Share

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** Azure Container Registry (ACR) is a private registry for hosting Docker container images securely. AKS can inherently authenticate with ACR (via Managed Identities) to pull those images.
</details>

### Q45. [EASY] Managing Costs for VMs
**Scenario:** You have a lab environment with 10 VMs. Developers only use them from 9 AM to 5 PM. What is the easiest way to significantly reduce costs?

**Options:**
- A) Downgrade them to the Basic tier disks, but leave them running.
- B) Configure "Auto-shutdown" schedules on the VMs to deallocate them every evening at 6 PM.
- C) Delete the VMs completely every day and restore them from backup.
- D) Turn on Azure Advisor.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** Virtual Machines only incur compute charges when they are running. The built-in "Auto-shutdown" feature easily schedules them to deallocate (stop and release the hardware) automatically, saving massive amounts of money overnight.

*(End of Batch 2)*
</details>

---

## Domain 4: Configure and Manage Virtual Networking (Questions 46 - 70)

### Q46. [EASY] VNet Peering Transitivity
**Scenario:** You have three Virtual Networks. VNet-A is peered with VNet-B. VNet-B is peered with VNet-C. A Virtual Machine in VNet-A needs to communicate with a database in VNet-C. How do you allow this?

**Options:**
- A) Communication works by default since peering is transitive.
- B) You must configure a Route Table in VNet-B.
- C) You must explicitly peer VNet-A directly to VNet-C.
- D) You must use an Azure VPN Gateway.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** C

**Explanation:** VNet Peering is NOT transitive. If A peers to B, and B peers to C, A cannot talk to C without a direct peering connection between A and C, or a complex routing hub-and-spoke setup with an NVA/Firewall.
</details>

### Q47. [MEDIUM] VNet Peering Address Space
**Scenario:** You are trying to establish a VNet peering connection between VNet-East (10.1.0.0/16) and VNet-West (10.1.5.0/24). The portal returns an error. Why?

**Options:**
- A) You cannot peer VNets across different regions.
- B) The IP address spaces overlap.
- C) VNet-East does not have a Bastion host.
- D) You must use an ExpressRoute for cross-region peering.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** VNets cannot be peered if their IP address spaces overlap. Since 10.1.5.0/24 is a subset of 10.1.0.0/16, they overlap and peering will fail.
</details>

### Q48. [HARD] Azure VPN Gateway vs ExpressRoute
**Scenario:** Your company requires a highly secure, private, dedicated connection to Azure that does NOT travel over the public internet, supporting speeds up to 10 Gbps. Which networking solution should you deploy?

**Options:**
- A) Site-to-Site VPN
- B) Point-to-Site VPN
- C) Azure ExpressRoute
- D) VNet Peering

<details>
<summary><b>Show Answer</b></summary>

**Answer:** C

**Explanation:** ExpressRoute provides a private, dedicated connection to Azure via a connectivity provider. It does not go over the public internet (unlike VPNs) and offers much higher reliability, speed, and lower latencies.
</details>

### Q49. [MEDIUM] Network Security Groups (NSG) Priority
**Scenario:** An NSG on a subnet has the following inbound rules:
Rule 100: Allow Port 80 (HTTP)
Rule 200: Deny Port 80 (HTTP)
Rule 300: Allow Port 443 (HTTPS)
What traffic is allowed?

**Options:**
- A) Only Port 443. Port 80 is denied because Deny takes precedence.
- B) Both Port 80 and Port 443 are allowed.
- C) Neither are allowed.
- D) Traffic is dropped due to a rule grouping error.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** NSG rules are evaluated by Priority number, starting from the lowest number to the highest. Once a match is found, evaluation stops. Since 100 (Allow 80) is evaluated before 200 (Deny 80), Port 80 is allowed. Port 443 is allowed by rule 300.
</details>

### Q50. [EASY] NSG Default Rules
**Scenario:** You create a brand new NSG and associate it to a subnet. You do not create any custom rules. Can a VM in this subnet ping a database in the exact same VNet?

**Options:**
- A) Yes.
- B) No.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** A

**Explanation:** Yes. NSGs contain default rules that cannot be deleted. One of the default inbound and outbound rules is to "AllowVnetInBound" and "AllowVnetOutBound" which allows all traffic to flow freely inside the same VNet.
</details>

### Q51. [EXPERT] Effective Security Rules
**Scenario:** You apply an NSG to `SubnetA` that ALLOWS port 3389 inbound. You apply a different NSG directly to the Network Interface (NIC) of `VM1` (inside SubnetA) that DENIES port 3389 inbound. What happens when you try to RDP to `VM1`?

**Options:**
- A) The connection succeeds because Subnet rules override NIC rules.
- B) The connection fails because the connection must pass BOTH the Subnet NSG and the NIC NSG.
- C) The connection succeeds because NIC rules override Subnet rules.
- D) This configuration is not allowed in Azure.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** For inbound traffic, Azure evaluates the Subnet NSG first, and then the NIC NSG. If *either* of them has a Deny rule, the traffic drops. Because the NIC NSG denies it, the connection fails. 
</details>

### Q52. [MEDIUM] Azure DNS
**Scenario:** You want your internal Azure VMs to be able to resolve each other by a custom domain name (e.g., `db.internal.contoso.com`). You do NOT want this domain exposed to the public internet. What should you use?

**Options:**
- A) Azure Public DNS Zone
- B) Azure Private DNS Zone linked to the VNet
- C) An Azure Load Balancer
- D) Active Directory Domain Services only

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** Azure Private DNS provides a reliable and secure DNS service for your virtual network without the need to manage custom DNS servers. By linking the Private DNS zone to your VNet, VMs can securely resolve each other.
</details>

### Q53. [HARD] User-Defined Routes (UDR)
**Scenario:** You want to force all outbound internet traffic (0.0.0.0/0) from your database subnet to pass through an Azure Firewall appliance deployed in a hub VNet. How do you implement this?

**Options:**
- A) Add a rule to the Network Security Group.
- B) Use Azure Network Watcher.
- C) Create a Route Table with a UDR pointing to the Firewall's private IP as the Next Hop, and attach it to the database subnet.
- D) Create a NAT Gateway.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** C

**Explanation:** BGP and system routes handle default routing, but to override the default route to the internet, you must create a Custom Route Table with a UDR (User-Defined Route) for `0.0.0.0/0` with the "Next hop type" set to "Virtual Appliance" (the firewall).
</details>

### Q54. [MEDIUM] Public IPs
**Scenario:** A VM with a Dynamic Public IP address is shut down (Deallocated) for the weekend. When it is started on Monday, what happens to the IP?

**Options:**
- A) It remains exactly the same.
- B) It changes to a different IP address because Dynamic IPs are released when a VM is deallocated.
- C) The IP is locked for 7 days before changing.
- D) A Static IP is automatically assigned.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** Dynamic Public IP addresses are released back into the Azure pool when the resource is stopped (deallocated). When started again, it gets a new IP. (To keep it the same, it must be changed to Static).
</details>

### Q55. [EASY] Application Security Groups (ASG)
**Scenario:** You have 50 web servers and 50 database servers in the same subnet. You want to create an NSG rule that allows port 1433 only from the web servers to the database servers. Instead of listing 50 IP addresses in the rule, what feature simplifies this?

**Options:**
- A) Application Gateway
- B) Resource Tags
- C) Application Security Groups (ASGs)
- D) ExpressRoute

<details>
<summary><b>Show Answer</b></summary>

**Answer:** C

**Explanation:** ASGs allow you to group network interfaces logically (e.g., "WebGroup" and "DBGroup") and then use those groups as the source/destination in NSG rules, making rule management incredibly simple.
</details>

### Q56. [MEDIUM] Virtual Network NAT
**Scenario:** You have 10 VMs in a private subnet. They need to download updates from the internet, but they do NOT have public IP addresses. You want them to share a single, predictable Public IP address for outbound traffic. What is the best service to deploy?

**Options:**
- A) Azure Bastion
- B) Azure Load Balancer
- C) NAT Gateway attached to the subnet
- D) Route Table

<details>
<summary><b>Show Answer</b></summary>

**Answer:** C

**Explanation:** A NAT (Network Address Translation) Gateway provides highly resilient outbound-only internet connectivity for one or more subnets using a static Public IP address.
</details>

### Q57. [HARD] Service Endpoints
**Scenario:** You want VMs in a specific Subnet to access an Azure SQL Database securely over the Azure backbone network without going to the public internet. Furthermore, the Azure SQL database must ONLY accept connections from this Subnet. What should you configure?

**Options:**
- A) Enable a Service Endpoint for `Microsoft.Sql` on the subnet, and configure the SQL firewall to allow that subnet.
- B) Use Azure Front Door.
- C) Use a public NAT Gateway.
- D) Change the DNS settings of the VM.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** A

**Explanation:** Service Endpoints extend your VNet's private address space to Azure PaaS resources over a direct connection. By enabling the endpoint on the subnet, traffic routes privately, and you can lock down the PaaS firewall to only allow that specific subnet.
</details>

### Q58. [EXPERT] Private Link vs Service Endpoints
**Scenario:** What is the fundamental difference between a Service Endpoint and a Private Endpoint (Private Link) for an Azure Storage account?

**Options:**
- A) They are identical names for the same service.
- B) A Service Endpoint uses public IP routing underneath, whereas Private Link creates a Network Interface (NIC) inside your VNet with a private IP address.
- C) Service Endpoints cost $100/month, Private Endpoints are free.
- D) Private Link only works for Azure SQL.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** Service Endpoints optimise the route to the PaaS service's *public* IP over the Azure backbone. Private Link actually injects a Virtual Network Interface (NIC) into your subnet, giving the PaaS service a *private* IP (e.g., 10.0.1.5) native to your VNet.
</details>

### Q59. [MEDIUM] Load Balancer Types
**Scenario:** You want to distribute HTTPS (port 443) network traffic across a pool of VMs behind a public IP. The traffic is purely TCP based and doesn't require inspecting the URL path. Which Azure service is most appropriate?

**Options:**
- A) Azure Front Door
- B) Application Gateway
- C) Azure Standard Load Balancer
- D) Virtual Network Gateway

<details>
<summary><b>Show Answer</b></summary>

**Answer:** C

**Explanation:** Azure Load Balancer operates at Layer 4 (TCP/UDP) and is perfect for simple port-based load balancing. Application Gateway operates at Layer 7 and is used when you need HTTP/HTTPS URL path-based routing or SSL termination.
</details>

### Q60. [HARD] Application Gateway Routing
**Scenario:** You have a single public IP address. If users visit `contoso.com/images`, you want traffic routed to VM Pool A. If they visit `contoso.com/video`, you want traffic routed to VM Pool B. Which Azure service supports this?

**Options:**
- A) Azure Standard Load Balancer
- B) Azure Application Gateway
- C) Azure Traffic Manager
- D) Azure DNS

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** Azure Application Gateway is a web traffic (OSI Layer 7) load balancer that allows you to make routing decisions based on URL Path or HTTP headers.
</details>

### Q61. [EASY] Network Watcher
**Scenario:** Two VMs in different subnets cannot ping each other. You suspect an NSG is blocking traffic but cannot figure out which rule is causing it. Which Azure Network Watcher tool tells you exactly which NSG rule is denying the packet?

**Options:**
- A) IP Flow Verify
- B) Next Hop
- C) Connection Monitor
- D) Traffic Analytics

<details>
<summary><b>Show Answer</b></summary>

**Answer:** A

**Explanation:** IP Flow Verify checks if a packet is allowed or denied to/from a VM based on configured NSG rules. It explicitly tells you the name of the rule that is blocking the traffic.
</details>

### Q62. [MEDIUM] ExpressRoute Circuits
**Scenario:** You deploy an ExpressRoute circuit. The provider status says "Provisioned" but the circuit status says "Not Enabled". What is the issue?

**Options:**
- A) The ExpressRoute Gateway has not been created in Azure.
- B) Microsoft has not received your BGP routing tables.
- C) You have not linked a Virtual Network to the circuit yet.
- D) Both the Provider and Microsoft must complete the peering configuration, and the status implies the provider is done, but the BGP peering isn't up.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** D

**Explanation:** ExpressRoute circuits require both the service provider to provision the line, and then BGP peering must be configured (Private, Public, or Microsoft peering) before traffic can flow.
</details>

### Q63. [HARD] Traffic Manager Routing Methods
**Scenario:** You have a web application hosted in the US East region and a backup copy in US West. You want all user traffic to go to US East normally, and ONLY go to US West if US East goes completely offline. Which Traffic Manager routing method should you use?

**Options:**
- A) Performance
- B) Weighted
- C) Priority
- D) Geographic

<details>
<summary><b>Show Answer</b></summary>

**Answer:** C

**Explanation:** Priority routing is used for active-passive failover. You give US East Priority 1, and US West Priority 2. Traffic Manager sends everyone to 1, unless 1 fails its health checks, then it sends them to 2.
</details>

### Q64. [EASY] VPN Gateway SKU
**Scenario:** You need to connect an on-premises branch office to Azure using a dedicated VPN appliance over the internet. Which Azure resource provides the endpoint in the cloud?

**Options:**
- A) Azure Firewall
- B) Local Network Gateway
- C) Virtual Network Gateway (VPN type)
- D) Application Security Group

<details>
<summary><b>Show Answer</b></summary>

**Answer:** C

**Explanation:** A Virtual Network Gateway provides the cloud-side endpoint for encrypted cross-premises VPN traffic over the internet.
</details>

### Q65. [MEDIUM] Global VNet Peering
**Scenario:** VNet A is in North Europe. VNet B is in East US. You set up VNet Peering between them. How does the network traffic travel?

**Options:**
- A) It is routed securely over the public internet.
- B) It uses the Microsoft private backbone network.
- C) It requires an ExpressRoute circuit intermediary.
- D) It drops because cross-region peering is not allowed.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** Global VNet Peering connects VNets across different regions entirely over the Microsoft backbone infrastructure. It never touches the public internet.
</details>

### Q66. [EASY] Azure Firewall
**Scenario:** You need a managed, cloud-based network security service that protects your VNet resources with built-in high availability and unrestricted cloud scalability, specializing in complex L3-L7 rule processing across multiple subscriptions. What should you use?

**Options:**
- A) Network Security Groups (NSGs)
- B) Azure Firewall
- C) Web Application Firewall (WAF)
- D) ExpressRoute

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** Azure Firewall is a managed, cloud-native network firewall service providing central packet filtering across VNets and subscriptions with threat intelligence.
</details>

### Q67. [HARD] Internal Load Balancer
**Scenario:** An application tiered architecture consists of Web VMs and Database VMs. You want to load balance traffic coming from the Web VMs to the Database VMs. You do NOT want the Database load balancer exposed to the internet. What do you deploy?

**Options:**
- A) A Public Load Balancer without an IP address.
- B) An Internal Load Balancer (ILB) with a private frontend IP address.
- C) Azure Front Door.
- D) An Application Gateway with a Public IP.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** Internal Load Balancers are designed specifically to balance traffic inside a virtual network. They use a private frontend IP address and are inaccessible from the outside world.
</details>

### Q68. [MEDIUM] Azure Front Door vs Traffic Manager
**Scenario:** You have a globally distributed web application. You require global HTTP/HTTPS routing, SSL offloading, and Web Application Firewall (WAF) protection at the global edge. Which service fits best?

**Options:**
- A) Azure Traffic Manager
- B) Azure Front Door
- C) Application Gateway
- D) Azure Load Balancer

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** Azure Front Door is an OSI Layer 7 global load balancer that provides SSL termination and WAF. Traffic Manager is a DNS-based (Layer 8) router that cannot do SSL offloading or WAF filtering.
</details>

### Q69. [EXPERT] Route Table Limitations
**Scenario:** You attach a custom Route Table to `SubnetA` to force all traffic through a virtual firewall appliance. You realize that traffic from `SubnetA` to Azure Storage is failing. How do you bypass the firewall JUST for Azure Storage traffic?

**Options:**
- A) This is not possible; UDRs affect all traffic universally.
- B) Provide a Service Tag for Storage in the Route Table to drop the route.
- C) You must disable the BGP propagation.
- D) Add a specific User Defined Route for the IP Address prefix of Azure Storage with Next Hop type "Internet".

<details>
<summary><b>Show Answer</b></summary>

**Answer:** D

**Explanation:** Azure evaluates routes based on Longest Prefix Match. By adding a specific route for the Azure Storage IPs pointing to "Internet" (which in Azure actually means standard Azure backbone flow for public services), it overrides the 0.0.0.0/0 default UDR. Using Service Endpoints also forces traffic directly to storage, effectively bypassing a 0.0.0.0/0 UDR.
</details>

### Q70. [MEDIUM] Network Watcher - Next Hop
**Scenario:** You need to find out exactly where a packet sent from a VM to address 10.0.5.5 is going to be routed. Which Network Watcher tool do you use?

**Options:**
- A) IP Flow Verify
- B) Next Hop
- C) Packet Capture
- D) VPN Troubleshoot

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** The "Next Hop" tool tells you the next routing destination for a packet (e.g., Virtual Appliance, Internet, Virtual Network) and helps identify if traffic is being misrouted due to a UDR.

</details>

---

## Domain 5: Monitor and Maintain Azure Resources (Questions 71 - 85)

### Q71. [EASY] Azure Monitor vs Log Analytics
**Scenario:** You need to query the historical event logs of 10 virtual machines using the Kusto Query Language (KQL) to find a specific error code. Where does Azure Monitor store this data for you to query?

**Options:**
- A) Azure Storage Table
- B) Log Analytics Workspace
- C) Application Insights
- D) Activity Log

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** Azure Monitor collects log data and stores it in a Log Analytics Workspace, where it can be queried incredibly fast using KQL.
</details>

### Q72. [MEDIUM] Azure Activity Log
**Scenario:** A developer accidentally deleted a critical Resource Group. You need to find out *who* deleted it and *when*. Which logging tool provides this exact information natively?

**Options:**
- A) Log Analytics Workspace
- B) System Event Logs on the VMs
- C) Azure Activity Log
- D) Microsoft Entra ID Sign-in logs

<details>
<summary><b>Show Answer</b></summary>

**Answer:** C

**Explanation:** The Azure Activity Log is a massive audit trail of all control-plane actions taken on resources in your subscription (e.g., who created/modified/deleted a resource, and at what time).
</details>

### Q73. [HARD] Alerts and Action Groups
**Scenario:** You have configured an Alert Rule in Azure Monitor: "If CPU > 90%, trigger an alert." You want this alert to automatically send an SMS to the IT team AND trigger an Azure Function to restart the VM. What must you configure?

**Options:**
- A) Two separate Alert Rules.
- B) An Action Group attached to the Alert Rule.
- C) Azure Logic Apps with a time trigger.
- D) Event Grid topics.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** An Action Group is a collection of notification preferences (email, SMS, push) and automated actions (run a Runbook, call an Azure Function, trigger a Webhook) that are executed when an Alert is triggered.
</details>

### Q74. [EASY] Application Insights
**Scenario:** You have a custom web application hosted on an Azure App Service. Users complain the website is randomly throwing "HTTP 500" errors. You want a tool that maps the application dependencies and highlights which specific code module is failing. What should you use?

**Options:**
- A) Azure Network Watcher
- B) Application Insights
- C) Log Analytics
- D) VM Diagnostics Extension

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** Application Insights is an APM (Application Performance Management) tool designed for web developers to monitor live applications. It automatically maps out dependencies and traces requests to find exactly where code is breaking.
</details>

### Q75. [MEDIUM] Azure Backup Vault vs Recovery Services Vault
**Scenario:** You want to back up an Azure PostgreSQL database. When you go into your existing Recovery Services Vault, you do not see the option for PostgreSQL. Why?

**Options:**
- A) PostgreSQL is not supported for backup in Azure.
- B) You must use an Azure Backup Vault, as Recovery Services Vaults are only for VMs, SQL in VMs, and Azure Files.
- C) You must enable it via Azure Policy first.
- D) You are using the Free tier of Azure.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** Azure now has two distinct types of vaults. Recovery Services Vaults handle classic backups (VMs, SQL on VMs, Azure Files). *Backup Vaults* handle newer managed PaaS workloads like Azure Database for PostgreSQL, Managed Disks, and Azure Blobs. 
</details>

### Q76. [HARD] Soft Delete for Backups
**Scenario:** A hacker gains "Contributor" access to your Azure subscription and maliciously deletes your Backup vaults and recovery points to ruin the company. However, they are blocked for 14 days, giving you time to recover the data. What feature saved the company?

**Options:**
- A) Resource Locks
- B) Multi-Factor Authentication (MFA)
- C) Soft Delete for Azure Backup
- D) Azure Policy

<details>
<summary><b>Show Answer</b></summary>

**Answer:** C

**Explanation:** Soft Delete for Azure Backup (enabled by default) protects backup data from accidental or malicious deletion. Even if an attacker explicitly deletes the backups, Azure retains the recovery points for 14 additional days for free before permanently destroying them.
</details>

### Q77. [EXPERT] Azure Site Recovery (ASR)
**Scenario:** You are configuring Azure Site Recovery (ASR) to replicate a VM from the "East US" region to the "West US" region for disaster recovery. The VM has 3 managed disks. Which of the following is true regarding how the data is replicated?

**Options:**
- A) ASR creates a snapshot every 24 hours and copies it to West US.
- B) ASR installs a Mobility Service extension on the VM that captures all data writes in memory and continuously streams them to a Cache storage account in the source region, which then replicates to the target region.
- C) ASR uses VNet Peering to synchronize the disks.
- D) ASR relies entirely on Geo-Redundant Storage (GRS).

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** ASR uses continuous replication, not daily snapshots. The Mobility Service intercepts disk writes and sends them to a local cache storage account before pushing them across the Azure backbone to the target region, keeping the Recovery Point Objective (RPO) within minutes.
</details>

### Q78. [MEDIUM] Diagnostic Settings
**Scenario:** You have a Key Vault. By default, it does not keep a long-term log of who accessed which secrets. How do you configure it so all access logs for the Key Vault are permanently archived to an Azure Storage account?

**Options:**
- A) Create a Diagnostic Setting on the Key Vault targeting the Storage account.
- B) Enable Azure Defender.
- C) Use Azure Activity Log.
- D) Write an Azure Function to poll the metrics.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** A

**Explanation:** Diagnostic Settings allow you to stream resource-level logs (data-plane logs, like who accessed a secret) to destinations like Log Analytics, Event Hubs, or a Storage Account for long-term retention.
</details>

### Q79. [EASY] Recovery Point Objective (RPO)
**Scenario:** In disaster recovery planning, business leadership asks for an RPO of 1 hour. What does RPO mean in this context?

**Options:**
- A) The maximum amount of time it takes to boot the backup server (1 hour).
- B) The maximum acceptable amount of data loss resulting from the disaster (e.g., losing the last 1 hour of data).
- C) The cost of the backup.
- D) The network latency between the two sites.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** Recovery Point Objective (RPO) defines your tolerance for data loss (how old the backup is). Recovery Time Objective (RTO) refers to how fast you can get the systems back online and functioning.
</details>

### Q80. [MEDIUM] Azure Monitor Metrics
**Scenario:** You want to monitor the "Percentage CPU" of an Azure VM and set up an alert if it exceeds 80%. When you look at the metrics screen, the highest granularity (frequency) you can select is 1 minute. Why can't you select 1 second?

**Options:**
- A) Azure metrics only support 5-minute intervals.
- B) You need to install a custom agent for 1-second metrics.
- C) The default Azure Monitor host-level platform metrics are collected every 1 minute.
- D) You must be on the Premium tier of Azure Monitor.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** C

**Explanation:** By default, Azure platform metrics are collected and aggregated at a 1-minute frequency. For sub-minute metrics (like 1-second), you generally need custom telemetry via Application Insights or a specialized agent, though 1-minute is the standard baseline.
</details>

### Q81. [HARD] VM Insights
**Scenario:** You want to see a topological map of all the processes running on a group of VMs and identify which specific external IP addresses those processes are communicating with. What Azure Monitor feature provides this?

**Options:**
- A) Network Security Group Flow Logs
- B) Network Watcher Next Hop
- C) VM Insights (specifically the Map feature)
- D) Azure Firewall Analytics

<details>
<summary><b>Show Answer</b></summary>

**Answer:** C

**Explanation:** VM Insights uses the Dependency Agent (along with the Azure Monitor agent) to discover running processes and network dependencies, visualizing them in a "Map". It shows exactly what processes are talking to what IPs/Ports.
</details>

### Q82. [MEDIUM] Backup Policy
**Scenario:** You need to create a backup policy for your VMs. You are required to keep daily backups for 30 days, weekly backups for 52 weeks, and monthly backups for 5 years. Does Azure Backup support this complex retention scheme in a single policy?

**Options:**
- A) Yes, utilizing the GFS (Grandfather-Father-Son) retention rules built into standard backup policies.
- B) No, you must create three separate policies.
- C) Yes, but only if you use a script.
- D) No, Azure Backup caps retention at 99 years but only supports one type of frequency at a time.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** A

**Explanation:** Azure Backup natively supports the GFS (Grandfather-Father-Son) retention model in a single policy, allowing you to seamlessly configure overlapping daily, weekly, monthly, and yearly retention limits.
</details>

### Q83. [EASY] Multi-Region Recovery
**Scenario:** You want to back up an Azure VM located in the East US region. Can you use a Recovery Services Vault created in the West US region to store the backup?

**Options:**
- A) Yes.
- B) No.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** No. A Recovery Services Vault must reside in the EXACT same region as the Virtual Machine it is protecting. However, you can enable Cross-Region Restore (CRR) on the vault to allow restoring the data to a paired region if the primary region dies.
</details>

### Q84. [HARD] Azure Alerts Processing Rules
**Scenario:** Your web application throws thousands of automated alerts every Saturday night from 1 AM to 4 AM due to a known patching window. It constantly wakes up the on-call engineer. How do you silence the notifications during this window without disabling the actual alert rules?

**Options:**
- A) Use an Azure Automation Runbook to delete the Action Group every Saturday.
- B) Create an Alert Processing Rule (formerly Action Rules) to suppress notifications during that schedule.
- C) Turn off the VM.
- D) Change the alert severity to Level 4.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** Alert Processing Rules allow you to apply logic to fired alerts, such as suppressing the triggering of Action Groups (like SMS/Emails) during a specific scheduled maintenance window, without having to disable the underlying alert setup.
</details>

### Q85. [MEDIUM] Restoring Files from VM Backup
**Scenario:** You backed up a Windows VM. A user accidentally deleted an important Word document from the C:\ drive. What is the fastest way to recover just that specific document without restoring the entire VM?

**Options:**
- A) You cannot; you must restore the entire VM to a new disk.
- B) Perform a "File Recovery" from the Azure portal, which provides an executable script to mount the recovery point as a local drive on your PC so you can copy the file.
- C) Use Azure File Sync.
- D) Contact Microsoft Support to extract it.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** Azure VM Backup includes a "File Recovery" feature. It generates a temporary script and password that securely mounts the specific backup recovery point as an iSCSI drive on a local machine, allowing you to browse and copy individual files rapidly.

</details>

---

## Domain 6: Advanced Scenarios - Automation, Cost, & Hybrid (Questions 86 - 100)

### Q86. [MEDIUM] Azure Automation Runbooks
**Scenario:** You have a PowerShell script that parses all Resource Groups and shuts down unused VMs. You want to run this automatically in the cloud every Friday at 7 PM. What Azure service should you use?

**Options:**
- A) Azure Batch
- B) Azure Automation (Runbooks)
- C) Event Grid
- D) Azure DevOps

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** Azure Automation allows you to host PowerShell and Python scripts as "Runbooks" and execute them on a schedule in a serverless manner, perfectly suited for cloud operations tasks.
</details>

### Q87. [EASY] Hybrid Runbook Worker
**Scenario:** You created an Azure Automation Runbook, but it needs to execute a script against an on-premises VMware vSphere server that is NOT connected to the internet. How can the cloud Runbook execute code inside your private datacenter?

**Options:**
- A) You cannot; runbooks only run in the cloud.
- B) Use Azure Arc.
- C) Install the Hybrid Runbook Worker agent on a server in your on-premises datacenter.
- D) Expose the vSphere server to the internet.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** C

**Explanation:** The Hybrid Runbook Worker feature allows Azure Automation to execute runbooks directly on the machines located in your local data center, overcoming network boundaries safely.
</details>

### Q88. [HARD] State Configuration (DSC)
**Scenario:** You manage 50 Windows Server VMs. You want to ensure that if any administrator accidentally uninstalls the IIS Web Server role, Azure automatically detects it and reinstalls it continuously to enforce the desired configuration. What Azure Automation feature does this?

**Options:**
- A) Update Management
- B) Inventory Tracking
- C) State Configuration (Desired State Configuration / DSC)
- D) Azure Policy

<details>
<summary><b>Show Answer</b></summary>

**Answer:** C

**Explanation:** Azure Automation State Configuration is a management service that allows you to write, manage, and compile PowerShell DSC configurations. It actively monitors nodes and brings them back into compliance ("desired state") if they drift.
</details>

### Q89. [MEDIUM] Azure Cost Management
**Scenario:** The monthly Azure bill for the Development subscription has suddenly spiked. Your manager asks you to figure out exactly which specific Resource Group or Service (VMs vs Database) caused the spike. What tool do you use first?

**Options:**
- A) Azure Advisor
- B) Cost Assessment Dashboard in Azure Portal (Cost Management + Billing)
- C) Total Cost of Ownership (TCO) Calculator
- D) Azure Pricing Calculator

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** Azure Cost Management + Billing provides deep cost analysis breakdowns. You can group by Resource Group, Resource Type, or Tags to visually identify exactly what is spending the money over time.
</details>

### Q90. [EASY] Azure Budgets
**Scenario:** You want an email notification to be sent to you if the spending in your subscription exceeds $1,000 this month. Does setting an Azure Budget stop resources from being created?

**Options:**
- A) Yes, it cuts off the subscription at the budget amount.
- B) No, budgets track and alert on spending but do not physically block resources from running or being created.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** Budgets in Azure are purely for tracking and alerting. If you hit 100% of your $1,000 budget, Azure sends an alert, but your VMs keep running and you keep getting billed. To block resources automatically, you would need complex automation runbooks acting on the alert.
</details>

### Q91. [HARD] Cost Allocation via Tags
**Scenario:** Marketing, IT, and HR all share the same subscription. You need to pull a billing report that calculates exactly how much money Marketing spent this month. How should you structure this to succeed?

**Options:**
- A) Create three separate Entra ID Directories.
- B) Ensure all Marketing resources are tagged with `Department: Marketing`, then filter the Cost Analysis report by that tag.
- C) Look at the IP addresses of the resources.
- D) You cannot; subscriptions cannot be split in billing.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** Tags are the cornerstone of cross-subscription and shared-subscription cost allocation. By tagging every resource with `Department: Name`, Cost Management can accurately group and sum the billing data.
</details>

### Q92. [MEDIUM] Azure Cost Savings
**Scenario:** You have a massive SQL database VM that you know you will need to keep running 24/7 for the next 3 years. What is the most effective way to lower the compute cost of this VM without changing its size?

**Options:**
- A) Use B-series VMs.
- B) Purchase a 3-Year Azure Reserved Virtual Machine Instance.
- C) Apply a ReadOnly lock.
- D) Turn on Availability Zones.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** Reserved Instances allow you to commit to 1- or 3-year terms for a specific VM family and region in exchange for incredibly massive discounts (up to 72% off pay-as-you-go rates).
</details>

### Q93. [EASY] Azure Advisor
**Scenario:** You want a quick dashboard that analyzes your entire Azure environment and gives you actionable recommendations to improve Security, High Availability, Performance, and Costs. Which tool provides this?

**Options:**
- A) Azure Sentinel
- B) Azure Monitor
- C) Azure Advisor
- D) Network Watcher

<details>
<summary><b>Show Answer</b></summary>

**Answer:** C

**Explanation:** Azure Advisor is an automated consulting tool that continuously evaluates your deployments and identifies ways to optimize them across Cost, Security, Reliability, Operational Excellence, and Performance.
</details>

### Q94. [HARD] Import/Export Job
**Scenario:** You need to migrate 20 Terabytes of historical log data from on-premises over to an Azure Storage Blob. Your company's internet upload speed is 10 Mbps. Uploading it would take months. What is the fastest physical way to get the data to Azure?

**Options:**
- A) Azure Data Box / Import/Export Service
- B) Site-to-Site VPN
- C) Azure File Sync
- D) Compressing it into a ZIP file before fast-uploading.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** A

**Explanation:** The Azure Import/Export service (and the modern Azure Data Box family) allows you to securely transfer large amounts of data by shipping physical hard drives to an Azure datacenter, bypassing network bandwidth limitations completely.
</details>

### Q95. [MEDIUM] ARM Templates Outputs
**Scenario:** You have an ARM template that successfully creates an Azure SQL Database. You need the deployment to output the fully qualified domain name (FQDN) of the newly created server so your scripts can use it. Where in the ARM template structure do you write this?

**Options:**
- A) In the `parameters` section.
- B) In the `variables` section.
- C) In the `resources` section.
- D) In the `outputs` section.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** D

**Explanation:** ARM Templates (and Bicep) have a specific `outputs` block at the end of the JSON/Bicep file. This is used to capture and return values (like dynamic IP addresses or generated FQDNs) generated during the deployment back to the user or pipeline.
</details>

### Q96. [EXPERT] App Service Custom Domains and SSL
**Scenario:** You mapped a custom domain `www.myorg.com` to an Azure App Service using a CNAME record. Now you need to secure it with an SSL/TLS certificate. You don't want to buy a certificate, and you want Microsoft to manage the renewals automatically. What do you do?

**Options:**
- A) You must buy a certificate from a third-party vendor.
- B) Generate an App Service Managed Certificate for the custom domain.
- C) Keep using the `*.azurewebsites.net` certificate.
- D) Enable Azure Key Vault auto-sign.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** Azure offers App Service Managed Certificates completely for free. It is a fully managed, auto-renewing TLS/SSL certificate strictly for custom domains mapped to App Services.
</details>

### Q97. [HARD] Network Routing Diagnostics
**Scenario:** Users cannot reach a web server VM hosted in Azure. You verify the VM is running and the NSG rules appear correct. You suspect the VNet routing table is routing the traffic to a dead virtual firewall appliance. How do you verify the *Effective Routes* applied to the VM's network interface?

**Options:**
- A) Run `ipconfig /all` inside the VM.
- B) Go to the Network Interface (NIC) of the VM in the Portal, click "Effective routes".
- C) Use Azure Activity Log.
- D) Look at the default route table assigned to the VNet.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** User-Defined Routes (UDRs), BGP routes from ExpressRoute, and default Azure routes all merge into "Effective Routes". Viewing the Effective Routes directly on the NIC tells you the exact aggregate routing table that the VM is currently using.
</details>

### Q98. [EASY] File Share Backups
**Scenario:** What is the underlying technology that Azure Backup uses to take incredibly fast, non-disruptive backups of Azure File shares?

**Options:**
- A) Installing an agent on the storage account.
- B) Copying the files to a secondary account over the network.
- C) Azure File Share Snapshots.
- D) Zipping the directory.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** C

**Explanation:** Azure Backup integrates directly with Azure Files to take storage-level snapshots. Snapshots happen almost instantly because they just capture the state of the data blocks, without actually moving any data across the network.
</details>

### Q99. [MEDIUM] Storage Tiering Costs
**Scenario:** Moving a massive blob from the Hot tier to the Archive tier lowers your monthly storage cost. However, if you move it from Archive back to Hot, what kind of fee occurs?

**Options:**
- A) You are charged a Rehydration (read and write) fee, which can be expensive depending on priority.
- B) There is no fee; tier changes inside the same account are completely free.
- C) You must pay for an entire year of Hot storage up front.
- D) The blob degrades in quality.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** A

**Explanation:** While Archive is super cheap to store data, "Rehydrating" data (moving it from Archive back to Hot/Cool) incurs significant per-GB read charges, and doing a "High Priority" rehydration increases those charges further.
</details>

### Q100. [EASY] Role Definition Source
**Scenario:** You find a role in your Subscription called "Virtual Machine Contributor". How can you tell if this is a built-in role provided by Microsoft or a custom role created by your company?

**Options:**
- A) Custom roles are highlighted in red.
- B) Look at the `Type` column under Access Control (IAM) -> Roles, it will explicitly say "Built-in" or "Custom".
- C) Built-in roles cannot be viewed.
- D) Try to delete it. If it deletes, it was custom.

<details>
<summary><b>Show Answer</b></summary>

**Answer:** B

**Explanation:** The Azure Portal explicitly tags the "Type" of role in the IAM assignments and Roles list, differentiating Built-In Microsoft generic roles from Custom ones written by your organization.
</details>

---

## Final Summary 

Congratulations on reaching the end of the 100-question gauntlet!

If you were able to comfortably answer these scenarios and understand the *Why* behind the explanations, you are demonstrating strong first-principles knowledge of the Azure Administrator domains. 

**Key AZ-104 Reminders for the Exam:**
- **Identity:** Know the difference between P1/P2 licenses, RBAC inheritance (additive), and Policy (deny/audit).
- **Networking:** Know Service Endpoints vs Private Link, VNet Peering limits (non-transitive, no overlapping IPs), NSG ordering, and when to use Load Balancers vs Application Gateway.
- **Compute:** Understand Availability Sets (Update/Fault domains), Scale Sets (autoscaling), App Service, and ACI vs AKS.
- **Storage:** Remember tiering, redundancy setups (LRS vs ZRS vs GRS), and SAS tokens for delegated access.
- **Monitoring/Backup:** Understand Log Analytics Workspaces, Action Groups, and Recovery Services Vault boundaries.

Best of luck on your AZ-104 journey!
a