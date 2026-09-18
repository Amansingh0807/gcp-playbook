# Phase 0 — Cloud Computing Fundamentals ⭐⭐⭐☆☆

> **Prerequisite:** None (Absolute Beginner Friendly)  
> **Estimated Study Time:** 2–3 Hours  
> **Exam Weight:** ⭐⭐⭐☆☆ (Foundational Core Concepts)  
> **Covers:** Module 0.1 (Intro to Cloud Computing), Module 0.2 (Service Models), Module 0.3 (Deployment Models), Module 0.4 (Google Cloud Overview)

---

## 1. Definition

**Cloud Computing** is the delivery of computing resources — servers, storage, databases, networking, and software — over the internet ("the cloud"), instead of owning and running physical hardware yourself. You rent computing power from a provider (like Google, AWS, Microsoft) and pay only for what you use.

> **Hinglish:** Cloud computing matlab apna khud ka computer/server kharidne ke bajaye, kisi bade company (Google/AWS) ke servers ko internet ke through rent par lena — **jitna use, utna paisa**.

---

## 2. Why does this service exist?

### Problem it solves:
- Before cloud, companies had to buy physical servers, set up data centers, hire dedicated teams to maintain them, and guess years in advance how much capacity they would need.
- If traffic spiked (e.g., e-commerce flash sales), the company either had too little capacity (site crashes) or had bought too much (wasted money sitting idle all year).
- Setting up a new server used to take weeks or months (procurement, delivery, rack mounting, OS installation, network cabling).

### Why it was created:
- Large technology companies realized their internal infrastructure (built to withstand peak holiday surges) sat idle for most of the year. Amazon capitalized on this by creating AWS in 2006.
- Google, having engineered world-class planetary-scale infrastructure for Search, YouTube, and Gmail, packaged and opened these capabilities as **Google Cloud Platform (GCP)**.

### What happens if cloud didn't exist:
- Startups would need hundreds of thousands of dollars in upfront capital expenditure just to purchase hardware before writing a single line of code.
- Global scaling, rapid experimentation, and multi-region disaster recovery would be impossible for smaller teams.

> [!IMPORTANT]
> **Remember for ACE Exam:** Cloud computing's fundamental value proposition is converting **CapEx (Capital Expenditure)** into **OpEx (Operational Expenditure)** — you stop buying physical hardware upfront and start paying an ongoing operational bill based strictly on measured consumption.

---

## 3. Architecture / Working

### Step-by-Step Flow of a Cloud Request
```
[User / Developer]
       │
       │ (1) Makes request via Console / gcloud CLI / API / Terraform
       ▼
[Cloud Provider's Control Plane / API Gateway Layer]
       │
       │ (2) Authenticates & Authorizes (IAM permission verification)
       ▼
[Resource Manager & Orchestrator]
       │
       │ (3) Allocates resource from global shared physical pool
       ▼
[Physical Data Center Infrastructure]
       ├── Thousands of physical servers
       ├── Hypervisor / Virtualization layer creates VMs/Containers
       └── Software-Defined Networking (SDN) & Distributed Storage
       ▼
[Your Resource is Provisioned & Active]
       │
       │ (4) Billing meter starts tracking usage per second / minute / GB
       ▼
[You access and run workloads over the Internet]
```

### Lifecycle of a Cloud Resource (e.g., Compute Engine VM)
1. **Request:** You request a VM via Console, `gcloud` CLI, or Terraform.
2. **Provisioning:** Google's control plane identifies physical hardware in the requested zone with sufficient capacity; the hypervisor provisions the virtual slice.
3. **Running:** The VM boots your OS image, attaches disks and network interfaces, and begins processing. Billing meter runs.
4. **Scaling (if configured):** Managed Instance Groups (MIGs) automatically spin up extra VMs when CPU/HTTP load spikes and tear them down when demand recedes.
5. **Termination:** When deleted, virtual resources are unmapped, memory is scrubbed, and hardware is returned to Google's shared pool for other tenants.

> **Hinglish:** Jab tum cloud par resource maangte ho, Google ke paas already hazaron servers ready hote hain (shared pool mein) — woh turant ek chhota sa slice tumhe assign kar dete hain, jaise hotel mein room turant mil jata hai bina nayi building banaye.

