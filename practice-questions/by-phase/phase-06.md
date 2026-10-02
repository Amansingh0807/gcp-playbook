# Phase 06 Practice Drill — Networking Services

> **Total Questions:** 15 (10 MCQs + 5 MSQs)  
> **Topic Tag:** `#phase-06-networking`  
> **Target Score:** 13/15 (86%+)  
> **Covers:** VPC (Global, Custom Mode, Shared VPC), Subnets, Firewall Rules (Stateful, Priority, Targets), Cloud NAT, VPC Peering, Cloud Interconnect, HA VPN, Cloud Load Balancing.

---

## MCQ Section (Single Correct Answer)

---

### Q1. A company operates a fleet of private backend Compute Engine VMs with no external IP addresses. The VMs must download OS security patches from public Linux package repositories on the internet, but must never be directly accessible from the internet. What should you configure?

- A) Assign a static external IP address to every VM.
- B) Deploy a Cloud NAT gateway on a Cloud Router in the subnet's region.
- C) Establish a VPC Peering connection with Google's public network.
- D) Configure an Ingress firewall rule allowing `0.0.0.0/0` on port 80.

<details>
<summary>✅ Answer</summary>

**B) Deploy a Cloud NAT gateway on a Cloud Router in the subnet's region.**

**Explanation:** Cloud NAT provides managed outbound-only internet access for private VMs without exposing them to inbound internet connections. Assigning external IPs (A) unnecessarily expands the public attack surface. VPC Peering (C) connects private VPCs, not the public internet. Opening port 80 ingress (D) permits inbound traffic rather than enabling outbound updates.
</details>

---

### Q2. VPC-1 is peered directly with VPC-2. VPC-2 is peered directly with VPC-3. A developer on a VM in VPC-1 attempts to send ping requests to the internal IP of a database in VPC-3. What is the expected outcome?

- A) The connection succeeds because VPC peering is automatically transitive.
- B) The connection fails because VPC peering is strictly non-transitive.
- C) The connection succeeds only if all three VPCs belong to the same project.
- D) The connection succeeds only if dynamic BGP routing is enabled.

<details>
<summary>✅ Answer</summary>

**B) The connection fails because VPC peering is strictly non-transitive.**

**Explanation:** VPC Network Peering is non-transitive. Traffic cannot route through VPC-2 to reach VPC-3. For VPC-1 and VPC-3 to communicate privately, a direct peering connection must be established between them.
</details>

---

### Q3. Which VPC creation mode does Google Cloud recommend for enterprise production architectures?

- A) Auto Mode VPC
- B) Custom Mode VPC
- C) Shared Default VPC
- D) Dynamic Mesh VPC

<details>
<summary>✅ Answer</summary>

**B) Custom Mode VPC**

**Explanation:** Google explicitly recommends Custom Mode VPC for all production environments. Custom Mode gives network engineers complete control over subnet regions and non-overlapping CIDR blocks, preventing IP collisions when configuring hybrid VPN, Interconnect, or VPC peering later.
</details>

---

### Q4. In Google Cloud VPC firewall policy evaluation, which priority number takes precedence when multiple rules match?

- A) The lowest numerical value (e.g., Priority 100 beats Priority 1000).
- B) The highest numerical value (e.g., Priority 5000 beats Priority 500).
- C) Priority numbers have no effect; rules are evaluated randomly.
- D) Egress rules always evaluate before Ingress rules regardless of priority.

<details>
<summary>✅ Answer</summary>

**A) The lowest numerical value (e.g., Priority 100 beats Priority 1000).**

**Explanation:** Firewall rules are evaluated in ascending order of their priority integer (0 to 65535). Lower numerical values indicate higher evaluation precedence. The first rule that matches terminates evaluation.
</details>

---

### Q5. A financial organization requires multiple project teams to deploy workloads into their own independent projects, but mandates that a central network engineering team must manage all subnets, CIDR allocations, and firewall policies. What should be implemented?

- A) VPC Network Peering
- B) Shared VPC
- C) Cloud Interconnect
- D) Cloud NAT

<details>
<summary>✅ Answer</summary>

**B) Shared VPC**

