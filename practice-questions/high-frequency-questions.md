# 🏆 Google Cloud ACE — High-Frequency & Recurring Exam Questions

> **Value Note:** These questions represent scenarios, architectural patterns, and tricky edge cases that candidates consistently report encountering on the Google Cloud Associate Cloud Engineer (ACE) exam year after year. Drill these thoroughly before your test.

---

## ⚡ Quick-Hit Trap Master Table

| Concept Area | The Trap / Misconception | The Golden Rule / Reality |
| :--- | :--- | :--- |
| **Billing Budgets** | Thinking setting a budget will stop VMs or cap spending | **Budgets only notify!** Automatic shutdown requires `Pub/Sub` ➔ `Cloud Function` to detach billing. |
| **IAM Inheritance** | Believing you can revoke an Org-level permission at the Project level | **IAM is strictly ADDITIVE (Union).** Permissions granted at parent nodes cannot be removed lower down. |
| **Static External IPs** | Believing you only pay for static IPs when instances are active | **Unattached static external IPs incur hourly charges** to prevent hoarding. Attached IPs to running VMs are free (or lower cost depending on tier). |
| **Network Traffic** | Assuming cross-region data transfer is free | **Ingress is FREE; Egress is CHARGED.** Moving data out of GCP or between regions incurs egress bandwidth costs. |
| **VPC Scope** | Assuming GCP VPCs are regional like AWS | **GCP VPC Networks are GLOBAL.** Subnets are regional, VMs are zonal. |
| **Service Accounts** | Creating and downloading user-managed JSON service account keys for VMs | **Never download service account keys for GCP VMs!** Attach the service account directly to the VM and use metadata server tokens. |
| **App Engine Scaling** | Choosing App Engine Flexible when instant zero-scaling is required | **App Engine Standard scales down to 0 instances** in seconds. App Engine Flexible runs on Docker VMs and cannot scale instantly to zero. |
| **Cloud SQL Failover** | Believing read replicas provide automated high-availability failover | **Read replicas are for read scaling, NOT HA failover.** Enable "High Availability (HA)" configuration for automated regional failover to a standby instance. |
| **Disaster Recovery** | Backing up Compute Engine disks manually with `dd` | Use **Persistent Disk Snapshots**, which are incremental, global, and stored automatically in Cloud Storage multi-regions. |
| **Kubernetes Upgrades** | Upgrading both Master control plane and Node pools simultaneously | **GKE control planes must always be upgraded BEFORE node pools.** Node pools can never run a higher Kubernetes version than the master. |

---

## 🔥 Top Recurring Scenario Questions

### High-Frequency Scenario 1: Automated Spend Capping
**Scenario:**  
Your company has created a Google Cloud project for an intern team. To protect against accidental overspending, leadership mandates that if the project spend reaches $500, all compute workloads must be immediately halted and no further costs should accrue. You need to configure this with minimum operational overhead. What should you do?

- [ ] A. Configure a billing budget of $500 with a 100% threshold alert rule sending an email to the intern lead.
- [ ] B. Set up a billing budget of $500 with Pub/Sub notifications enabled. Create a Cloud Function subscribed to the topic that programmatically disables billing or removes the billing account from the project using the Cloud Billing API.
- [ ] C. Create an Organization Policy constraint that caps Compute Engine vCPU quota when billing hits $500.
- [ ] D. Write a bash cron script on a Compute Engine VM that monitors the Cloud Billing API every 5 minutes and runs `gcloud compute instances stop`.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
Google Cloud budgets do **not** automatically stop spending or enforce hard caps. A budget alert simply sends notifications. To implement automated remediation (capping spend), Google recommends exporting budget programmatic notifications to a Cloud Pub/Sub topic and triggering a Cloud Function that invokes the Cloud Billing API (`projects.updateBillingInfo` with an empty `billingAccountName`) to unlink the billing account.

**Why other options are incorrect:**  
- **A:** Only sends an email; the resources continue running and incurring charges.
- **C:** Organization policies do not monitor billing thresholds.
- **D:** High operational overhead, unreliable, and requires managing a VM that itself incurs billing costs.
</details>

---

### High-Frequency Scenario 2: Principle of Least Privilege for Object Uploads
**Scenario:**  
A developer needs to upload nightly data export files into a specific Cloud Storage bucket named `company-analytics-exports`. The developer should not be allowed to view, modify, or delete existing objects in the bucket, nor access any other buckets in the project. Following Google-recommended security practices, what should you do?

- [ ] A. Grant the developer the `roles/storage.objectCreator` role on the specific bucket `company-analytics-exports`.
- [ ] B. Grant the developer the `roles/storage.objectAdmin` role at the Project level.
- [ ] C. Grant the developer the `roles/editor` role on the project.
- [ ] D. Grant the developer the `roles/storage.admin` role on the bucket.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `A`

