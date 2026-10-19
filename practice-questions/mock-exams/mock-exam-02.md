# ACE Full-Length Mock Exam 02

> **Exam Timing:** 2 Hours (120 Minutes)  
> **Total Questions:** 50 Questions (Mixed Domains)  
> **Passing Score:** 40/50 (80%+)  
> **Instructions:** Full-length test designed to measure readiness across all 5 official ACE exam domains. Attempt in one continuous sitting without consulting external documentation.

---

## Domain Coverage Matrix
- **Section 1:** Setting up a cloud solution environment (~20%)
- **Section 2:** Planning and configuring a cloud solution (~16%)
- **Section 3:** Deploying and implementing a cloud solution (~25%)
- **Section 4:** Ensuring successful operation of a cloud solution (~20%)
- **Section 5:** Configuring access and security (~19%)

---

## Exam Questions (1 to 50)

### Question 1 [Resource Hierarchy & Organization Policies]
You are an organization administrator. You need to enforce a policy that prevents all developers in your organization from assigning external public IP addresses to Compute Engine instances. How should you apply this policy with minimal administrative overhead?
- [ ] A. Configure a VPC firewall egress rule blocking all public IP addresses.
- [ ] B. Enforce the `compute.vmExternalIpAccess` Organization Policy constraint at the Organization root node.
- [ ] C. Create a custom IAM role denying `compute.addresses.create` and apply it to all developers.
- [ ] D. Run a nightly Cloud Function that scans and deletes external IPs on all instances.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
Enforcing the **`compute.vmExternalIpAccess`** Organization Policy constraint at the Organization root node is inherited by all child folders and projects, automatically preventing public IP assignments across the entire enterprise with zero ongoing manual maintenance.

**Why other options are incorrect:**  
- **A:** Firewall rules govern network traffic flow, not IP allocation during instance creation.
- **C:** IAM controls permissions, but does not enforce architectural resource guardrails as effectively as Organization Policies.
- **D:** High operational overhead and leaves a window of exposure before deletion.
</details>

---

### Question 2 [Cloud Storage Encryption & Key Management]
Your company mandates that all data stored in Cloud Storage must be encrypted using customer-managed keys (CMEK) that your security team can rotate and revoke at will. Which service should you integrate with Cloud Storage?
- [ ] A. Google default encryption keys
- [ ] B. Cloud Key Management Service (Cloud KMS)
- [ ] C. Customer-Supplied Encryption Keys (CSEK)
- [ ] D. Secret Manager

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
**Cloud KMS** allows organizations to generate, rotate, and manage Customer-Managed Encryption Keys (CMEK) that natively integrate with Cloud Storage and other Google Cloud services.

**Why other options are incorrect:**  
- **A:** Google-managed keys cannot be directly rotated or revoked by customer security teams.
- **C:** CSEK requires providing the raw key with every individual API call, which is difficult to manage at scale.
- **D:** Secret Manager is for storing passwords and API tokens, not cryptographic block encryption keys for storage buckets.
</details>

---

### Question 3 [Compute Engine Right-Sizing & Recommendations]
You manage 100 Compute Engine instances in a project. Finance reports that compute costs are higher than anticipated. You want to identify underutilized instances and resize them following Google-recommended practices. What should you use?
- [ ] A. Cloud Monitoring Metrics Explorer manually
- [ ] B. Compute Engine Recommender (VM rightsizing recommendations)
- [ ] C. Export logs to Cloud Storage
- [ ] D. Run a Python script via SSH on each VM

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
The **Compute Engine Recommender** automatically analyzes 8 days of CPU and memory utilization telemetry to generate actionable VM right-sizing recommendations that can be applied directly via the console or CLI.
</details>

---

### Question 4 [GKE Release Channels & Stability]
You are provisioning a production GKE cluster for an enterprise core banking application. You want automated node updates, but you must minimize the frequency of changes and only adopt Kubernetes versions that have undergone extensive enterprise validation. Which GKE Release Channel should you select?
- [ ] A. Rapid Channel
- [ ] B. Regular Channel
- [ ] C. Stable Channel
- [ ] D. Static / No channel

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `C`

**Why it's correct:**  
The **Stable Channel** receives updates least frequently and provides the highest level of version maturity and enterprise stabilization for mission-critical production workloads.
</details>

---

