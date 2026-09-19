# Phase 03 Practice Drill — Compute Services

> **Total Questions:** 15 (10 MCQs + 5 MSQs)  
> **Topic Tag:** `#phase-03-compute`  
> **Target Score:** 13/15 (86%+)  
> **Covers:** Compute Engine, Machine Types, Persistent Disk vs Local SSD, Spot VMs, Managed Instance Groups (MIGs), Autoscaling & Autohealing, App Engine Standard vs Flexible, Cloud Run, Cloud Functions.

---

## Multiple Choice Questions (MCQs — Single Answer)

### Question 1 [Compute Runtime Selection]
A company operates a stateless REST API with highly volatile and unpredictable traffic, including multi-hour periods of complete inactivity. The company mandates minimizing both operational management overhead and infrastructure cost. Which service should they choose?
- [ ] A. Compute Engine with a fixed cluster of VMs
- [ ] B. App Engine Flexible environment
- [ ] C. Google Cloud Run
- [ ] D. A Managed Instance Group with fixed size

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `C`

**Why it's correct:**
**Google Cloud Run** is a fully managed serverless platform for stateless containers. It automatically scales to zero instances during idle periods (incurring $0 cost) and scales up rapidly upon incoming requests, requiring zero operating system or cluster management.

**Why other options are incorrect:**
- **A & D:** Fixed Compute Engine VMs run 24/7, continuing to incur billing even during zero-traffic hours.
- **B:** App Engine Flexible runs on underlying Compute Engine VMs and maintains a baseline of running instances; it does not scale to zero.

> **Key Exam Takeaway:** Volatile traffic + stateless container + scale-to-zero = Cloud Run.
</details>

---

### Question 2 [Disk Ephemerality]
An engineer attaches a Local SSD to a Compute Engine virtual machine for high-performance caching. What happens to the data stored on the Local SSD when the VM is stopped?
- [ ] A. The data is automatically snapshotted to a multi-region Cloud Storage bucket.
- [ ] B. The data persists on the host machine and is available when the VM starts again.
- [ ] C. The data is permanently lost.
- [ ] D. The data is transferred into a balanced Persistent Disk.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `C`

**Why it's correct:**
**Local SSDs** are physically integrated into the specific host server hosting the virtual machine. When a VM is stopped or terminated, the virtual machine is de-allocated from the physical host, and all data on the Local SSD is permanently erased.

**Why other options are incorrect:**
- **A, B, D:** Google Cloud does not automatically snapshot, preserve, or migrate Local SSD data across VM stops.

> **Key Exam Takeaway:** Local SSD data is strictly EPHEMERAL — it is wiped upon VM stop or termination.
</details>

---

### Question 3 [Discount Models & Preemption]
Which Google Cloud compute pricing model provides the steepest discounts (between 60% and 91% compared to standard on-demand pricing), but carries the operational risk of instance preemption at any time?
- [ ] A. Committed Use Discounts (CUDs)
- [ ] B. Sustained Use Discounts (SUDs)
- [ ] C. Spot VMs
- [ ] D. Custom Machine Type Savings

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `C`

**Why it's correct:**
**Spot VMs** utilize Google Cloud's spare datacenter capacity, offering deep discounts of up to 91%. In exchange for the discount, Google reserves the right to terminate (preempt) the instance with a 30-second warning whenever capacity is needed elsewhere.

**Why other options are incorrect:**
- **A (CUDs):** Require a 1-year or 3-year contract; no preemption risk.
- **B (SUDs):** Automatic discounts applied when instances run >25% of a month; no preemption risk.
- **D:** Custom machine types prevent over-provisioning but do not provide preemptible spare capacity discounts.

> **Key Exam Takeaway:** Up to 91% discount + can be terminated anytime = Spot VMs.
</details>

---

### Question 4 [IP Address Billing Traps]
You reserve a static external IP address in your Google Cloud project for an upcoming product launch. The launch is postponed, and the static IP remains unattached to any running VM instance or load balancer. How is this static IP billed?
- [ ] A. It is completely free because no network traffic is passing through it.
- [ ] B. It incurs an hourly charge because unattached static external IPs are billed to prevent IP address hoarding.
- [ ] C. It is automatically released and deleted after 7 days.
- [ ] D. It automatically changes into an internal private IP address.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**
Google Cloud charges an hourly fee for **reserved static external IP addresses that are unattached** to active resources. This policy discourages reserving limited IPv4 addresses without actively using them.

**Why other options are incorrect:**
- **A:** Classic beginner misconception; unattached IPs are not free.
- **C & D:** Reserved addresses persist until manually deleted and never alter their IP type.

> **Key Exam Takeaway:** Unattached static external IPs incur hourly charges.
</details>

---

### Question 5 [Event-Driven Microservices]
You need to automatically execute a lightweight Python script to extract metadata whenever an image file is uploaded into a Cloud Storage bucket. The execution takes approximately 3 seconds per file. What is the most cost-effective and operationally simple architecture?
- [ ] A. A dedicated e2-micro Compute Engine VM running a continuous polling script.
- [ ] B. An App Engine Flexible application with manual scaling.
- [ ] C. A Google Cloud Function configured with a Cloud Storage object finalization trigger.
- [ ] D. A Managed Instance Group with CPU-based autoscaling.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `C`

