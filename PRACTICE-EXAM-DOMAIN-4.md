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