---

## 4. Core Components

| Component | What it is | Why it exists | Real-World Analogy |
| :--- | :--- | :--- | :--- |
| **Compute** | Processing power (VMs, containers, functions) | To execute application code and logic | The **engine** of a car |
| **Storage** | Object storage, block disks, file shares | To persist data beyond a single session | A **secure warehouse / locker** |
| **Networking** | Virtual networks, subnets, routers, firewalls | To enable communication between services and internet | **Roads & highways** connecting buildings |
| **IAM** | Identity and Access Management | To control who can perform what actions on which resources | **Security guard** verifying ID badges |
| **Billing / Metering** | Granular usage tracking per second/GB/API call | To calculate and bill exact pay-as-you-go consumption | **Electricity meter** at home |
| **Control Plane / APIs** | Central orchestration and management endpoints | Lets you manage infrastructure via code/UI instead of manual wiring | **Restaurant counter** taking and dispatching orders |
| **Data Centers** | Physical facilities filled with servers, power, cooling | The physical hardware foundation of the cloud | The **factory** producing the compute and storage |

---

## 5. Key Features (The 5 NIST Characteristics)

These 5 characteristics (defined by NIST - National Institute of Standards and Technology) are heavily tested on the ACE exam:

### 5.1 On-Demand Self-Service
You can provision computing capabilities (servers, storage) automatically as needed without requiring human interaction with the service provider.  
*Example:* You spin up a VM in 60 seconds from the Google Cloud Console at 2 AM without calling or emailing Google support.

### 5.2 Broad Network Access
Capabilities are available over the network and accessed through standard mechanisms by heterogeneous client platforms (laptops, mobile phones, tablets).  
*Example:* You can manage your GCP project from a workstation terminal, a web browser, or via the Google Cloud mobile app on iOS/Android.

### 5.3 Resource Pooling
The provider's computing resources are pooled to serve multiple consumers using a multi-tenant model, with different physical and virtual resources dynamically assigned and reassigned according to consumer demand. Customer workloads remain strictly isolated.  
*Example:* Your VM runs on the same physical rack server as another company's VM, but virtualization and hardware security prevent either company from viewing the other's data.

> **Hinglish:** Jaise ek residential society mein alag-alag flats hote hain — sab ek hi building (physical server) mein rehte hain, par har flat (VM) private aur locked hota hai.

### 5.4 Rapid Elasticity
Capabilities can be elastically provisioned and released to scale rapidly outward and inward commensurate with demand. To the consumer, available resources often appear infinite.  
*Example:* An e-commerce platform automatically scales from 2 VMs to 50 VMs during a Diwali flash sale, then scales back down to 2 VMs once traffic normalizes.

### 5.5 Measured Service
Cloud systems automatically control and optimize resource use by leveraging a metering capability at some level of abstraction appropriate to the type of service (e.g., storage, processing, bandwidth, and active user accounts).  
*Example:* You are billed per second of VM execution, per gigabyte-month of storage, and per gigabyte of network egress.

> [!TIP]
> **ACE Exam Mnemonic:** Remember **"On Broad Roads, Rapid Pool Measures"**:  
> **On**-demand self-service | **Broad** network access | **Rapid** elasticity | Resource **Pool**ing | **Measure**d service.

---

## 6. Types / Variants

### 6.1 Evolution: On-Premises to Cloud

```
[ On-Premises ] ──▶ [ Colocation ] ──▶ [ Virtualization ] ──▶ [ Public Cloud ]
You own & manage    Rent rack space,   Multiple VMs on one     Rent compute/storage,
all physical hardware manage your hardware physical machine    zero hardware ownership
```

### 6.2 Cloud Service Models (IaaS, PaaS, SaaS, Serverless)

```
LOW CONTROL, MINIMAL MANAGEMENT                     HIGH CONTROL, HIGH MANAGEMENT
SaaS ──────────────────▶ PaaS ──────────────────▶ IaaS ──────────────────▶ On-Premises
(Google Workspace)      (App Engine/Cloud Run)   (Compute Engine)        (Own Data Center)
```

