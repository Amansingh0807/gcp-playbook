# Phase 12 Practice Drill - Capstone Scenario Mastery

> **Total Questions:** 15 (10 MCQs + 5 MSQs)  
> **Topic Tag:** `#phase-12-scenario-mastery`  
> **Target Score:** 14/15 (93%+)  
> **Covers:** Cross-cutting ACE exam scenarios, Decision Trees, 15 Battlecard integrations, Multi-service architecture, Troubleshooting, Least-privilege IAM, Cost vs. Resilience tradeoffs.

---

## MCQ Section (Single Correct Answer)

---

### Q1. A media company wants to run a containerized microservice that handles sporadic webhooks from external vendors. Webhooks arrive irregularly - sometimes 500 per minute, sometimes zero for several hours. Which compute service provides the lowest cost and least operational overhead?

- A) Compute Engine VM running Docker on an e2-standard-4 instance
- B) GKE Standard cluster with a 3-node pool
- C) Cloud Run configured to scale to zero
- D) App Engine Flexible environment with a minimum of 1 instance

<details>
<summary>[Yes] Answer</summary>

**C) Cloud Run configured to scale to zero**

**Explanation:** Cloud Run is a fully managed serverless container runtime that automatically scales to handle incoming HTTP request volume and scales down to zero when idle. With true scale-to-zero, the company incurs zero compute costs during hours with no webhook activity. Compute Engine (A) and App Engine Flexible (D) require running instances that incur 24/7 charges.
</details>

---

### Q2. A company needs to store relational customer financial records. The database requires strict ACID transactions, standard SQL support, and must scale horizontally across three continents with 99.999% availability. Which database service is required?

- A) Cloud SQL with High Availability
- B) Cloud Bigtable
- C) Cloud Spanner with a Multi-Region configuration
- D) Firestore in Datastore mode

<details>
<summary>[Yes] Answer</summary>

**C) Cloud Spanner with a Multi-Region configuration**

**Explanation:** Cloud Spanner is Google Cloud's enterprise-grade, globally distributed, horizontally scalable relational database that uniquely delivers multi-region ACID transactions, SQL support, and a five-nines (99.999%) availability SLA. Cloud SQL (A) is limited to a single region and cannot scale writes horizontally.
</details>

---

### Q3. An organization requires that developer virtual machines in a private subnet be able to download software patches from the public internet, but must be completely shielded from direct inbound internet connections. What should be deployed?

- A) Assign an External Static IP address to every VM
- B) Deploy a Cloud NAT gateway on the subnet
- C) Create a firewall rule allowing `0.0.0.0/0` ingress on port 80
- D) Configure VPC Network Peering to an external network

<details>
<summary>[Yes] Answer</summary>

**B) Deploy a Cloud NAT gateway on the subnet**

**Explanation:** Cloud NAT (Network Address Translation) provides managed outbound-only internet connectivity for Compute Engine instances lacking external IP addresses. It blocks unsolicited inbound traffic from reaching the VMs, satisfying the security isolation requirement.
</details>

---

### Q4. What is the Google-recommended approach for granting a Pod running on GKE permission to query a BigQuery dataset without embedding static credentials?

- A) Hardcode a Service Account private JSON key into the container's Dockerfile
- B) Store the service account private key in a Kubernetes Secret
- C) Configure GKE Workload Identity to bind the Kubernetes Service Account to a Google Service Account
- D) Grant the Compute Engine default node service account the `roles/bigquery.admin` role

<details>
<summary>[Yes] Answer</summary>

**C) Configure GKE Workload Identity to bind the Kubernetes Service Account to a Google Service Account**

**Explanation:** GKE Workload Identity is the Google-recommended best practice for authenticating GKE workloads to Google Cloud APIs. It binds a Kubernetes Service Account (KSA) directly to a Google Cloud Service Account (GSA), eliminating the security risk of managing and rotating static JSON private keys.
</details>

---

### Q5. A DevOps engineer wants to ensure that all infrastructure resources across development, staging, and production are defined in code, reviewable via pull requests, and provisionable reproducibly across Google Cloud and on-premises environments. What tool should be used?

- A) Google Cloud Deployment Manager
- B) HashiCorp Terraform
- C) Google Cloud Shell interactive scripts
- D) Cloud Build substitution macros

