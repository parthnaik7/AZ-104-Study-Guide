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

