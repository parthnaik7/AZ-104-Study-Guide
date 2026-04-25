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