### Question 5 [VPC Network Peering Constraints]
Company A and Company B need to connect their Google Cloud VPC networks using VPC Network Peering. Both companies created their VPC networks using default Auto Mode settings. Why will the peering configuration fail?
- [ ] A. VPC Network Peering does not support Auto Mode networks.
- [ ] B. The primary subnet IP CIDR ranges (`10.128.0.0/9`) overlap identically in every region across both Auto Mode VPCs.
- [ ] C. Peering requires a Cloud VPN gateway.
- [ ] D. Peering cannot connect VPCs in different projects.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
VPC Network Peering strictly requires that peered networks have **non-overlapping IP CIDR blocks**. Auto Mode VPCs automatically create subnets with identical CIDR ranges (`10.128.0.0/9`) in all regions, causing an immediate CIDR collision error during peering setup.
</details>

---

### Question 6 [Artifact Registry Repository Cleanup Policies]
Your CI/CD pipeline builds dozens of container images daily, causing Artifact Registry storage costs to escalate. You need to automatically delete untagged container images older than 14 days without deleting release-tagged images. What should you do?
- [ ] A. Write a nightly cron job using `gcloud artifacts docker images delete`.
- [ ] B. Configure an automated Cleanup Policy on the Artifact Registry repository targeting untagged images with an age greater than 14 days.
- [ ] C. Enable Cloud Storage Object Versioning on the backing bucket.
- [ ] D. Grant developers read-only access to Artifact Registry.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
Artifact Registry supports native **Cleanup Policies** that evaluate criteria (such as untagged status and age) to automatically purge unneeded images without external scripts.
</details>

---

### Question 7 [Cloud Run Concurrency & Performance]
You deploy a Node.js web API on Cloud Run. Under high traffic, Cloud Run spins up multiple container instances, but latency spikes because each container is configured to handle only 1 concurrent request. You want to optimize resource utilization and allow each container to handle up to 80 concurrent requests. What setting should you adjust?
- [ ] A. Increase CPU allocation to 8 vCPUs.
- [ ] B. Adjust the `concurrency` setting on the Cloud Run service revision to 80.
- [ ] C. Deploy the container as a Cloud Run job.
- [ ] D. Switch to App Engine Flexible.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
Cloud Run supports configurable **concurrency** (up to 1,000 requests per container instance). Raising concurrency allows a single container to process multiple simultaneous requests, reducing cold starts and infrastructure costs.
</details>

---

### Question 8 [BigQuery Authorized Datasets & Security]
You want to grant a marketing analyst group access to view query results from a specific analytical model without granting them read permissions to the sensitive raw underlying tables. What should you create?
- [ ] A. A custom IAM role with `bigquery.tables.getData`.
- [ ] B. A BigQuery Authorized View or Authorized Dataset.
- [ ] C. Export the data to Cloud Storage and share via signed URLs.
- [ ] D. A Cloud Function API gateway.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
An **Authorized View** (or Authorized Dataset) allows designated users to query view results without possessing read access to the underlying referenced tables.
</details>

---

### Question 9 [Cloud Spanner vs Cloud SQL Architecture]
A global financial enterprise is migrating its core ledger database to Google Cloud. The database requires standard SQL, strict serializable ACID transactions across North America, Europe, and Asia, and write capacity scaling past 100 TB. Which database should you choose?
- [ ] A. Cloud SQL for PostgreSQL with Cross-Region Read Replicas
- [ ] B. Cloud Spanner with a Multi-Region instance configuration
- [ ] C. Cloud Bigtable
- [ ] D. Firestore in Native Mode

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
**Cloud Spanner** is Google Cloud's globally distributed relational database offering horizontal write scaling, multi-region ACID consistency, and a 99.999% SLA. Cloud SQL does not support multi-region write scalability.
</details>

---

### Question 10 [Terraform Remote Backend & State Locking]
A team of 10 DevOps engineers collaborates on managing Google Cloud infrastructure using Terraform. How should the team configure the Terraform backend to prevent concurrent modification race conditions and ensure state security?
- [ ] A. Commit `terraform.tfstate` directly to a shared Git repository.
- [ ] B. Store the state file in a secured Google Cloud Storage bucket with Object Versioning and native state locking (`backend "gcs"`).
- [ ] C. Store the state file on a shared Filestore NFS mount.
- [ ] D. Run Terraform solely in local workstation memory.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
The Google Cloud Storage (`gcs`) backend for Terraform provides centralized remote state storage, automated state locking during execution to prevent collisions, and object versioning for disaster recovery.
</details>

---