| Model | Full Form | What Google Manages | What You Manage | GCP Example | Real-World Analogy |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **IaaS** | Infrastructure as a Service | Physical servers, virtualization, physical network, data center facilities | OS, runtime, middleware, application code, data, patches, networking rules | Compute Engine | Renting an empty plot of land; you construct the building |
| **PaaS** | Platform as a Service | Physical infra, virtualization, OS, language runtimes, automatic scaling, patching | Application source code, configuration, data | App Engine, Cloud Run | Renting a fully-built house; you just bring your furniture |
| **SaaS** | Software as a Service | Everything (infrastructure, OS, application code, updates, maintenance) | Your end-user data and user access credentials | Google Workspace, Gmail | Staying in a hotel room; everything is fully managed |
| **Serverless** | Serverless Computing | Server provisioning, OS patching, idle management, zero-scaling | Application logic, event triggers, query definitions | Cloud Functions, Cloud Run, BigQuery | Hailing a taxi / Uber; you only pay while riding, no car maintenance |
| **FaaS** | Function as a Service | Underlying runtime, container lifecycle, event bindings | Discrete modular functions executed on specific triggers | Cloud Functions | A vending machine: insert coin (event trigger) ➔ get snack (function output) |
| **CaaS** | Containers as a Service | Underlying VM provisioning, cluster lifecycle, master plane | Container images, Kubernetes manifests, pod configurations | Google Kubernetes Engine (GKE) | Cargo shipping containers; standard packaging, carrier manages the ship |

> **Hinglish:**  
> - **IaaS:** Khali plot milta hai, tumhe khud cement, deewar aur chhat banani padti hai.  
> - **PaaS:** Ready-made flat milta hai, bas apna saaman le aao aur rehna shuru karo.  
> - **SaaS:** Luxury hotel room — khana, safai sab ready, bas enjoy karo.

### 6.3 Shared Responsibility Model

The Shared Responsibility Model outlines the division of security obligations between Google Cloud and the customer.

```
┌────────────────────────────────┬──────────┬──────────┬──────────┬──────────┐
│ Stack Layer                    │ On-Prem  │   IaaS   │   PaaS   │   SaaS   │
├────────────────────────────────┼──────────┼──────────┼──────────┼──────────┤
│ Data & Content                 │   YOU    │   YOU    │   YOU    │   YOU    │
│ Access Control & IAM           │   YOU    │   YOU    │   YOU    │   YOU    │
│ Application Code               │   YOU    │   YOU    │   YOU    │  Google  │
│ Runtime & Frameworks           │   YOU    │   YOU    │  Google  │  Google  │
│ Operating System & Patching    │   YOU    │   YOU    │  Google  │  Google  │
│ Virtualization Layer           │   YOU    │  Google  │  Google  │  Google  │
│ Physical Servers & Network     │   YOU    │  Google  │  Google  │  Google  │
│ Physical Facility Security     │   YOU    │  Google  │  Google  │  Google  │
└────────────────────────────────┴──────────┴──────────┴──────────┴──────────┘
```

> [!CAUTION]
> **Golden Rule for the ACE Exam:** Regardless of which service model you select (IaaS, PaaS, or SaaS), **Data classification and Access Control (IAM) are ALWAYS your responsibility**. Google never makes authorization decisions for your data.

### 6.4 Customer-Managed vs. Google-Managed Services

| Aspect | Customer-Managed (e.g., MySQL on Compute Engine) | Google-Managed (e.g., Cloud SQL) |
| :--- | :--- | :--- |
| **OS & DB Patching** | Customer must manually apply OS updates and database patches | Handled automatically by Google during maintenance windows |
| **Backups & Snapshots** | Customer scripts and automates backup creation and retention | Built-in point-in-time recovery and automated daily backups |
| **High Availability** | Customer configures read replicas, pacemaker/corosync, failover | One-click High Availability (HA) with automated cross-zone failover |
| **Operational Overhead** | Very high administrative burden | Very low; engineers focus purely on database queries and schemas |

### 6.5 Cloud Deployment Models