**Why it's correct:**
**Google Cloud Functions** with a Cloud Storage trigger is purpose-built for event-driven, single-purpose logic. The function only runs when a file upload event occurs, scales automatically, and bills only for the exact milliseconds of execution.

**Why other options are incorrect:**
- **A, B, D:** Require maintaining servers or containers 24/7, introducing operational overhead and unnecessary idle costs.

> **Key Exam Takeaway:** Small logic triggered by GCS upload = Cloud Function with Storage trigger.
</details>

---

### Question 6 [Managed Instance Group Autohealing]
What is the primary role of an application-level Health Check when configured on a Compute Engine Managed Instance Group (MIG)?
- [ ] A. To monitor project billing spending and generate alerts.
- [ ] B. To probe instance health so Autohealing can automatically destroy and recreate unhealthy instances.
- [ ] C. To ensure all VMs are running the latest Linux kernel patches.
- [ ] D. To encrypt data at rest on attached persistent disks.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**
When attached to a MIG, a **Health Check** periodically polls an HTTP, HTTPS, or TCP endpoint on each VM. If an instance fails consecutive checks, the MIG's **Autohealing** policy automatically recreates the failed instance from the Instance Template.

**Why other options are incorrect:**
- **A, C, D:** Health checks do not manage billing, OS kernel patching, or disk encryption.

> **Key Exam Takeaway:** Health Checks trigger MIG Autohealing to replace crashed VMs.
</details>

---

### Question 7 [Custom Kernel Requirements]
An enterprise needs to migrate a legacy proprietary database requiring a heavily customized, non-standard Linux kernel configuration with specific kernel modules. Which compute service must be selected?
- [ ] A. Google Cloud Functions
- [ ] B. Google Cloud Run
- [ ] C. Google Compute Engine
- [ ] D. App Engine Standard environment

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `C`

**Why it's correct:**
**Compute Engine (IaaS)** provides full root administrative access to the underlying operating system and kernel. Serverless and PaaS options (Cloud Run, Cloud Functions, App Engine Standard) run inside managed sandboxes or containers where custom OS kernels are unsupported.

**Why other options are incorrect:**
- **A, B, D:** Abstract away the OS kernel and forbid custom kernel modifications.

> **Key Exam Takeaway:** Custom OS kernel / legacy system requirements = Compute Engine.
</details>

---

### Question 8 [Cloud Run Concurrency]
In the context of Google Cloud Run, what does the **Concurrency** setting configure?
- [ ] A. The number of separate Cloud Run services deployed within a single project.
- [ ] B. The maximum number of simultaneous HTTP requests a single container instance can process concurrently.
- [ ] C. The maximum number of regions a service can scale across.
- [ ] D. The number of container revisions retained in memory.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**
In Cloud Run, **Concurrency** specifies the maximum number of requests that can be handled simultaneously by a given container instance (default: 80). Higher concurrency reduces the total number of container instances required, lowering costs.

**Why other options are incorrect:**
- **A, C, D:** Inaccurate definitions unrelated to Cloud Run concurrency.

> **Key Exam Takeaway:** Concurrency = simultaneous requests per container instance in Cloud Run.
</details>

---

### Question 9 [Automatic Discounts]
Which Google Cloud discount is applied completely automatically without requiring any contracts, reservations, or upfront commitments, based purely on how long a VM runs in a month?
- [ ] A. Committed Use Discount (CUD)
- [ ] B. Sustained Use Discount (SUD)
- [ ] C. Spot VM Discount
- [ ] D. Free Tier Voucher

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**
**Sustained Use Discounts (SUDs)** are applied automatically by Google Cloud when an eligible Compute Engine VM runs for more than 25% of a billing month. The discount scales up to approximately 30% without any action required by the customer.

**Why other options are incorrect:**
- **A (CUD):** Requires committing to a 1-year or 3-year term.
- **C (Spot):** Requires explicitly launching a Spot/preemptible instance.
- **D:** Free Tier is a fixed monthly usage quota, not an automatic sustained-run discount.

> **Key Exam Takeaway:** Automatic discount without commitments = Sustained Use Discount (SUD).
</details>

---

### Question 10 [MIG Rolling Updates]
An administrator needs to deploy an updated software version across a production Managed Instance Group without introducing application downtime. What mechanism should they use?
- [ ] A. Delete the MIG and recreate it with the new Instance Template.
- [ ] B. Execute a Rolling Update specifying the new Instance Template version with configured surge limits.
- [ ] C. Stop all VMs simultaneously and execute a manual restart script.
- [ ] D. Edit the existing Instance Template in place.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**
A **Rolling Update** gradually replaces older VM instances with new instances created from the updated Instance Template. By setting `maxSurge` (temporary extra VMs allowed) and `maxUnavailable` (maximum VMs offline during update), the deployment completes with zero downtime.

**Why other options are incorrect:**
- **A & C:** Cause severe service outages.
- **D:** Instance Templates are completely immutable and can never be edited in place.

