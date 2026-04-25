# Module 6: Monitor and Maintain Azure Resources

Once your infrastructure is running, you must ensure it stays healthy and that data is never lost. The AZ-104 exam focuses on differentiating between *monitoring* tools and choosing the right *backup* architecture.

---

## 1. Azure Monitor

Azure Monitor is the central hub for collecting, analyzing, and acting on telemetry from your cloud and on-premises environments.

### Metrics vs. Logs (The Core Difference)

```mermaid
graph TD
    subgraph Azure Monitor
        subgraph Metrics [Metrics (Numbers)]
            M1[CPU %]
            M2[Disk I/O]
            M3[Network Traffic]
        end
        subgraph Logs [Logs (Text/Events)]
            L1[Event Viewer Logs]
            L2[Syslog]
            L3[App Exceptions]
        end
    end
    
    Metrics --> Action1[Trigger Alerts Instantly]
    Logs --> Action2[Query using KQL in Log Analytics Workspace]
    
    style Metrics fill:#fff9c4,stroke:#fbc02d
    style Logs fill:#e1bee7,stroke:#8e24aa
```

- **Metrics:** Numerical values at a point in time (e.g., "CPU is at 85%"). They are lightweight, near real-time, and used to trigger fast alerts (e.g., Auto-scaling rules).
- **Logs:** Detailed text records of events that occurred. You send these to a **Log Analytics Workspace**, where you write queries using **KQL (Kusto Query Language)** to find root causes of complex issues.

### Alerts and Action Groups
When a Metric or Log hits a certain threshold, an Alert fires. The Alert triggers an **Action Group**.
- **Action Group:** Defines *what* happens when the alert fires (e.g., Send an SMS to the IT Admin, email a distribution list, trigger an Azure Function, or call a Webhook).

---

## 2. Azure Backup

Azure Backup is a centralized service to back up your data to a **Recovery Services Vault**. The exam heavily tests the difference between the two main methods of backing up a Virtual Machine.

### Method A: The Azure VM Agent Extension (Native)
When you back up an Azure VM directly from the portal, Azure silently installs a VM Extension into the OS.
- Coordinates with the underlying Hypervisor to take a "snapshot" of the whole VM disk.
- **Pros:** No manual agent installation, backs up the entire VM state.

### Method B: The MARS Agent (Microsoft Azure Recovery Services)
If you want to back up an *on-premises* physical server or VM to Azure, or if you only want to back up specific files/folders on an Azure VM instead of the whole disk, you manually install the MARS Agent.
- **Pros:** Granular file/folder backup, works on-premises without a local backup server.
- **Cons:** Can only back up Windows machines (no Linux), does not back up the full OS state.

> [!WARNING]
> **Exam Gotcha:** If the scenario asks how to back up an on-premises *Linux* machine directly to Azure without an intermediate backup server, the answer is "You cannot use the MARS agent" (MARS is Windows only). You would need Microsoft Azure Backup Server (MABS).

---

## 3. Azure Site Recovery (ASR)

Backup is for recovering deleted data. **Disaster Recovery (DR)** is for keeping the business online when a whole datacenter burns down.

Azure Site Recovery (ASR) continuously replicates your VMs from a Primary Region (e.g., East US) to a Secondary Region (e.g., West US).
- If East US goes completely offline, you trigger a "Failover" in ASR, and your VMs are booted up in West US within minutes.

> [!IMPORTANT]
> **Exam Gotcha:** Backup = Recovery Services Vault. Disaster Recovery / Replication = Azure Site Recovery.

---

## 4. Azure Advisor

Azure Advisor is a free, personalized cloud consultant. It analyzes your entire Azure environment and provides recommendations in five categories:
1. **Reliability:** E.g., "Your VM is not in an Availability Set."
2. **Security:** E.g., "You have port 3389 open to the internet."
3. **Cost:** E.g., "This VM has 1% CPU usage, you should downsize it."
4. **Performance:** E.g., "Add an index to your SQL database."
5. **Operational Excellence:** E.g., "Create an Azure Service Health alert."

---

## 5. Portal Walkthrough: "Where to Click"

* **To create an Action Group for Alerts:**
  * Navigate to `Monitor` -> Click `Alerts` on the left -> Click `Action groups` at the top -> Click `+ Create` -> Under Notifications, select `Email/SMS/Push/Voice` and enter the Admin's phone number.
* **To configure a VM Backup:**
  * Navigate to your Virtual Machine -> Click `Backup` under the Operations section on the left menu -> Create a new Recovery Services Vault (or select an existing one) -> Choose a Backup Policy (e.g., Daily at 2 AM, retain for 30 days) -> Click `Enable backup`.
* **To view Cost saving recommendations:**
  * Search the top bar for `Advisor` -> Click on the `Cost` tile to see idle or underutilized resources.

---

## 6. CLI & PowerShell Cheatsheet

### PowerShell
```powershell
# Create a new Recovery Services Vault
New-AzRecoveryServicesVault -ResourceGroupName "MyRG" -Name "MyBackupVault" -Location "EastUS"

# Set the context to the vault so subsequent backup commands know where to go
Get-AzRecoveryServicesVault -Name "MyBackupVault" | Set-AzRecoveryServicesVaultContext
```

### Azure CLI
```bash
# Create a Log Analytics Workspace
az monitor log-analytics workspace create --resource-group "MyRG" --workspace-name "MyLogWorkspace"

# Create a Recovery Services Vault
az backup vault create --resource-group "MyRG" --name "MyBackupVault" --location "eastus"
```