| Model | Architecture | Best For | GCP Products Enabling It |
| :--- | :--- | :--- | :--- |
| **Public Cloud** | Multi-tenant infrastructure owned by cloud provider, accessible over the internet | Startups, modern web applications, scalable consumer workloads | Compute Engine, Cloud Storage, BigQuery |
| **Private Cloud** | Dedicated infrastructure used exclusively by a single enterprise (hosted on-prem or isolated) | Highly regulated banking, national defense, strict compliance | Bare Metal Solution, Private Google Access |
| **Hybrid Cloud** | Secure interconnection between on-premises data centers and public cloud environments | Gradual enterprise cloud migrations, burst capacity, data residency | Cloud VPN, Cloud Interconnect, Google Distributed Cloud (Anthos) |
| **Multi-Cloud** | Workloads distributed across two or more public cloud vendors (e.g., GCP + AWS + Azure) | Avoiding vendor lock-in, disaster independence, best-of-breed tool usage | BigQuery Omni, Anthos, Multi-Cloud VPC routing |

---

## 7. Configuration & Important Settings

Foundational concepts applied across all Google Cloud environments:
- **Project Boundary:** The fundamental organizational container. Every single resource must reside inside exactly one project.
- **Billing Association:** Every project must be linked to an active Billing Account to enable paid resources.
- **Regions & Zones:**
  - **Region:** A distinct geographic location (e.g., `asia-south1` in Mumbai, `us-central1` in Iowa) consisting of 3 or more zones.
  - **Zone:** An isolated deployment area within a region (e.g., `asia-south1-a`, `asia-south1-b`). Zones have high-bandwidth, low-latency network connections to other zones in the same region.
- **Global Resources:** Resources accessible everywhere without geographic restriction (e.g., Global VPC networks, Cloud IAM, Cloud DNS, Custom VM Images).

---

## 8. Pricing & Cost Considerations

### How Pricing Works
- **Pay-As-You-Go:** Billed per second for compute, per GB-month for storage, per million requests for serverless.
- **Sustained Use Discounts (SUD):** Automatic discounts applied up to 30% when Compute Engine VMs run for a significant portion of a billing month. Requires zero commitments.
- **Committed Use Discounts (CUD):** Deep discounts (up to 57% for compute, up to 70% for memory) in exchange for a 1-year or 3-year commitment.
- **Free Tier & Free Trial:** Google provides an "Always Free" allocation every month (e.g., 1 e2-micro instance in US regions, 5GB Cloud Storage) and a $300 credit for 90 days for new accounts.

### Cost Optimization Tips
- Terminate or stop idle compute instances outside business hours.
- Utilize **Spot VMs** for batch and fault-tolerant workloads (save up to 60–91%).
- Establish budget threshold alerts at 50%, 90%, and 100%.

> [!WARNING]
> **ACE Billing Trap:** Network **Ingress** (traffic coming into Google Cloud) is almost always **FREE**. Network **Egress** (traffic leaving Google Cloud to the internet or leaving a region) is **CHARGED**. Unattached static external IP addresses also incur hourly charges.

---

## 9. Security Considerations

- **Principle of Least Privilege:** Grant users and applications only the minimum permissions required to perform their tasks.
- **Default Encryption:** Google Cloud automatically encrypts customer data at rest (using Google-managed AES-256 keys) and in transit across external network boundaries.
- **Customer-Managed Encryption Keys (CMEK):** For regulatory compliance, integrate Cloud KMS to control your own key rotation and revocation.
- **Identity as the Security Perimeter:** Traditional network perimeter security is supplemented with identity-based controls (BeyondCorp and context-aware access).

---

## 10. Advantages of Cloud Computing

1. **CapEx to OpEx:** Eliminates millions in upfront capital hardware investment.
2. **Speed and Agility:** Spin up production infrastructure in minutes rather than waiting months for server procurement.
3. **Massive Elasticity:** Scale dynamically to handle unexpected load surges and de-scale to avoid paying for idle capacity.
4. **Planetary Reach:** Deploy services across 40+ regions globally within seconds.
5. **High Reliability & DR:** Built-in multi-zone replication, automated health checks, and cross-region failover.
6. **Managed Maintenance:** Let Google engineers handle hardware maintenance, cooling, fiber routing, and physical security.