> **Key Exam Takeaway:** Zero-downtime updates in a MIG = Rolling Update with a new Instance Template.
</details>

---

## Multiple Select Questions (MSQs — Multiple Correct Answers)

### Question 11 [Scale-to-Zero Runtimes]
Which of the following Google Cloud compute runtimes natively support automated scale-to-zero when idle? (Select THREE)
- [ ] A. Google Cloud Run
- [ ] B. Google Cloud Functions
- [ ] C. App Engine Standard environment
- [ ] D. App Engine Flexible environment
- [ ] E. Standalone Compute Engine instances

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answers:** `A`, `B`, and `C`

**Why they are correct:**
Cloud Run, Cloud Functions, and App Engine Standard all natively scale down to 0 instances when no requests are being processed, reducing idle compute costs to $0.

**Why other options are incorrect:**
- **D:** App Engine Flexible runs on underlying VMs and maintains at least 1 running instance.
- **E:** Compute Engine VMs remain in running state until explicitly stopped.

> **Key Exam Takeaway:** Scale-to-zero supported: Cloud Run, Cloud Functions, App Engine Standard.
</details>

---

### Question 12 [Spot VM Properties]
Which of the following statements regarding Google Cloud Spot VMs are TRUE? (Select THREE)
- [ ] A. They offer discounts between 60% and 91% compared to standard on-demand pricing.
- [ ] B. Google can preempt and terminate them at any time if capacity is required elsewhere.
- [ ] C. They are backed by the standard Compute Engine 99.99% availability SLA.
- [ ] D. They are ideal for batch rendering, genomics processing, and fault-tolerant container builds.
- [ ] E. They cannot have Persistent Disks attached to them.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answers:** `A`, `B`, and `D`

**Why they are correct:**
- **A:** Spot VMs offer dramatic cost savings.
- **B:** They can be reclaimed by Google at any time with a 30-second notification.
- **D:** Perfectly suited for batch, distributed, or stateless workloads that tolerate interruption.

**Why other options are incorrect:**
- **C:** Spot VMs carry no SLA availability guarantees.
- **E:** Spot VMs support standard, balanced, and SSD Persistent Disks.

> **Key Exam Takeaway:** Spot VMs: Deep discount, no SLA, interruptible, ideal for batch jobs.
</details>

---

### Question 13 [MIG Architecture Components]
Which of the following are fundamental building blocks of a Compute Engine Managed Instance Group (MIG)? (Select THREE)
- [ ] A. Instance Template
- [ ] B. Autoscaler
- [ ] C. Application-Level Health Check
- [ ] D. Cloud Marketplace catalog listing
- [ ] E. Cloud KMS Hardware Security Module

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answers:** `A`, `B`, and `C`

**Why they are correct:**
- **Instance Template:** The immutable configuration blueprint.
- **Autoscaler:** Manages capacity expansion and contraction based on load.
- **Health Check:** Enables autohealing by monitoring application responsiveness.

**Why other options are incorrect:**
- **D & E:** Marketplace and Cloud KMS HSM are separate services unrelated to MIG lifecycle management.

> **Key Exam Takeaway:** MIG building blocks: Instance Template + Autoscaler + Health Check.
</details>

---

### Question 14 [Cloud Functions Trigger Mechanisms]
Which of the following represent valid native event triggers for Google Cloud Functions? (Select THREE)
- [ ] A. Direct HTTP request invocation
- [ ] B. Cloud Pub/Sub topic message publication
- [ ] C. Cloud Storage bucket object mutation (upload/delete)
- [ ] D. Direct Compute Engine CPU throttling event
- [ ] E. VPC Subnet route update

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answers:** `A`, `B`, and `C`

**Why they are correct:**
Cloud Functions integrates natively with HTTP endpoints, Cloud Pub/Sub topics, and Cloud Storage bucket change notifications.

**Why other options are incorrect:**
- **D & E:** Compute Engine CPU events and subnet route changes are not native event triggers for Cloud Functions.

> **Key Exam Takeaway:** Common Cloud Function triggers: HTTP, Pub/Sub, Cloud Storage.
</details>

---

### Question 15 [Persistent Disk vs Local SSD]
Which of the following statements comparing Persistent Disks (PD) and Local SSDs are TRUE? (Select TWO)
- [ ] A. Data stored on a Persistent Disk survives when the VM is stopped and restarted.
- [ ] B. Data stored on a Local SSD persists across VM stop and start cycles.
- [ ] C. Local SSDs generally offer significantly lower latency and higher IOPS than standard Persistent Disks.
- [ ] D. Persistent Disks cannot be dynamically resized while attached to a running instance.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answers:** `A` and `C`

**Why they are correct:**
- **A:** Persistent Disks are network-attached durable storage that survive VM stops.
- **C:** Local SSDs are physically attached NVMe drives providing ultra-high IOPS and sub-millisecond latency.

**Why other options are incorrect:**
- **B:** Local SSD data is erased when a VM is stopped.
- **D:** Persistent Disks can be expanded live without detaching or stopping the VM.

> **Key Exam Takeaway:** Persistent Disk = Durable & resizable. Local SSD = Extreme speed but ephemeral.
</details>