### Question 11 [Cloud Audit Logs Retention & Immutability]
An auditor requires proof of all administrative actions (who created, modified, or deleted resources) in a project over the past 300 days. Where are these logs stored by default, and can they be deleted?
- [ ] A. In the `_Default` log bucket for 30 days; they can be deleted by the Project Owner.
- [ ] B. In the `_Required` log bucket for 400 days; they cannot be deleted or modified by anyone.
- [ ] C. In BigQuery datasets permanently.
- [ ] D. In Cloud Storage Coldline buckets.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
Admin Activity audit logs are stored in the immutable **`_Required`** bucket for **400 days** free of charge and cannot be modified or deleted by any user or administrator.
</details>

---

### Question 12 [Cloud NAT Gateway Sizing & IP Allocation]
You configure Cloud NAT for a private subnet. As traffic grows to millions of concurrent outbound connections, instances begin experiencing TCP timeout drops to public destinations. What is the most likely cause, and how do you resolve it?
- [ ] A. Firewall egress rules are blocking traffic; disable firewall.
- [ ] B. NAT port exhaustion; allocate additional static external IP addresses to the Cloud NAT gateway or enable dynamic port allocation.
- [ ] C. Subnet CIDR is too small.
- [ ] D. Cloud NAT does not support more than 1,000 connections.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
Each external IP assigned to a Cloud NAT gateway provides approximately 64,512 TCP/UDP source ports. When connection volumes exceed available ports, **NAT port exhaustion** occurs. Allocating additional static IPs or enabling dynamic port allocation resolves the issue.
</details>

---

### Question 13 [GKE Pod Security & RunAsNonRoot]
Security guidelines require that containerized workloads deployed to GKE must never execute with root user privileges inside the container. Which Kubernetes manifest configuration enforces this at the pod level?
- [ ] A. Set `privileged: true` in the container spec.
- [ ] B. Configure `securityContext.runAsNonRoot: true` in the Pod or Container manifest.
- [ ] C. Set `hostNetwork: true`.
- [ ] D. Assign `roles/viewer` to the Kubernetes service account.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
Setting **`securityContext.runAsNonRoot: true`** instructs the container runtime to validate that the image does not run as UID 0 (root) and rejects scheduling if root execution is attempted.
</details>

---

### Question 14 [Cloud SQL Storage Auto-Increase]
A production MySQL database on Cloud SQL experiences steady transactional growth. To avoid storage exhaustion outages, you want the database disk to automatically increase in capacity as data grows. What should you configure?
- [ ] A. A Cloud Function monitoring storage metrics via Cloud Monitoring.
- [ ] B. Enable the "Enable storage auto-increase" setting on the Cloud SQL instance configuration.
- [ ] C. Attach an unformatted Persistent Disk to the instance.
- [ ] D. Cloud SQL does not support automatic disk resizing.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
Cloud SQL features native **storage auto-increase**, which automatically adds storage capacity when free disk space falls below a critical threshold (typically 10%), preventing database crashes due to full disks.
</details>

---

### Question 15 [Multi-Region Load Balancing with Cloud CDN]
A global news website hosts media assets on Compute Engine instances in North America and Asia. You want to cache static media assets at Google edge locations worldwide to minimize latency and offload backend traffic. What should you deploy?
- [ ] A. An Internal Application Load Balancer with Cloud Armor.
- [ ] B. A Global External Application Load Balancer with Cloud CDN enabled on the backend service.
- [ ] C. Cloud Storage Archive buckets in every region.
- [ ] D. External Network Load Balancer with regional static IPs.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
**Cloud CDN (Content Delivery Network)** works natively with Global External Application Load Balancers to cache static web content at Google edge Points of Presence across the globe, dramatically reducing origin server load and user latency.
</details>

---

### Question 16 [Billing Account User vs Costs Manager]
A project manager needs the ability to link newly created projects to an approved corporate billing account, but must not be allowed to inspect enterprise discount contracts or view invoices. What role should be assigned?
- [ ] A. `roles/billing.admin`
- [ ] B. `roles/billing.user`
- [ ] C. `roles/billing.costsManager`
- [ ] D. `roles/billing.viewer`

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
**`roles/billing.user`** grants permissions strictly to associate projects with a billing account without granting permissions to view spend reports, invoices, or payment methods.
</details>

---

### Question 17 [Persistent Disk Asynchronous Replication]
A mission-critical financial application on Compute Engine requires regional disaster recovery protection between `us-central1` and `us-east4` with an RPO of less than 15 minutes. What storage feature should be implemented?
- [ ] A. Zonal Persistent Disk with daily snapshots
- [ ] B. Regional Persistent Disk across two zones in `us-central1`
- [ ] C. Persistent Disk Asynchronous Replication (PD Async Replication) between the two regions
- [ ] D. Cloud Storage FUSE mounting

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `C`