<details>
<summary>[Yes] Answer</summary>

**B) HashiCorp Terraform**

**Explanation:** HashiCorp Terraform is an open-source, multi-cloud declarative Infrastructure as Code (IaC) tool. It codifies infrastructure into version-controlled files (`.tf`) that can be peer-reviewed in Git pull requests and applied across Google Cloud and on-premises systems.
</details>

---

### Q6. A billing alert is triggered at 100% of a $5,000 monthly budget. What happens immediately to active compute instances in the project?

- A) All Compute Engine VMs are paused
- B) The Cloud Billing account is permanently suspended
- C) Resources continue running and accruing charges uninterrupted
- D) GKE clusters scale down to zero worker nodes

<details>
<summary>[Yes] Answer</summary>

**C) Resources continue running and accruing charges uninterrupted**

**Explanation:** Google Cloud Budgets and alerts are strictly informational. When a budget threshold is reached, automated email notifications are dispatched, but running workloads are never stopped or modified automatically.
</details>

---

### Q7. A legacy accounting application running on Linux requires a POSIX-compliant shared file system that can be mounted simultaneously by 15 Compute Engine virtual machines. Which service should be used?

- A) Regional Persistent Disk with ReadWriteMany mode
- B) Cloud Storage FUSE mount
- C) Filestore
- D) Local SSD array

<details>
<summary>[Yes] Answer</summary>

**C) Filestore**

**Explanation:** Filestore provides a fully managed NFSv3 network-attached storage (NAS) system compatible with POSIX file system standards, allowing multiple Linux Compute Engine instances to mount and perform concurrent read/write operations simultaneously.
</details>

---

### Q8. A security team mandates that administrative changes to IAM policies and project metadata must be audited and stored for compliance. Who has the authority to disable Admin Activity audit logs in Google Cloud?

- A) The Organization Administrator
- B) The Billing Account Administrator
- C) Google Cloud Support upon formal written request
- D) Nobody; Admin Activity audit logs are permanently enabled and cannot be disabled

<details>
<summary>[Yes] Answer</summary>

**D) Nobody; Admin Activity audit logs are permanently enabled and cannot be disabled**

**Explanation:** Admin Activity audit logs are permanently enabled across all Google Cloud services, free of charge, stored in the immutable `_Required` bucket for 400 days, and cannot be disabled or modified by any user or administrator.
</details>

---

### Q9. An e-commerce platform needs to distribute incoming HTTPS traffic globally to compute backends located in both North America and Europe, routing users to the nearest healthy region. Which load balancer is required?

- A) Regional External Application Load Balancer
- B) Global External Application Load Balancer
- C) Internal Passthrough Network Load Balancer
- D) External Passthrough Network Load Balancer

<details>
<summary>[Yes] Answer</summary>

**B) Global External Application Load Balancer**

**Explanation:** Global External Application Load Balancers operate at Layer 7 using a single global Anycast VIP. Incoming HTTP/HTTPS client requests are terminated at the nearest Google edge Point of Presence (PoP) and routed over Google's private network backbone to the closest healthy backend region.
</details>

---

### Q10. A development team wants to deploy code to production automatically whenever changes pass testing, but enterprise regulatory compliance mandates that a human manager must click an approval button before live deployment occurs. Which model does this represent?

- A) Continuous Integration (CI)
- B) Continuous Delivery (CD)
- C) Continuous Deployment
- D) Manual Infrastructure as Code

<details>
<summary>[Yes] Answer</summary>

**B) Continuous Delivery (CD)**

**Explanation:** Continuous Delivery automates all build, test, and staging steps, but deliberately enforces an intentional manual approval gate prior to promoting changes into live production. Continuous Deployment eliminates that manual gate.
</details>

---

## MSQ Section (Multiple Select Questions)

---

### Q11. Which of the following statements regarding Google Cloud VPC Networks are TRUE? (Select TWO)

- A) VPC networks in Google Cloud are global resources that span all geographical regions.
- B) Subnets are regional resources that reside entirely within a single specified region.
- C) Two VPC networks can communicate by default without firewall rules or peering.
- D) Auto Mode VPC networks create subnets in only three primary regions.
- E) VPC firewall rules are applied at the project level and cannot filter traffic by network tags.