**Explanation:** Shared VPC allows an organization to designate a central Host Project to manage the network topology (subnets, routes, firewalls) while attaching multiple Service Projects where development teams provision VMs into shared subnets.
</details>

---

### Q6. Which Google Cloud load balancer is required if an application requires a single global Anycast IP address, Layer 7 content-based URL routing, and edge SSL termination?

- A) Regional Internal Application Load Balancer
- B) Regional External Passthrough Network Load Balancer
- C) Global External Application Load Balancer
- D) Internal Passthrough Network Load Balancer

<details>
<summary>✅ Answer</summary>

**C) Global External Application Load Balancer**

**Explanation:** The Global External Application Load Balancer operates at Layer 7 (HTTP/HTTPS) across Google's global edge network, providing a single Anycast IP with URL path-based routing, HTTP header inspection, and edge SSL termination.
</details>

---

### Q7. What happens by default to inbound traffic attempting to reach a newly provisioned Compute Engine VM that has no custom firewall rules applied?

- A) All inbound traffic is accepted.
- B) All inbound traffic is dropped by the implied Deny Ingress rule.
- C) Only HTTP and HTTPS traffic are accepted.
- D) Only traffic from the same subnet is accepted.

<details>
<summary>✅ Answer</summary>

**B) All inbound traffic is dropped by the implied Deny Ingress rule.**

**Explanation:** Every Google Cloud VPC network possesses an implied default Ingress DENY rule at the lowest priority (65535) that drops all inbound packets unless an explicit higher-priority ALLOW rule permits them.
</details>

---

### Q8. An enterprise requires a dedicated 10 Gbps private physical circuit linking its Chicago data center directly to Google Cloud without routing packets over the public internet. What service should be deployed?

- A) High Availability Cloud VPN
- B) Dedicated Cloud Interconnect
- C) VPC Network Peering
- D) Cloud NAT Gateway

<details>
<summary>✅ Answer</summary>

**B) Dedicated Cloud Interconnect**

**Explanation:** Dedicated Cloud Interconnect provides physical 10 Gbps or 100 Gbps cross-connect circuits directly connecting enterprise on-premises data centers to Google's edge network, completely bypassing the public internet. Cloud VPN (A) routes encrypted traffic over the public internet and is limited to 3 Gbps per tunnel.
</details>

---

### Q9. An Ingress firewall rule with Priority 100 allows TCP port 80 traffic to a web server VM. What happens to the return HTTP response traffic sent back to the client?

- A) The return traffic is blocked unless an explicit Egress ALLOW rule is created for port 80.
- B) The return traffic is automatically allowed because GCP firewalls are stateful.
- C) The return traffic is dropped by the implied deny egress rule.
- D) The return traffic is routed through Cloud NAT automatically.

<details>
<summary>✅ Answer</summary>

**B) The return traffic is automatically allowed because GCP firewalls are stateful.**

**Explanation:** Google Cloud firewalls are stateful. When an Ingress connection is evaluated and allowed, all associated return/response packets are tracked and permitted automatically, regardless of egress firewall configurations.
</details>

---

### Q10. Which feature allows Compute Engine VMs without external IP addresses to securely access Cloud Storage buckets and BigQuery datasets using internal Google network paths?

- A) Cloud NAT
- B) Private Google Access
- C) Dedicated Interconnect
- D) External Passthrough Load Balancing

<details>
<summary>✅ Answer</summary>

**B) Private Google Access**

**Explanation:** Private Google Access is a subnet-level configuration that enables instances with only private RFC 1918 IPs to route to the public IP ranges of Google APIs and services (such as Cloud Storage and BigQuery) via Google's internal network backbone.
</details>

---

## MSQ Section (Multiple Correct Answers — Select ALL that apply)

---

### Q11. Which of the following statements regarding Google Cloud VPC networks and subnets are TRUE? (Select TWO)

- A) A VPC network is a global resource spanning all regions worldwide.
- B) A VPC network is confined strictly to a single availability zone.
- C) A subnet is a regional resource that spans all zones within its region.
- D) A subnet can span across multiple geographical regions.
- E) Subnets can be shrunk in size dynamically after creation.

<details>
<summary>✅ Answer</summary>