**Why it's correct:**  
**Persistent Disk Asynchronous Replication** provides managed cross-region block storage replication with sub-minute RPO for disaster recovery across distant Google Cloud regions.
</details>

---

### Question 18 [Cloud Run Custom Domains & SSL]
You deployed a web application to Cloud Run and want to map it to a custom corporate domain (`app.company.com`) with an automatically renewed SSL/TLS certificate. What should you do?
- [ ] A. Provision a dedicated VM running NGINX with Let's Encrypt certbot.
- [ ] B. Add a custom domain mapping in Cloud Run, update DNS CNAME/A records, and allow Cloud Run to provision a Google-managed SSL certificate automatically.
- [ ] C. Buy an SSL certificate and upload private keys to Secret Manager.
- [ ] D. Cloud Run does not support custom domain mapping.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
Cloud Run provides built-in custom domain mapping and automatically provisions and renews managed SSL/TLS certificates through Google Certificate Manager at zero extra cost.
</details>

---

### Question 19 [Compute Engine Live Migration]
Google Cloud periodically patches host hardware and updates physical hypervisors in its data centers. How does Compute Engine maintain VM availability during these host events without terminating running user instances?
- [ ] A. It reboots the VM into single-user mode.
- [ ] B. It performs Live Migration, transparently moving the running VM instance to another host in the same zone with no user downtime.
- [ ] C. It converts the instance to a Spot VM.
- [ ] D. It restarts the VM from a snapshot in another region.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
Compute Engine uses **Live Migration** to move running virtual machines to another physical host server within the same availability zone during infrastructure maintenance without interrupting workload execution or dropping network state.
</details>

---

### Question 20 [IAM Policy Conditions for Time-Bound Access]
A contractor requires temporary read access to a production Cloud Storage bucket that must expire automatically on Friday at 5:00 PM UTC. How should you configure access?
- [ ] A. Grant `roles/storage.objectViewer` and set a calendar reminder to remove it manually.
- [ ] B. Grant `roles/storage.objectViewer` with an IAM Policy Condition evaluating `request.time < timestamp("2026-10-10T17:00:00Z")`.
- [ ] C. Create a temporary Google Account.
- [ ] D. Write a Cloud Function to revoke the role.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
**IAM Conditions** allow administrators to define conditional attribute constraints (such as date and time bounds). Once the specified timestamp passes, the IAM grant automatically stops authorizing access without manual intervention.
</details>

---

### Question 21 [BigQuery Cost Controls & Maximum Bytes Billed]
A developer writes experimental SQL queries in BigQuery that accidentally perform full-table scans across petabyte-scale tables, resulting in unexpected query charges. What query setting should you enforce to prevent queries from running if they exceed a specified data scan limit?
- [ ] A. Enable cached results.
- [ ] B. Configure the `maximum_bytes_billed` limit on queries or client jobs.
- [ ] C. Restrict user IAM to `roles/bigquery.admin`.
- [ ] D. Partition by month only.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
Setting the **`maximum_bytes_billed`** flag on query jobs instructs BigQuery to estimate scanned bytes before execution and fail the query immediately if scanned bytes exceed the defined ceiling.
</details>

---

### Question 22 [Cloud Monitoring Alert Silencing & Maintenance Windows]
Your operations team plans a 3-hour scheduled maintenance outage on production Compute Engine instances. To prevent on-call staff from being flooded with false-positive alerts, what should you configure in Cloud Monitoring?
- [ ] A. Delete all alert policies and recreate them afterwards.
- [ ] B. Create an Alert Snooze (Maintenance Window) covering the duration of the scheduled maintenance.
- [ ] C. Disable the Ops Agent on all instances.
- [ ] D. Unlink the project from Cloud Monitoring.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
**Alert Snooze** in Cloud Monitoring allows operators to suppress notifications for designated alert policies during scheduled maintenance windows while preserving monitoring history and policy configurations.
</details>

---

### Question 23 [GKE Cluster Node Auto-Repair]
A worker node in a GKE cluster fails health checks due to a corrupted kubelet daemon. Which native GKE feature automatically drains, terminates, and replaces the unhealthy node instance?
- [ ] A. Node Auto-Upgrade
- [ ] B. Node Auto-Repair
- [ ] C. Horizontal Pod Autoscaler
- [ ] D. Kubernetes ReplicaSet

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
**Node Auto-Repair** continuously monitors node health checks against the GKE control plane and automatically initiates node recreation when a worker node fails consecutive health checks.
</details>

