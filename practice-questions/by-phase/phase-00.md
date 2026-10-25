# Phase 00 Practice Drill - Cloud Computing Fundamentals

> **Total Questions:** 15 (10 MCQs + 5 MSQs)  
> **Topic Tag:** `#phase-00-cloud-fundamentals`  
> **Target Score:** 13/15 (86%+)  
> **Covers:** NIST Characteristics, Cloud Service Models (IaaS, PaaS, SaaS), Deployment Models, Shared Responsibility Model, GCP Regions vs Zones.

---

## Multiple Choice Questions (MCQs - Single Answer)

### Question 1 [NIST Characteristics]
Which of the following is NOT one of the five essential characteristics of cloud computing as defined by NIST?
- [ ] A. Rapid elasticity
- [ ] B. Measured service
- [ ] C. Manual provisioning
- [ ] D. Resource pooling

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `C`

**Why it's correct:**
NIST defines the first core characteristic as **On-demand self-service**, which explicitly eliminates the need for manual human provisioning. A customer can provision compute and storage automatically without human interaction with the service provider.

**Why other options are incorrect:**
- **A (Rapid elasticity):** This is a valid NIST characteristic describing the ability to dynamically scale resources up and down.
- **B (Measured service):** This is a valid NIST characteristic describing usage metering and pay-as-you-go billing.
- **D (Resource pooling):** This is a valid NIST characteristic describing multi-tenant infrastructure sharing with logical isolation.

> **Key Exam Takeaway:** Remember the 5 NIST pillars: On-demand self-service, Broad network access, Resource pooling, Rapid elasticity, and Measured service.
</details>

---

### Question 2 [Cloud Service Models]
A software development startup wants to deploy a new web application. They do not want to manage operating systems, security patches, or server scaling, but they need complete control over application business logic and code execution. Which cloud service model best fits their requirements?
- [ ] A. Infrastructure as a Service (IaaS)
- [ ] B. Platform as a Service (PaaS)
- [ ] C. Software as a Service (SaaS)
- [ ] D. Colocation

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**
**Platform as a Service (PaaS)** (such as Google App Engine or Cloud Run) abstracts away the underlying operating system, server hardware, virtualization, and runtime maintenance, leaving the developer in complete control of application code and configurations.

**Why other options are incorrect:**
- **A (IaaS):** Compute Engine (IaaS) requires the customer to configure, patch, and maintain the guest operating system and installed software runtimes.
- **C (SaaS):** Google Workspace (SaaS) provides ready-to-use software where users have no control over application code or internal business logic.
- **D (Colocation):** Involves renting physical rack space in a data center while still purchasing and managing physical servers.

> **Key Exam Takeaway:** Code-only focus with zero OS patching = PaaS / Serverless.
</details>

---

### Question 3 [Shared Responsibility Model]
Under the Google Cloud Shared Responsibility Model, which operational responsibility ALWAYS remains with the customer regardless of whether they choose IaaS, PaaS, or SaaS?
- [ ] A. Physical data center perimeter security
- [ ] B. Hypervisor patch management
- [ ] C. Data classification and Access Control (IAM)
- [ ] D. Storage disk hardware recycling

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `C`

**Why it's correct:**
Across all cloud models (even SaaS like Google Workspace), the customer is **always** responsible for customer data classification, access governance, and Identity and Access Management (IAM). Google never decides who is permitted to view or access your data.

**Why other options are incorrect:**
- **A, B, D:** Physical facility security, hypervisors, and physical disk disposal are always Google's responsibility.

> **Key Exam Takeaway:** You ALWAYS own your data and who has access to it.
</details>

---

### Question 4 [Deployment Models]
An enterprise wants to use Google Cloud BigQuery for advanced machine learning analytics while retaining its customer transactional database on Amazon Web Services (AWS). Which cloud deployment model does this architecture represent?
- [ ] A. Hybrid Cloud
- [ ] B. Multi-Cloud
- [ ] C. Private Cloud
- [ ] D. Community Cloud

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**
**Multi-cloud** is defined as the concurrent use of two or more distinct public cloud providers (e.g., GCP + AWS) to distribute workloads or utilize specialized services.

**Why other options are incorrect:**
- **A (Hybrid Cloud):** Hybrid cloud specifically describes the combination of on-premises private data centers with a public cloud provider.
- **C (Private Cloud):** Dedicated infrastructure utilized exclusively by one organization.
- **D (Community Cloud):** Infrastructure shared by specific organizations with common concerns (e.g., regulatory or compliance).

> **Key Exam Takeaway:** Public + Public = Multi-Cloud. Public + On-Premises = Hybrid Cloud.
</details>

---

### Question 5 [GCP Infrastructure]
Which of the following statements correctly describes a unique networking differentiator of Google Cloud Platform compared to other major cloud providers?
- [ ] A. Virtual Private Cloud (VPC) networks are zonal resources
- [ ] B. Virtual Private Cloud (VPC) networks are regional resources
- [ ] C. Virtual Private Cloud (VPC) networks are global resources
- [ ] D. Subnets can span across multiple regions

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `C`