**Why it's correct:**  
The **Principle of Least Privilege** requires granting only the minimum necessary permissions at the most specific resource level:
- `roles/storage.objectCreator` allows creating/uploading objects without granting read, delete, or modify permissions.
- Binding this role directly on the **bucket** (rather than the project) prevents access to any other buckets in the project.

**Why other options are incorrect:**  
- **B:** `roles/storage.objectAdmin` includes read, overwrite, and delete permissions, and binding at project level affects all buckets.
- **C:** `roles/editor` is a basic primitive role that grants broad write permissions across almost all GCP services.
- **D:** `roles/storage.admin` grants permission to delete the bucket and alter IAM policies.
</details>

---

### High-Frequency Scenario 3: Secure Service Account Credentials on Compute Engine
**Scenario:**  
You have a Python application running on a Compute Engine virtual machine that needs to read and write records to a Cloud SQL database and Cloud Storage buckets. How should you authenticate the application securely without hardcoding credentials?

- [ ] A. Create a Service Account, generate a JSON private key file, and store it on the VM's boot disk.
- [ ] B. Create a Service Account with the required IAM roles, attach the Service Account to the Compute Engine VM instance, and use the Google Cloud Application Default Credentials (ADC) client library.
- [ ] C. Store the project Owner credentials inside an environment variable on the VM.
- [ ] D. Configure the application to prompt the administrator for OAuth2 credentials whenever the VM boots.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
Google Cloud strongly discourages downloading and managing Service Account JSON key files because they pose severe credential leakage risks. The Google-recommended pattern is to **attach the Service Account directly to the VM instance**. The VM's internal Metadata Server automatically issues temporary short-lived OAuth tokens, which Google Cloud SDKs and client libraries read automatically via **Application Default Credentials (ADC)**.

**Why other options are incorrect:**  
- **A:** Storing long-lived JSON keys on disks creates security vulnerabilities and key rotation management overhead.
- **C:** Using Owner credentials violates least privilege and exposes full project compromise.
- **D:** Unviable for automated production software.
</details>

---

### High-Frequency Scenario 4: Cost-Effective Fault-Tolerant Batch Workloads
**Scenario:**  
An image processing company needs to process 500,000 satellite images every weekend. The processing pipeline is stateless, containerized, can be stopped and resumed at any time, and must complete within 36 hours. What compute strategy provides the maximum cost savings?

- [ ] A. Purchase 3-year Committed Use Discounts (CUDs) for N2 machine instances.
- [ ] B. Deploy a Managed Instance Group using **Spot VMs** with an autoscaling policy based on CPU utilization.
- [ ] C. Run the workload on a single large M2 memory-optimized instance with Sustained Use Discounts.
- [ ] D. Use App Engine Flexible environment with manual scaling set to 50 instances.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
**Spot VMs** offer discounts of **60% to 91%** compared to standard on-demand compute pricing. They are ideal for batch, stateless, and fault-tolerant workloads that can tolerate interruptions.

**Why other options are incorrect:**  
- **A:** CUDs require paying 24/7 for 3 years, which is wasteful for weekend-only batch jobs.
- **C:** Single large instances create a single point of failure and lack elasticity.
- **D:** App Engine Flexible is expensive and not tailored for weekend batch rendering workloads.
</details>

---

### High-Frequency Scenario 5: Migrating Cloud Storage Classes for Archived Data
**Scenario:**  
Your compliance department mandates that financial transaction logs stored in a Cloud Storage bucket must be retained for 7 years. The logs are queried frequently during the first 30 days, accessed fewer than once a year after 90 days, and never accessed after 1 year (only retained for regulatory audits). How can you minimize storage costs automatically?

- [ ] A. Manually move objects between buckets every month using a cron script running `gsutil mv`.
- [ ] B. Configure **Object Lifecycle Management** rules on the bucket:
  - Transition objects to **Nearline Storage** after 30 days.
  - Transition objects to **Coldline Storage** after 90 days.
  - Transition objects to **Archive Storage** after 365 days.
- [ ] C. Store all objects in Standard storage and compress them using gzip.
- [ ] D. Move all objects immediately to Archive storage on Day 1.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
**Object Lifecycle Management** provides automated, rule-based tiering:
- Standard ➔ Nearline (accessed < once/month)
- Nearline ➔ Coldline (accessed < once/quarter)
- Coldline ➔ Archive (accessed < once/year, lowest cost per GB-month).

**Why other options are incorrect:**  
- **A:** Manual scripting adds operational overhead and risks silent failure.
- **C:** Fails to leverage cheaper cold storage tiers.
- **D:** Incurs heavy retrieval fees and minimum storage duration penalties during the first 30 days of active queries.
</details>