---

### Question 24 [Cloud Run Minimum Instances for Cold Starts]
A latency-critical microservice deployed on Cloud Run experiences unacceptable 3-second cold start latencies during sudden traffic spikes. How can you eliminate cold starts?
- [ ] A. Increase memory allocation to 32 GB.
- [ ] B. Configure the `min-instances` setting (e.g., set to 2 or more) on the Cloud Run service.
- [ ] C. Deploy the service to Compute Engine.
- [ ] D. Enable HTTP/2 only.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
Configuring **`min-instances`** on Cloud Run keeps a designated baseline of container instances warm and pre-initialized in memory, completely eliminating cold starts for baseline traffic.
</details>

---

### Question 25 [Cloud Armor WAF Protection]
Your web application hosted on an External Application Load Balancer is under a Layer 7 Distributed Denial of Service (DDoS) attack and SQL injection attempts. What service should you configure to protect the application?
- [ ] A. VPC Firewall Rules
- [ ] B. Cloud Armor Security Policies
- [ ] C. Cloud NAT
- [ ] D. Cloud DNSSEC

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
**Google Cloud Armor** is an enterprise Web Application Firewall (WAF) and DDoS mitigation service that integrates directly with External Application Load Balancers to inspect and filter Layer 7 traffic against OWASP Top 10 vulnerabilities and rate-limit attacks.
</details>

---

### Question 26 [Compute Engine Instance Metadata vs Startup Scripts]
You need to pass dynamic configuration parameters (such as an environment flag `ENV=staging`) to a Compute Engine VM so that its startup script can read the value at boot time. Where should you store this parameter?
- [ ] A. In a text file committed to the base OS image.
- [ ] B. In Compute Engine Custom Instance Metadata.
- [ ] C. In an attached read-only Cloud Storage bucket.
- [ ] D. In a VPC subnetwork label.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
Compute Engine **Custom Metadata** stores key-value pairs accessible to the guest OS via the internal metadata server (`http://metadata.google.internal/computeMetadata/v1/instance/attributes/`).
</details>

---

### Question 27 [Cloud Storage Uniform vs Fine-Grained Access]
Google Cloud strongly recommends migrating storage buckets from Fine-Grained Access to Uniform Bucket-Level Access. What happens when Uniform Bucket-Level Access is enabled on a bucket?
- [ ] A. All objects are made publicly readable.
- [ ] B. Access Control Lists (ACLs) are completely disabled, and permissions are managed exclusively through Cloud IAM.
- [ ] C. Encryption keys are deleted.
- [ ] D. Objects can no longer be deleted.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
Enabling **Uniform Bucket-Level Access (UBLA)** deactivates legacy object-level Access Control Lists (ACLs), standardizing security governance exclusively through Google Cloud IAM policies.
</details>

---

### Question 28 [Cloud DNS Private Zones]
Internal virtual machines in a production VPC need to resolve internal service domain names (`db.internal.corp`) to private RFC 1918 IP addresses without publishing DNS records to the public internet. What should you configure?
- [ ] A. A Public Cloud DNS zone.
- [ ] B. A Private Cloud DNS zone visible only to the specified VPC network.
- [ ] C. Custom `/etc/hosts` files pushed via SSH.
- [ ] D. Cloud NAT DNS proxy.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
A **Private Cloud DNS Zone** resolves custom domain names strictly within designated VPC networks, keeping internal architecture hidden from the public internet.
</details>

---

### Question 29 [Cloud Functions Max Instances]
A Cloud Function processes incoming orders from a Cloud Pub/Sub topic and writes records to a downstream legacy database that can handle a maximum of 10 concurrent connections. How do you prevent the function from overwhelming the database?
- [ ] A. Set a Cloud Function timeout of 10 seconds.
- [ ] B. Configure the `max-instances` setting of the Cloud Function to 10.
- [ ] C. Upgrade the database hardware.
- [ ] D. Use Cloud Scheduler.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
Setting **`max-instances`** caps the maximum number of concurrent container execution environments Cloud Functions will scale out to, protecting sensitive downstream databases from connection saturation.
</details>

---

### Question 30 [Secret Manager vs Environment Variables]
An application running on Cloud Run requires an API key to communicate with a payment processor. What is the Google-recommended approach to manage and inject this credential?
- [ ] A. Hardcode the API key in the Dockerfile.
- [ ] B. Pass the API key as plain-text environment variables in the Cloud Run configuration.
- [ ] C. Store the API key in Google Cloud Secret Manager and mount it as a secret environment variable or volume in Cloud Run.
- [ ] D. Store the API key in a public GitHub repository.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `C`