<details>
<summary>[Yes] Answer</summary>

**A) VPC networks in Google Cloud are global resources that span all geographical regions, B) Subnets are regional resources that reside entirely within a single specified region.**

**Explanation:** VPC networks are global in scope, allowing subnets in different regions across the world to communicate securely over Google's internal network backbone. Subnets are regional resources bound to a specific region.
</details>

---

### Q12. Which of the following represent effective cost optimization practices for Google Cloud Compute Engine? (Select THREE)

- A) Utilizing Spot VMs for fault-tolerant, batch rendering workloads.
- B) Purchasing Committed Use Discounts (CUDs) for predictable 24/7 database instances.
- C) Configuring automated snapshot schedules with retention expiration rules to clean old backups.
- D) Provisioning all development environments on multi-region Sole-Tenant nodes.
- E) Reserving static external IP addresses and leaving them unattached to virtual machines.

<details>
<summary>[Yes] Answer</summary>

**A) Utilizing Spot VMs for fault-tolerant, batch rendering workloads, B) Purchasing Committed Use Discounts (CUDs) for predictable 24/7 database instances, C) Configuring automated snapshot schedules with retention expiration rules to clean old backups.**

**Explanation:** Spot VMs offer deep discounts for batch processing, CUDs provide significant savings for predictable steady workloads, and snapshot expiration policies prevent storage bloat. Unattached external IPs incur reservation fees (E is false).
</details>

---

### Q13. Which of the following services provide automated scale-to-zero capabilities when idle? (Select TWO)

- A) Cloud Run
- B) App Engine Standard
- C) Compute Engine standalone VMs
- D) Cloud SQL
- E) App Engine Flexible

<details>
<summary>[Yes] Answer</summary>

**A) Cloud Run, B) App Engine Standard.**

**Explanation:** Cloud Run and App Engine Standard support true scale-to-zero, spinning down completely when no requests are being processed. App Engine Flexible maintains a minimum of 1 running VM, and Compute Engine and Cloud SQL incur ongoing charges while provisioned.
</details>

---

### Q14. Which mechanisms can be used to interconnect an on-premises network with a Google Cloud VPC without sending unencrypted traffic over the public internet? (Select TWO)

- A) Cloud Dedicated Interconnect
- B) High Availability (HA) Cloud VPN
- C) Public External IP forwarding
- D) Standard Internet Gateway routing
- E) Cloud Storage Transfer Service

<details>
<summary>[Yes] Answer</summary>

**A) Cloud Dedicated Interconnect, B) High Availability (HA) Cloud VPN.**

**Explanation:** Dedicated Interconnect provides private physical circuit connections that do not traverse the public internet. HA Cloud VPN encapsulates and encrypts traffic inside secure IPsec tunnels across public networks.
</details>

---

### Q15. Which statements accurately characterize Google Cloud IAM best practices? (Select TWO)

- A) Predefined roles should always be prioritized over Primitive roles (Owner, Editor, Viewer).
- B) Workloads and applications should authenticate using dedicated Service Accounts rather than human user credentials.
- C) Developers should all be granted the primitive Owner role in production to accelerate deployments.
- D) Service Account private keys should be committed into Git repositories for easy sharing.
- E) Custom roles should be created for every single user in an organization.

<details>
<summary>[Yes] Answer</summary>

**A) Predefined roles should always be prioritized over Primitive roles (Owner, Editor, Viewer), B) Workloads and applications should authenticate using dedicated Service Accounts rather than human user credentials.**

**Explanation:** Predefined roles adhere to the principle of least privilege by scoping permissions specifically to distinct services. Automated applications should always use dedicated Service Accounts.
</details>

---

## Score Tracker

| Score | Verdict |
| :---: | :--- |
| **15/15** | **ACE Grandmaster:** Fully prepared to pass the Google Cloud Associate Cloud Engineer exam with distinction! |
| **13–14** | [Yes] **Exam-Ready:** Outstanding score; review any missed battlecard nuances before taking the test. |
| **11–12** | Note: **Near Passing:** Review the 5 Decision Trees and 10 Exam Gotchas in Phase 12. |
| **< 11** | **Targeted Review:** Revisit the specific domain chapters (Compute, Networking, IAM, or Storage) where mistakes occurred. |
