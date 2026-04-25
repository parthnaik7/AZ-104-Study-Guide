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