**Why it's correct:**
In Google Cloud, a **VPC network is a global resource**. A single VPC network can span every Google Cloud region across the planet without requiring cross-region VPN tunnels or complex inter-region peering configurations.

**Why other options are incorrect:**
- **A & B:** VPCs in GCP are neither zonal nor regional; they are global.
- **D:** Subnets in GCP are **regional** resources; individual subnets cannot span multiple regions.

> **Key Exam Takeaway:** VPC Network = Global. Subnets = Regional. Compute Instances = Zonal.
</details>

---

### Question 6 [Cloud Economics]
A company migrating its workloads from a private data center to Google Cloud eliminates the need to purchase physical Dell server racks and SAN arrays every 3 years. This financial shift is best described as:
- [ ] A. Converting OpEx to CapEx
- [ ] B. Converting CapEx to OpEx
- [ ] C. Fixed cost amortization
- [ ] D. Depreciation acceleration

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**
Cloud computing converts **Capital Expenditure (CapEx)** (large upfront purchases of physical assets) into **Operational Expenditure (OpEx)** (ongoing day-to-day operational costs based on actual consumption).

**Why other options are incorrect:**
- **A:** This is the exact reverse of what cloud computing achieves.
- **C & D:** These are standard accounting practices for depreciating physical hardware already owned.

> **Key Exam Takeaway:** Cloud computing transforms CapEx into OpEx.
</details>

---

### Question 7 [Managed Databases]
Your team needs a relational MySQL database for a production web application. To minimize operational overhead, you want automated patching, automatic daily backups, and automated high-availability cross-zone failover without managing Linux virtual machines. Which service should you choose?
- [ ] A. Compute Engine running MySQL
- [ ] B. Cloud SQL
- [ ] C. Cloud Bigtable
- [ ] D. Firestore

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**
**Cloud SQL** is Google Cloud's fully managed relational database service supporting MySQL, PostgreSQL, and SQL Server. Google manages the OS, database engine patching, replication, automated backups, and HA failover.

**Why other options are incorrect:**
- **A (Compute Engine):** Self-hosting MySQL on a VM requires the customer to configure, patch, and manage HA failover manually (IaaS).
- **C (Bigtable):** A managed NoSQL wide-column database, not relational MySQL.
- **D (Firestore):** A managed NoSQL document database, not relational MySQL.

> **Key Exam Takeaway:** Managed relational DB = Cloud SQL (or Cloud Spanner for global horizontal scale).
</details>

---

### Question 8 [NIST Resource Pooling]
Which of the following best characterizes the NIST cloud characteristic of "Resource Pooling"?
- [ ] A. Physical servers are dedicated solely to a single customer tenant at all times.
- [ ] B. Multiple customers dynamically share physical hardware while remaining logically isolated.
- [ ] C. Customers can manually request hardware allocations through support tickets.
- [ ] D. Resources can only be pooled within a single localized data center rack.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**
Resource pooling refers to a **multi-tenant model** where a provider's computing resources are pooled together to serve multiple customers dynamically, with virtual boundaries ensuring strict tenant isolation.

**Why other options are incorrect:**
- **A:** Describes dedicated single-tenancy (e.g., Sole-tenant nodes).
- **C:** Violates the principle of on-demand self-service.
- **D:** Resource pooling occurs globally across massive data centers and regions.

> **Key Exam Takeaway:** Resource pooling = multi-tenancy with secure logical isolation.
</details>

---

### Question 9 [Cost Optimization]
You run several stateless batch computation virtual machines on Compute Engine that run continuously for 24 hours a day, 7 days a week. You have not made any contractual commitment with Google. Which discount will Google Cloud automatically apply to your monthly invoice?
- [ ] A. Committed Use Discount (CUD)
- [ ] B. Sustained Use Discount (SUD)
- [ ] C. Spot VM Discount
- [ ] D. Free Tier Waiver

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**
**Sustained Use Discounts (SUDs)** are applied **automatically** by Google Cloud when eligible Compute Engine instances run for more than 25% of a billing month. They require zero commitments and no contracts.

**Why other options are incorrect:**
- **A (CUD):** Requires an explicit 1-year or 3-year contractual commitment.
- **C (Spot VM):** Requires configuring instances as Spot/Preemptible, which can be terminated at any time.
- **D:** Free Tier covers only specific micro-instance allocations.

> **Key Exam Takeaway:** Automatic discount without commitment = Sustained Use Discount (SUD).
</details>

---

### Question 10 [Deployment Models]
A national defense agency requires that its mission data be hosted on physical hardware completely isolated from all other commercial organizations, situated in an air-gapped environment with no connection to the public internet. Which cloud model must be implemented?
- [ ] A. Public Cloud
- [ ] B. Private Cloud
- [ ] C. Hybrid Cloud
- [ ] D. Multi-Cloud

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**
A **Private Cloud** provides dedicated, single-tenant infrastructure operated solely for a single enterprise, fulfilling strict regulatory, sovereignty, or national security isolation requirements.

