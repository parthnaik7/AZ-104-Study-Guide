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