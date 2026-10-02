# ⏱️ ACE Full-Length Mock Exam 01

> **Exam Timing:** 2 Hours (120 Minutes)  
> **Total Questions:** 50 Questions (Mixed Domains)  
> **Passing Score:** 40/50 (80%+)  
> **Instructions:** Simulates the real Google Cloud Associate Cloud Engineer exam environment. Attempt in one continuous sitting without consulting external documentation.

---

## 📊 Domain Coverage Matrix
- **Section 1:** Setting up a cloud solution environment (~20%)
- **Section 2:** Planning and configuring a cloud solution (~16%)
- **Section 3:** Deploying and implementing a cloud solution (~25%)
- **Section 4:** Ensuring successful operation of a cloud solution (~20%)
- **Section 5:** Configuring access and security (~19%)

---

## 📝 Exam Questions (1 to 50)

*(Questions from your 100+ collection will be slotted here as you study through the phases)*

### Question 1 [Identity & Access Management]
A team of 4 data analysts needs read-only access to query datasets in Google BigQuery. They should not be able to delete tables, edit schemas, or view any billing information. What is the Google-recommended method to configure access?
- [ ] A. Assign each analyst the primitive `roles/viewer` role on the project.
- [ ] B. Create a Google Group `analysts@company.com`, add all 4 members to the group, and grant `roles/bigquery.dataViewer` and `roles/bigquery.jobUser` to the group.
- [ ] C. Create a custom Service Account, generate a JSON key, and share the key with the analysts.
- [ ] D. Grant each analyst `roles/bigquery.admin` at the dataset level.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
Google best practice is to assign IAM roles to **Google Groups** rather than individual user accounts. To run queries and view data, BigQuery requires two roles:
1. `roles/bigquery.dataViewer`: to view and read table contents.
2. `roles/bigquery.jobUser`: to execute query jobs and run SQL.

**Why other options are incorrect:**  
- **A:** Primitive `roles/viewer` grants broad read access across almost every GCP service in the project, violating least privilege.
- **C:** Service accounts and JSON keys are for applications, not human interactive access.
- **D:** `roles/bigquery.admin` grants full management permissions including table deletion.
</details>

---

### Question 2 [Compute Engine Management]
You need to deploy a fleet of stateless web servers behind an HTTP(S) Load Balancer. The servers must automatically scale based on CPU load and automatically replace any instance that fails an application-level health check. What compute configuration should you deploy?
- [ ] A. An Unmanaged Instance Group with a custom startup script.
- [ ] B. A regional Managed Instance Group (MIG) configured with an instance template, autoscaling, and autohealing health checks.
- [ ] C. Individual standalone Compute Engine instances managed by a bash cron script.
- [ ] D. A zonal Managed Instance Group without health checks.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
A **regional Managed Instance Group (MIG)** provides:
- High availability across multiple zones in a region.
- Automated scaling based on metrics like CPU utilization.
- **Autohealing**, which uses application-level health checks to automatically recreate unhealthy VM instances.

**Why other options are incorrect:**  
- **A:** Unmanaged instance groups do not support autoscaling, autohealing, or rolling updates.
- **C:** High operational burden and unreliable.
- **D:** Zonal MIGs fail if a single zone experiences an outage, and lack autohealing without health checks.
</details>

---

### Question 3 [VPC Networking]
You are creating a custom VPC network for a production workload. You create a subnet in `us-central1` with CIDR `10.10.0.0/24`. Later, as traffic grows, you need to add more IP addresses to this subnet without causing downtime or recreation. What should you do?
- [ ] A. Delete the subnet and recreate it with a larger CIDR block.
- [ ] B. Run `gcloud compute networks subnets expand-ip-range` to expand the subnet prefix (e.g., to `/20`).
- [ ] C. Create a second VPC and peer the two VPCs together.
- [ ] D. Subnet CIDR ranges can never be expanded in Google Cloud.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
Google Cloud allows you to **expand** the IP CIDR range of an existing subnet non-disruptively using the Console or `gcloud compute networks subnets expand-ip-range`. The new CIDR block must be larger (smaller prefix number, e.g., `/24` to `/20`) and cannot overlap with other subnets.

**Why other options are incorrect:**  
- **A:** Deleting a subnet deletes all attached VMs and causes total service outage.
- **C:** Unnecessary overhead when native CIDR expansion is supported.
- **D:** False; subnets can be expanded (though they cannot be shrunk).
</details>

### Question 4 [Service Account User Role]
A junior developer is tasked with deploying a new containerized microservice to Google Cloud Run. The Cloud Run service needs to execute under a custom user-managed Service Account named `payments-processor-sa`. When the developer attempts to deploy the service, the deployment fails with a permission denied error related to the Service Account. What IAM role must be granted to the developer?
- [ ] A. `roles/iam.serviceAccountAdmin`
- [ ] B. `roles/iam.serviceAccountUser` on `payments-processor-sa`
- [ ] C. `roles/owner` on the project
- [ ] D. `roles/iam.serviceAccountKeyAdmin`

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
To deploy a resource (like a Cloud Run service or Compute Engine VM) that executes under a specific Service Account, the deploying user must possess the **`roles/iam.serviceAccountUser`** role on that specific Service Account. This gives the user permission to "act as" or attach the identity without granting permissions to manage or delete the Service Account itself.

**Why other options are incorrect:**  
- **A:** `roles/iam.serviceAccountAdmin` allows creating and deleting Service Accounts, but is not the specific role required to attach an SA to a compute resource.
- **C:** Violates the principle of least privilege.
- **D:** Manages cryptographic JSON keys, which is unnecessary and poor practice for serverless deployments.
</details>

---

### Question 5 [Cost-Effective Batch Compute]
An analytical workload processes 100,000 satellite images every weekend. The jobs are stateless, can be interrupted and resumed from checkpoint storage, and must be completed with the lowest possible infrastructure spend. What compute deployment architecture should you choose?
- [ ] A. Single large M2 memory-optimized Compute Engine instance with On-Demand pricing.
- [ ] B. Regional Managed Instance Group (MIG) using Spot VMs with an autoscaling policy based on CPU utilization.
- [ ] C. A 3-year Committed Use Discount (CUD) on N2 standard instances.
- [ ] D. App Engine Flexible environment configured with manual scaling.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
**Spot VMs** provide discounts up to 91% compared to standard rates and are the Google-recommended compute choice for batch, stateless, and fault-tolerant workloads that can tolerate preemption. Wrapping them in a **Managed Instance Group (MIG)** ensures that if instances are preempted, the group automatically provisions replacements when capacity is available.

**Why other options are incorrect:**  
- **A:** Single instance creates a single point of failure and does not scale dynamically.
- **C:** CUDs require paying 24/7 for 3 years, which is wasteful for weekend-only processing.
- **D:** App Engine Flexible is expensive and unsuited for large-scale distributed batch compute.
</details>

---

*(Questions 6–50 reserved for remaining phases and incoming question sets)*

---

## 📈 Score Tracking Sheet

| Date | Score (/50) | Percentage | Target Met? (≥80%) | Weak Domains Identified |
| :--- | :---: | :---: | :---: | :--- |
| YYYY-MM-DD | -- | --% | [ ] Yes [ ] No | |