---

## 11. Limitations

1. **Internet Dependency:** Management planes and cloud access require continuous network connectivity.
2. **Ongoing Operational Cost Risk:** Unmonitored or unoptimized resource usage can lead to unexpected cloud bill spikes.
3. **Vendor Lock-in:** Deep reliance on proprietary cloud APIs (e.g., BigQuery, Datastore) requires refactoring if migrating to other clouds.
4. **Loss of Low-Level Hardware Control:** PaaS and serverless platforms abstract away the kernel, hardware flags, and low-level hypervisor tuning.
5. **Regulatory & Residency Constraints:** Strict laws in specific jurisdictions may prohibit placing sensitive citizen data in foreign-hosted data centers.

---

## 12. Real-World Use Cases

1. **Spotify:** Leverages Google Cloud (BigQuery, Compute Engine) to process billions of daily music streams and run recommendation models.
2. **Snapchat:** Utilizes Google Cloud's planetary storage and compute infrastructure to support billions of daily multimedia snaps.
3. **E-Commerce Flash Sales:** Rapidly scales frontend web servers from 5 to 200 instances using Managed Instance Groups during festive sales.
4. **Disaster Recovery Replication:** Keeps warm secondary standby databases replicated into a distant secondary region for business continuity.
5. **Financial Services Hybrid Cloud:** Processes sensitive core customer banking ledgers on-premises while running analytical risk models on Google Cloud.
6. **Big Data Analytics:** Ingests petabytes of IoT sensor telemetry into BigQuery for real-time fleet telemetry visualization.
7. **Burst Rendering in Media:** Film animation studios spin up thousands of Spot VMs for 6 hours to render complex 3D scenes at minimal cost.
8. **Automated CI/CD Pipelines:** Containerized code builds spin up dynamically in response to git commits and shut down immediately upon test completion.
9. **Global API Backbones:** Deploys containerized microservices behind a Global External HTTPS Load Balancer with Anycast IP routing.
10. **Healthcare Research:** High-performance computing clusters process genomics sequencing workloads across thousands of preemptible cores.

---

## 13. Hands-on (Console + CLI)