**Why it's correct:**  
**Secret Manager** provides secure, auditable, encrypted storage for sensitive keys and certificates, allowing Cloud Run to access secrets dynamically using IAM permissions.
</details>

---

### Question 31 [GKE Ingress vs NodePort]
You need to expose a web service running across multiple pods in a GKE cluster to external internet users with SSL termination, URL path-based routing, and global Anycast IP integration. What Kubernetes resource should you create?
- [ ] A. ClusterIP Service
- [ ] B. NodePort Service
- [ ] C. Kubernetes Ingress resource backed by the GKE Ingress controller
- [ ] D. HostPort configuration

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `C`

**Why it's correct:**  
Creating a **Kubernetes Ingress** resource in GKE automatically provisions a Google Cloud External Application Load Balancer with Anycast IP routing and SSL termination.
</details>

---

### Question 32 [Filestore Performance Tiers]
A high-performance computing (HPC) research workload requires a shared NFS filesystem providing over 100,000 IOPS and several gigabytes per second of throughput. Which Filestore tier is appropriate?
- [ ] A. Basic HDD
- [ ] B. Basic SSD
- [ ] C. High Scale / Enterprise / Zonal tier with SSD
- [ ] D. Cloud Storage Standard

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `C`

**Why it's correct:**  
Filestore **Zonal / High Scale** tiers provide scalable flash storage delivering high throughput and hundreds of thousands of IOPS for demanding HPC applications.
</details>

---

### Question 33 [Cloud Trace Distributed Tracing]
A multi-tier microservice architecture deployed across Cloud Run, GKE, and Compute Engine experiences sporadic 5-second end-to-end request latency. How can engineers identify the exact microservice causing the bottleneck?
- [ ] A. Inspect CPU charts in Cloud Monitoring.
- [ ] B. Trace request latencies across service boundaries using Cloud Trace.
- [ ] C. Profile code functions using Cloud Profiler.
- [ ] D. Review Admin Activity audit logs.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
**Cloud Trace** collects latency data from distributed applications, generating visual waterfall charts of remote procedure call (RPC) spans to pinpoint latency bottlenecks across services.
</details>

---

### Question 34 [Cloud Profiler Code-Level Optimization]
A Go microservice in production is consuming 90% of its VM CPU capacity. Developers need to inspect CPU utilization at the function and line-of-code level with negligible production overhead. What should they use?
- [ ] A. Cloud Trace
- [ ] B. Cloud Profiler
- [ ] C. Cloud Logging
- [ ] D. Error Reporting

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
**Cloud Profiler** is a continuous code profiling tool that samples CPU and heap memory consumption directly inside application source code functions with less than 1% overhead.
</details>

---

### Question 35 [BigQuery Data Ingestion: Batch vs Streaming]
A company needs to load 50 GB of log files daily into BigQuery at 1:00 AM. Cost minimization is the primary goal, and near-real-time analytical availability is not required. What ingestion method should be chosen?
- [ ] A. BigQuery Storage Write API with streaming inserts.
- [ ] B. Free BigQuery Batch Load jobs from Cloud Storage.
- [ ] C. Cloud Pub/Sub streaming pipeline.
- [ ] D. Insert rows via SQL `INSERT` statements in a loop.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
**BigQuery Batch Load jobs** from Cloud Storage are **100% free of charge** (you only pay for storing the data in BigQuery), whereas streaming ingestion APIs incur per-gigabyte insertion charges.
</details>

---

### Question 36 [Dedicated vs Partner Interconnect Bandwidth]
An enterprise data center requires a 20 Gbps private, dedicated physical connection to Google Cloud that bypasses the public internet completely. What solution satisfies this requirement?
- [ ] A. HA Cloud VPN with 8 tunnels
- [ ] B. Dedicated Interconnect (using two 10 Gbps circuits)
- [ ] C. VPC Network Peering
- [ ] D. Carrier Peering

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
**Dedicated Interconnect** provides direct physical cross-connects at Google colocation facilities in 10 Gbps or 100 Gbps circuit increments.
</details>

---