**A) A VPC network is a global resource spanning all regions worldwide, C) A subnet is a regional resource that spans all zones within its region.**

**Explanation:** In Google Cloud, a VPC network is global (spanning all regions), while subnets are regional constructs that span all availability zones within that single region. Subnets cannot cross into multiple regions (D is false), and subnet CIDR blocks can only be expanded, never shrunk (E is false).
</details>

---

### Q12. Which of the following are valid targeting mechanisms for Google Cloud VPC firewall rules? (Select TWO)

- A) Network Tags applied to VM instances
- B) Service Accounts attached to VM instances
- C) Google Workspace User Group emails
- D) Billing Account IDs
- E) Cloud Storage bucket names

<details>
<summary>✅ Answer</summary>

**A) Network Tags applied to VM instances, B) Service Accounts attached to VM instances**

**Explanation:** VPC firewall rules can target instances using: (1) All instances in the network, (2) Specified target tags, or (3) Specified target service accounts. User groups, billing accounts, and storage buckets cannot be targeted by VPC firewall rules.
</details>

---

### Q13. Which statements regarding Cloud NAT are TRUE? (Select TWO)

- A) It allows external clients on the internet to initiate inbound connections to private VMs.
- B) It provides outbound-only internet connectivity for instances lacking external IP addresses.
- C) It is a fully managed, software-defined service requiring no proxy instances.
- D) It requires every VM to be assigned a dedicated static public IP address.
- E) It can only translate traffic within a single availability zone.

<details>
<summary>✅ Answer</summary>

**B) It provides outbound-only internet connectivity for instances lacking external IP addresses, C) It is a fully managed, software-defined service requiring no proxy instances.**

**Explanation:** Cloud NAT is a managed SDN translation service that delivers outbound-only connectivity without running proxy VMs. It does not permit inbound-initiated connections (A is false), and eliminates the requirement for per-VM public IPs (D is false).
</details>

---

### Q14. Which of the following load balancer types operate at Layer 7 of the OSI model? (Select TWO)

- A) Global External Application Load Balancer
- B) Internal Application Load Balancer
- C) External Passthrough Network Load Balancer
- D) Internal Passthrough Network Load Balancer
- E) SSL Proxy Load Balancer

<details>
<summary>✅ Answer</summary>

**A) Global External Application Load Balancer, B) Internal Application Load Balancer**

**Explanation:** Application Load Balancers operate at Layer 7 (HTTP/HTTPS), supporting URL maps, path-based routing, and header processing. Passthrough Network Load Balancers and SSL/TCP Proxies operate at Layer 4 (Transport layer).
</details>

---

### Q15. Which statements regarding VPC Network Peering are TRUE? (Select TWO)

- A) Peered networks exchange internal RFC 1918 traffic without traversing the public internet.
- B) Peering relationships are transitive across intermediate networks.
- C) Peered VPC networks cannot possess overlapping subnet CIDR blocks.
- D) Peering automatically merges the IAM permission policies of both projects.
- E) Peering requires provisioning an intermediate Cloud Router in every zone.

<details>
<summary>✅ Answer</summary>

**A) Peered networks exchange internal RFC 1918 traffic without traversing the public internet, C) Peered VPC networks cannot possess overlapping subnet CIDR blocks.**

**Explanation:** VPC Network Peering enables direct, private internal communication across Google's internal fabric and strictly forbids overlapping subnet IP ranges. Peering is non-transitive (B is false), does not affect IAM policies (D is false), and does not require Cloud Routers (E is false).
</details>

---

## 📊 Score Tracker

| Score | Verdict |
| :---: | :--- |
| **15/15** | 🏆 **Network Architect Master:** Completely exam-ready for Google Cloud networking. |
| **13–14** | ✅ **Solid Readiness:** Minor review of load balancer types or stateful firewall rules needed. |
| **10–12** | ⚠️ **Needs Review:** Re-read Section 4 (VPC Peering & Cloud NAT) and Section 6.2 (Load Balancer Matrix). |
| **< 10** | 🔄 **Re-study Phase 06:** Carefully study firewall priority rules, peering non-transitivity, and Load Balancing layers. |