### Console Walkthrough
1. Navigate to [console.cloud.google.com](https://console.cloud.google.com) and log in with your Google account.
2. Accept the terms of service and review the $300 Free Trial status.
3. Locate the **Project Selector** dropdown in the top bar to verify or create a new project.
4. Click the **Navigation Menu (hamburger icon)** to view Google Cloud's organized product taxonomy (Compute, Storage, Networking, IAM).
5. Click the **Cloud Shell** icon (`>_`) in the top-right header to launch an in-browser Linux shell.

### CLI (`gcloud`) Essentials
```bash
# 1. View authenticated active accounts
gcloud auth list

# 2. View current active project configuration
gcloud config get-value project

# 3. Set the active project for all future commands
gcloud config set project [PROJECT_ID]

# 4. List all available projects your account has access to
gcloud projects list

# 5. Set default compute region and zone to avoid repetitive flags
gcloud config set compute/region asia-south1
gcloud config set compute/zone asia-south1-a

# 6. Verify your complete CLI configuration
gcloud config list
```

#### Explanation of Flags:
- `compute/region`: Sets the default region parameter so you don't need to specify `--region` on regional commands.
- `compute/zone`: Sets the default zone parameter so you don't need to specify `--zone` on VM commands.
- `gcloud config set project`: Updates the local active profile configuration file in `~/.config/gcloud/`.

---

## 14. ACE Exam Focus ⭐⭐⭐⭐⭐

### Most Commonly Tested Concepts:
- **NIST Characteristics:** Identifying scenarios matching Elasticity vs. Measured Service vs. Resource Pooling.
- **Shared Responsibility:** Who patches the OS in IaaS (You) vs. PaaS (Google).
- **Regions vs. Zones:** Understanding that a zone failure does not take down other zones in the same region.
- **GCP Networking Differentiator:** GCP VPC networks are **Global** by default (unlike AWS where VPCs are regional).

### Common Traps:
- *Trap:* Thinking Google manages customer data security in SaaS. (Truth: Data authorization is **always** customer-managed).
- *Trap:* Assuming budget alerts automatically stop billing. (Truth: Alerts only notify; automation requires Cloud Functions).
- *Trap:* Confusing Sustained Use Discounts (automatic) with Committed Use Discounts (contractual commitment).

---

## 15. Service Comparisons

### IaaS vs. PaaS vs. SaaS
| Dimension | IaaS | PaaS | SaaS |
| :--- | :--- | :--- | :--- |
| **Management Overhead** | High (Customer handles OS & runtimes) | Low (Google handles platform & scaling) | Zero (Google handles entire software stack) |
| **Architectural Flexibility** | Maximum (Any OS, custom kernels) | Moderate (Constrained to supported runtimes) | Low (Configurable only via software settings) |
| **GCP Flagship Product** | Compute Engine | App Engine, Cloud Run | Google Workspace |
| **Primary Audience** | SysAdmins, Infrastructure Architects | Software Engineers, App Developers | End Business Users |

### Public vs. Private vs. Hybrid Cloud
| Dimension | Public Cloud | Private Cloud | Hybrid Cloud |
| :--- | :--- | :--- | :--- |
| **Hardware Tenancy** | Multi-tenant shared infrastructure | Single-tenant dedicated hardware | Mixed (Public cloud + dedicated DC) |
| **Upfront CapEx** | $0 | High (Hardware purchasing & data center leases) | Moderate to High |
| **Connectivity** | Over the public internet / Cloud CDN | Local private corporate network | Cloud VPN / Dedicated Interconnect |

---

## 16. Best Practices

- Always enable **Multi-Factor Authentication (MFA)** on all administrative accounts.
- Follow the **Principle of Least Privilege** from Day 1; avoid granting the `Owner` or `Editor` role broadly.
- Set default compute regions and zones in your `gcloud` configuration to prevent accidental deployments in expensive regions.
- Set up a **Billing Budget & Threshold Alerts** immediately upon creating any Google Cloud account.
- Prefer managed services (PaaS/Serverless) when application requirements allow, reducing OS patching overhead.

---

## 17. Common Mistakes

- Leaving unused external static IPs reserved (GCP charges an hourly fee for unattached static IPs).
- Forgetting to delete test Compute Engine instances after completing practice labs.
- Confusing Regions and Zones (thinking a Zone can span across multiple Regions).
- Assuming that setting a Budget will automatically cap spending without configuring automation.
- Storing unencrypted secrets or credentials in application source code.

---

## 18. Quick Revision Sheet

- **Cloud Definition:** Renting compute, storage, and networking over the internet; pay-as-you-go.
- **5 NIST Characteristics:**
  1. On-Demand Self-Service (instant provisioning without human intervention)
  2. Broad Network Access (accessible everywhere via web/CLI/mobile)
  3. Resource Pooling (multi-tenant shared hardware with logical isolation)
  4. Rapid Elasticity (dynamic auto-scaling up and down)
  5. Measured Service (granular per-second/GB billing)
- **Service Models:**
  - **IaaS:** Google manages physical hardware; You manage OS, runtime, and app (Compute Engine).
  - **PaaS:** Google manages OS and runtimes; You manage app and data (App Engine, Cloud Run).
  - **SaaS:** Google manages everything; You manage data and users (Google Workspace).
- **Shared Responsibility Golden Rule:** You are **ALWAYS** responsible for your data classification and IAM access control.
- **GCP Global Infrastructure:**
  - Region = Geographic area with 3+ isolated zones.
  - Zone = Single fault-isolated deployment location within a region.
  - Global VPC = GCP networks span the entire planet by default.
- **Cost Traps:** Ingress is free; Egress is billed. Unattached static IPs cost money.

---

## 19. Interview Questions

**Q1: What is the main difference between IaaS and PaaS?**  
*Answer:* In IaaS (Compute Engine), the customer is responsible for the operating system, patching, middleware, and runtime. In PaaS (App Engine), the cloud provider manages the OS and runtime environment, allowing developers to focus solely on deploying code.

**Q2: What are the 5 essential characteristics of cloud computing according to NIST?**  
*Answer:* On-demand self-service, broad network access, resource pooling, rapid elasticity, and measured service.

**Q3: Does Google Cloud manage security of the data stored in a Cloud Storage bucket?**  
*Answer:* Google manages physical security and default at-rest encryption. However, access control (IAM policies, bucket permissions, making files public) is entirely the customer's responsibility under the Shared Responsibility Model.

**Q4: What is the difference between a Region and a Zone in GCP?**  
*Answer:* A Region is an independent geographic area (e.g., `europe-west1`) containing three or more Zones. A Zone is an isolated deployment location (e.g., `europe-west1-b`) within that region engineered with independent power and cooling.

**Q5: What is a Sustained Use Discount (SUD)?**  
*Answer:* An automatic discount provided by Google Cloud for running Compute Engine instances for more than 25% of a billing month. It requires no upfront contractual commitment.

**Q6: What is the difference between Public Cloud and Hybrid Cloud?**  
*Answer:* Public cloud delivers shared multi-tenant resources owned and operated by a third-party provider over the internet. Hybrid cloud connects an organization's on-premises private infrastructure with public cloud services via secure connections like Cloud VPN or Interconnect.

**Q7: Why is GCP's VPC considered unique compared to other major cloud providers?**  
*Answer:* In GCP, VPC networks are global resources. A single VPC network can host subnets across multiple regions worldwide without requiring manual inter-region VPC peering.

**Q8: If a customer wants zero server management and wants to pay only when their code runs in response to HTTP events, what service model should they choose?**  
*Answer:* Serverless / Function-as-a-Service (FaaS) using Google Cloud Functions or Cloud Run.

**Q9: What happens when you reserve a static external IP address in GCP but do not attach it to any running VM?**  
*Answer:* Google charges an hourly fee for reserved but unattached external IP addresses to prevent address reservation waste.

**Q10: What is the principle of least privilege in cloud IAM?**  
*Answer:* The security practice of granting an identity only the absolute minimum set of permissions necessary to perform its intended job function, and nothing more.

---

## 20. ACE Practice Questions

*(Full practice drill available in [practice-questions/by-phase/phase-00.md](../practice-questions/by-phase/phase-00.md))*

### Question 1 (NIST Characteristics)
Which of the following is NOT one of the five essential characteristics of cloud computing as defined by NIST?
- [ ] A. Rapid elasticity
- [ ] B. Measured service
- [x] C. Manual provisioning
- [ ] D. Resource pooling
*Explanation: Manual provisioning contradicts "On-demand self-service," which requires automated provisioning without human intervention.*

### Question 2 (Service Models)
A company wants to deploy application code without managing underlying servers, OS updates, or manual scaling, while retaining control of the application logic. Which service model is most appropriate?
- [ ] A. IaaS
- [x] B. PaaS
- [ ] C. Colocation
- [ ] D. On-Premises
*Explanation: PaaS abstracts infrastructure and OS management while allowing full control over application code.*

### Question 3 (Shared Responsibility)
Under the Google Cloud Shared Responsibility Model, which of the following is ALWAYS the customer's responsibility across all service models (IaaS, PaaS, SaaS)?
- [ ] A. Physical data center access
- [ ] B. Hypervisor security updates
- [x] C. Data classification and Access Control (IAM)
- [ ] D. Server hardware maintenance
*Explanation: Customers always retain responsibility for their own data and who is granted access to it.*

---

## 21. Related Topics

- **Study Before This:** None (This is the starting phase of the playbook).
- **Study After This:** [Phase 01 — GCP Fundamentals, Projects, Billing & Resource Hierarchy](phase-01-gcp-fundamentals.md).
- **Directly Related GCP Services:** Compute Engine, App Engine, Cloud Functions, Cloud Run, Cloud Storage, VPC Networks.
- **Official ACE Exam Guide Mappings:**
  - *Section 1:* Setting up a cloud solution environment (understanding cloud architecture & resource deployment scopes).