### Question 37 [Service Account Impersonation vs Keys]
What is the Google-recommended approach for a human administrator to test an application's permissions without generating or downloading a service account JSON key?
- [ ] A. Grant the user's personal account the Owner role temporarily.
- [ ] B. Grant the user the `roles/iam.serviceAccountTokenCreator` role and use Service Account Impersonation via `gcloud --impersonate-service-account`.
- [ ] C. Reset the service account password.
- [ ] D. Add the user as a project editor.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
**Service Account Impersonation** allows authorized users holding `roles/iam.serviceAccountTokenCreator` to generate short-lived tokens and execute commands as the service account without creating permanent private key files.
</details>

---

### Question 38 [Firestore in Datastore Mode vs Native Mode]
When should an enterprise choose Firestore in **Datastore Mode** over Firestore in Native Mode?
- [ ] A. When building mobile applications requiring real-time offline synchronization with Firebase SDKs.
- [ ] B. When migrating existing Google App Engine applications that use the legacy Datastore API.
- [ ] C. When building analytical business intelligence dashboards.
- [ ] D. When standard SQL transactions are required.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
**Datastore Mode** is designed for backend server architectures migrating legacy App Engine Datastore workloads, whereas **Native Mode** provides client SDKs, real-time listeners, and mobile offline support.
</details>

---

### Question 39 [GCP Resource Manager Labels vs Tags]
A company needs to enforce IAM conditional access rules and VPC firewall policies based on an environment classification (`env=prod`). What should they use?
- [ ] A. Resource Labels
- [ ] B. Resource Manager Tags
- [ ] C. Custom metadata attributes
- [ ] D. Cloud Monitoring dashboard names

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
**Resource Manager Tags** are centrally governed at the Organization level and integrate with IAM policy conditions and VPC firewall rules. Labels are for billing and cost reporting only.
</details>

---

### Question 40 [VPC Flow Logs Sampling Rate]
You are enabling VPC Flow Logs on a production subnet to monitor network traffic for security anomalies. The subnet handles heavy internal traffic, and you want to reduce logging ingestion costs while gathering representative telemetry. What setting should you adjust?
- [ ] A. Disable firewall rules.
- [ ] B. Adjust the VPC Flow Logs aggregation interval and reduce the log sampling rate (e.g., to 0.1 or 0.25).
- [ ] C. Delete old log buckets.
- [ ] D. Route all traffic through Cloud NAT.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
VPC Flow Logs supports configurable **sampling rates** (e.g., 25% or 10%) and aggregation intervals (e.g., 5s, 30s) to balance network visibility against Cloud Logging ingestion expenses.
</details>

---

### Question 41 [Compute Engine Custom Machine Types]
A legacy database running on Compute Engine requires exactly 6 vCPUs and 45 GB of RAM. Predefined machine types either provide too few CPUs or excessive memory. How should you provision the VM to optimize cost?
- [ ] A. Choose an oversized `n2-standard-16` instance.
- [ ] B. Provision a Custom Machine Type with exactly 6 vCPUs and 45 GB of RAM.
- [ ] C. Deploy two smaller VMs and cluster them.
- [ ] D. Deploy on App Engine.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
Compute Engine allows creating **Custom Machine Types**, customizing exact vCPU counts and memory allocations to match specific application demands without paying for unneeded capacity.
</details>

---

### Question 42 [Cloud Storage Signed URLs]
An e-commerce website needs to allow authenticated users to upload user profile pictures directly into a private Cloud Storage bucket without granting users Google Cloud accounts or proxying images through web servers. What should you generate?
- [ ] A. A public read/write bucket policy.
- [ ] B. Cloud Storage Signed URLs with temporary write permissions.
- [ ] C. Service Account JSON keys shared with the browser.
- [ ] D. An internal load balancer.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
A **Signed URL** gives time-limited, cryptographically authorized read or write access to a specific Cloud Storage object without requiring the user to have a Google identity.
</details>

---

### Question 43 [App Engine Flexible Scaling vs Manual Scaling]
You have an App Engine Flexible application that maintains persistent WebSocket connections with connected IoT devices. Sudden autoscaling causes connections to drop. How should you configure scaling?
- [ ] A. Automatic scaling with target CPU of 0.5.
- [ ] B. Manual scaling with a fixed number of instances.
- [ ] C. Basic scaling with idle timeout of 5 minutes.
- [ ] D. Cloud Run scale to zero.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
**Manual scaling** in App Engine maintains a fixed number of instances continuously, preventing automated scaling events from severing stateful, persistent TCP/WebSocket connections.
</details>

---