**Why other options are incorrect:**
- **A (Public Cloud):** Multi-tenant infrastructure connected to the public internet.
- **C & D:** Both involve public cloud infrastructure.

> **Key Exam Takeaway:** Dedicated, exclusive infrastructure for one entity = Private Cloud.
</details>

---

## Multiple Select Questions (MSQs - Multiple Correct Answers)

### Question 11 [NIST Characteristics]
Which of the following are among the five essential NIST characteristics of cloud computing? (Select TWO)
- [ ] A. Rapid elasticity
- [ ] B. Broad network access
- [ ] C. Manual capacity reservation
- [ ] D. Sunk cost capitalization
- [ ] E. Vendor lock-in

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answers:** `A` and `B`

**Why they are correct:**
Rapid elasticity and Broad network access are two of the five canonical NIST characteristics.

**Why other options are incorrect:**
- **C:** Manual capacity planning is an on-premises limitation, contradicting on-demand self-service.
- **D & E:** These are business or risk considerations, not NIST characteristics.

> **Key Exam Takeaway:** Mnemonic: "On Broad Roads, Rapid Pool Measures".
</details>

---

### Question 12 [Service Model Categorization]
Which of the following Google Cloud services are categorized as Platform as a Service (PaaS) or Serverless? (Select TWO)
- [ ] A. Google Compute Engine
- [ ] B. Google App Engine
- [ ] C. Google Cloud Run
- [ ] D. Persistent Disk
- [ ] E. Sole-tenant Nodes

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answers:** `B` and `C`

**Why they are correct:**
- **App Engine** is Google Cloud's premier PaaS offering.
- **Cloud Run** is a fully managed serverless container execution platform.

**Why other options are incorrect:**
- **A & E:** Compute Engine and Sole-tenant nodes are Infrastructure as a Service (IaaS).
- **D:** Persistent Disk is block storage infrastructure (IaaS layer).

> **Key Exam Takeaway:** App Engine and Cloud Run abstract server management away.
</details>

---

### Question 13 [Shared Responsibility]
Which of the following statements regarding the Shared Responsibility Model are TRUE? (Select TWO)
- [ ] A. Google Cloud is responsible for the physical security of data center facilities across all models.
- [ ] B. The customer is responsible for patching the underlying hypervisor in IaaS.
- [ ] C. In SaaS, the customer is relieved of all security and access governance obligations.
- [ ] D. Customer data classification and IAM access control remain the customer's responsibility in all models.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answers:** `A` and `D`

**Why they are correct:**
- **A:** Google Cloud always secures physical data centers, cooling, generators, and physical fiber.
- **D:** The customer always retains full responsibility for their data and who is granted access permissions.

**Why other options are incorrect:**
- **B:** Google always patches and manages the hypervisor; the customer patches the guest OS.
- **C:** Customers remain responsible for access control and their own data even in SaaS.

> **Key Exam Takeaway:** Data and IAM access = Customer. Physical facilities = Google.
</details>

---

### Question 14 [Deployment Models]
Which of the following are recognized cloud deployment models? (Select THREE)
- [ ] A. Public Cloud
- [ ] B. Private Cloud
- [ ] C. Hybrid Cloud
- [ ] D. Serverless Cloud
- [ ] E. Dynamic Cloud

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answers:** `A`, `B`, and `C`

**Why they are correct:**
Public Cloud, Private Cloud, and Hybrid Cloud (along with Multi-Cloud/Community Cloud) are the standard deployment models.

**Why other options are incorrect:**
- **D & E:** "Serverless" is an execution model/service paradigm, not a deployment model. "Dynamic Cloud" is not a recognized cloud taxonomy term.

> **Key Exam Takeaway:** Deployment models describe where and for whom infrastructure is deployed.
</details>

---

### Question 15 [Cloud Billing Pitfalls]
Which of the following represent common Google Cloud billing mistakes made by beginners? (Select TWO)
- [ ] A. Reserving static external IP addresses and leaving them unattached to any running instance.
- [ ] B. Forgetting to delete test Compute Engine VM instances after completing testing.
- [ ] C. Enabling Sustained Use Discounts on running instances.
- [ ] D. Setting threshold alerts on monthly budgets.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answers:** `A` and `B`

**Why they are correct:**
- **A:** Unattached static external IP addresses incur an hourly charge to prevent IP address reservation hoarding.
- **B:** Running idle Compute Engine instances continue to accrue compute and persistent disk charges 24/7.

**Why other options are incorrect:**
- **C & D:** Sustained Use Discounts are automatic savings, and setting budget alerts is a recommended best practice.

> **Key Exam Takeaway:** Always release unattached static IPs and terminate abandoned lab VMs.
</details>