### Question 44 [Error Reporting Automatic Grouping]
A Python web application logging to Cloud Logging encounters multiple uncaught exceptions. Which Operations Suite service automatically groups these exceptions by stack trace, tracks recurrence frequency, and alerts developers?
- [ ] A. Cloud Monitoring
- [ ] B. Cloud Trace
- [ ] C. Error Reporting
- [ ] D. Cloud Profiler

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `C`

**Why it's correct:**  
**Error Reporting** analyzes application stack traces in Cloud Logging, automatically deduplicating and grouping similar exceptions to provide real-time alert triage.
</details>

---

### Question 45 [Dataflow Exactly-Once Processing]
Which Google Cloud serverless processing service provides unified stream and batch processing based on Apache Beam with built-in exactly-once processing semantics?
- [ ] A. Cloud Dataproc
- [ ] B. Cloud Dataflow
- [ ] C. BigQuery Data Transfer Service
- [ ] D. Cloud Pub/Sub

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
**Cloud Dataflow** is a fully managed, serverless execution service for Apache Beam pipelines that guarantees exactly-once processing across unbounded streaming and bounded batch data.
</details>

---

### Question 46 [Cross-Project Service Account Access]
An application running on a Compute Engine VM in `project-production` needs to read a BigQuery dataset in `project-analytics`. What is the secure way to grant access?
- [ ] A. Move both projects into the same VPC.
- [ ] B. Grant the VM's Service Account the `roles/bigquery.dataViewer` role in `project-analytics`.
- [ ] C. Create a service account key and hardcode it in the VM.
- [ ] D. Grant the default Compute Engine service account Owner on both projects.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
Service Accounts are cross-project IAM identities. Granting the service account from `project-production` the necessary role directly on the resource in `project-analytics` satisfies access across project boundaries securely.
</details>

---

### Question 47 [Cloud Storage Retention Policy Lock (WORM)]
A healthcare enterprise must guarantee that medical scan images cannot be deleted or overwritten by anyone for 5 years to satisfy legal compliance. What feature must be used?
- [ ] A. Object Versioning
- [ ] B. Cloud Storage Bucket Lock (Locked Retention Policy)
- [ ] C. Customer-Supplied Encryption Keys (CSEK)
- [ ] D. Cloud Spanner

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
A **Locked Retention Policy (Bucket Lock)** enforces Write Once, Read Many (WORM) compliance, permanently preventing object deletion or reduction of retention period until the retention duration expires.
</details>

---

### Question 48 [Compute Engine Local SSD Performance vs Persistence]
Which storage option provides the lowest latency and highest IOPS for Compute Engine, but loses all stored data when the VM instance is stopped or terminated?
- [ ] A. Standard Persistent Disk
- [ ] B. Balanced Persistent Disk
- [ ] C. Local SSD
- [ ] D. Cloud Storage Nearline

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `C`

**Why it's correct:**  
**Local SSDs** are physically attached to the server hosting the VM, delivering extreme sub-millisecond IOPS at the cost of being ephemeral (data is lost on VM stop).
</details>

---

### Question 49 [Cloud Pub/Sub Dead-Letter Topics]
A message processing worker on Cloud Run repeatedly crashes when attempting to parse malformed JSON messages from a Cloud Pub/Sub subscription. How can you prevent poison pill messages from blocking subscription processing?
- [ ] A. Delete the Pub/Sub subscription.
- [ ] B. Configure a Dead-Letter Topic on the subscription with a maximum delivery attempts threshold.
- [ ] C. Increase Pub/Sub message retention to 30 days.
- [ ] D. Filter messages in Cloud Logging.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
A **Dead-Letter Topic** isolates unparseable or repeatedly failing messages after a configured number of delivery attempts, preventing subscription stalls.
</details>

---

### Question 50 [Cloud Monitoring Uptime Checks with Alert Policies]
An external customer-facing e-commerce storefront must be continuously verified for availability from multiple geographic locations worldwide. What should be configured?
- [ ] A. Cloud Trace latency span
- [ ] B. Cloud Monitoring Uptime Check paired with an Alert Policy
- [ ] C. Cloud Profiler agent
- [ ] D. Cloud Logging sink routing to Pub/Sub

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
**Cloud Monitoring Uptime Checks** periodically send synthetic HTTP/HTTPS requests from edge points of presence across the globe to verify external reachability and trigger alerts if health checks fail.
</details>

---

## Score Tracking Sheet

| Date | Score (/50) | Percentage | Target Met? (≥80%) | Weak Domains Identified |
| :--- | :---: | :---: | :---: | :--- |
| YYYY-MM-DD | -- | --% | [ ] Yes [ ] No | |
