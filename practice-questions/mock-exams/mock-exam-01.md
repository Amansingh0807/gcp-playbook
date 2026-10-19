# ACE Full-Length Mock Exam 01

> **Exam Timing:** 2 Hours (120 Minutes)  
> **Total Questions:** 50 Questions (Mixed Domains)  
> **Passing Score:** 40/50 (80%+)  
> **Instructions:** Simulates the real Google Cloud Associate Cloud Engineer exam environment. Attempt in one continuous sitting without consulting external documentation.

---

## Domain Coverage Matrix
- **Section 1:** Setting up a cloud solution environment (~20%)
- **Section 2:** Planning and configuring a cloud solution (~16%)
- **Section 3:** Deploying and implementing a cloud solution (~25%)
- **Section 4:** Ensuring successful operation of a cloud solution (~20%)
- **Section 5:** Configuring access and security (~19%)

---

## Exam Questions (1 to 50)

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

---

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

### Question 6 [GKE Workload Architecture & Scaling]
You are planning to migrate a database and a backend application to a Standard Google Kubernetes Engine (GKE) cluster. You need to prevent data loss and make sure there are enough nodes available for your backend application based on the demands of your workloads. You want to follow Google-recommended practices and minimize manual work. What should you do?
- [ ] A. Run your database as a single Pod. Run the resize command when you notice changes in workload demand.
- [ ] B. Run your database as a StatefulSet. Configure cluster autoscaling to handle changes in the demands of your workloads.
- [ ] C. Run your database as a Deployment. Configure cluster autoscaling to handle changes in the demands of your workloads.
- [ ] D. Run your database as a DaemonSet. Run the resize command when you notice changes in workload demand.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
- Stateful workloads (databases) requiring stable network identifiers and persistent disk attachments must be deployed as a **StatefulSet** in Kubernetes.
- Enabling **Cluster Autoscaler** allows GKE to automatically add or remove worker nodes in response to pending Pod scheduling demand without manual intervention.

**Why other options are incorrect:**  
- **A:** Standalone pods lack persistence guarantees and manual resizing requires constant human monitoring.
- **C:** Deployments are designed for stateless workloads; pods are interchangeable and dynamic disk bindings risk data corruption.
- **D:** DaemonSets run one pod on every node, which is inappropriate for a centralized database.
</details>

---

### Question 7 [On-Premises Authentication & Identity]
You are implementing device management in your cloud organization. You have many applications on-premises that need to authenticate with Google Cloud. You want to avoid managing service account keys and implement a solution that uses managed services. What should you do?
- [ ] A. Use application default credentials with a Google Account.
- [ ] B. Use Workload Identity Federation.
- [ ] C. Use an API key stored in Secret Manager.
- [ ] D. Use a service account and set the account key file stored in Secret Manager.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
**Workload Identity Federation** allows on-premises systems and external cloud providers (AWS, Azure, OIDC/SAML providers) to securely exchange their native credentials for short-lived Google Cloud access tokens, completely eliminating the need to create, download, and rotate static service account JSON keys.

**Why other options are incorrect:**  
- **A:** Google Accounts are designed for human interactive authentication.
- **C & D:** API keys and service account key files still introduce static credential management risks.
</details>

---

### Question 8 [Storage Security & Least Privilege]
You are deploying an application on a Compute Engine VM. The application needs to store operational logs in a Cloud Storage bucket named `project-logs-us-central1` and create new objects in this bucket. The application must be restricted from modifying or deleting existing objects or accessing any other buckets in the project. You must follow least privilege. What should you do?
- [ ] A. Grant the Compute Engine default service account the predefined `roles/storage.objectCreator` role on the project.
- [ ] B. Create a dedicated service account for the application. Grant this service account the predefined `roles/storage.objectCreator` role on the `project-logs-us-central1` bucket. Attach the service account to the VM instance.
- [ ] C. Create a dedicated service account for the application. Generate a service account key, grant `roles/storage.admin` on the bucket, and store the key file on the VM.
- [ ] D. Grant the Compute Engine default service account `roles/storage.objectAdmin` on the project.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
- Creating a dedicated user-managed Service Account isolates permissions to this specific application.
- `roles/storage.objectCreator` grants permissions strictly to write new objects, without read, modify, or delete privileges.
- Applying the IAM binding at the **bucket level** restricts access strictly to `project-logs-us-central1`.
- Attaching the service account directly to the VM eliminates static private keys.

**Why other options are incorrect:**  
- **A & D:** Modifying the project-wide default service account impacts all VMs in the project and violates bucket boundary least privilege.
- **C:** Grants administrative permissions and downloads insecure static keys.
</details>

---

### Question 9 [High-Throughput Low-Latency Database]
You are designing a backend system for a global multiplayer mobile game. The system needs to store and retrieve player state data, such as inventory and current level, with single-digit millisecond latency. You need a storage layer that handles massive concurrent read and write operations. What should you do?
- [ ] A. Configure a Cloud SQL for MySQL database.
- [ ] B. Store player data as individual JSON files in a single-region Cloud Storage bucket.
- [ ] C. Store player data in Cloud Bigtable.
- [ ] D. Aggregate player data into BigQuery tables.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `C`

**Why it's correct:**  
**Cloud Bigtable** is Google Cloud's low-latency, petabyte-scale NoSQL wide-column database engineered specifically for workloads requiring massive concurrent throughput with consistent **single-digit millisecond latency** (e.g., gaming leaderboards, player profiles, IoT streams).

**Why other options are incorrect:**  
- **A:** Cloud SQL is vertically scaled and struggles with massive global concurrent game state writes.
- **B:** Cloud Storage object latency is in the tens/hundreds of milliseconds.
- **D:** BigQuery is an analytical OLAP warehouse, not an operational transactional store.
</details>

---

### Question 10 [Serverless Web Application Deployment]
You developed a containerized web application that serves traffic for a local event with unpredictable traffic surges. You want to deploy it on Google Cloud while minimizing ongoing costs. What should you do?
- [ ] A. Deploy the web application as a Cloud Run job.
- [ ] B. Deploy the web application on Google Kubernetes Engine in Standard mode.
- [ ] C. Deploy the web application on Google Kubernetes Engine in Autopilot mode.
- [ ] D. Deploy the web application as a Cloud Run service.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `D`

**Why it's correct:**  
A **Cloud Run service** automatically handles incoming HTTP web traffic, autoscales instantly from zero to thousands of container instances, and scales back down to **zero instances** when traffic subsides. You only pay for exact compute resources consumed during active request processing.

**Why other options are incorrect:**  
- **A:** Cloud Run jobs are for batch tasks that run to completion, not long-lived HTTP web servers.
- **B & C:** GKE Standard and Autopilot maintain minimum cluster and workload baselines that incur costs even when idle.
</details>

---

### Question 11 [Enterprise Landing Zone & Shared Networking]
You are setting up a landing zone in Google Cloud where multiple teams can deploy workloads. You want to ensure that all VMs in your production environment can communicate with each other through private IP addresses. You also want to allow all VMs in your organization to accept connections on specific TCP ports while minimizing administrative overhead. What should you do?
- [ ] A. Create individual VPCs per project, peer all VPCs together, and apply organization policies at the organization level.
- [ ] B. Create a Host VPC project with each production project as its Service Project. Apply hierarchical firewall policies at the organization level.
- [ ] C. Create a Host VPC project with each production project as its Service Project. Apply organization policies at the organization node.
- [ ] D. Create individual VPCs for each project, peer all VPCs together, and apply hierarchical firewall policies at the organization level.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
- **Shared VPC (Host & Service Projects):** Provides private RFC 1918 communication across multiple projects under centralized network administration.
- **Hierarchical Firewall Policies:** Enforce consistent firewall port rules organization-wide at the organization or folder node, eliminating the need to create repetitive firewall rules in individual projects.

**Why other options are incorrect:**  
- **A & D:** Full-mesh VPC peering between dozens of projects creates immense administrative overhead and hits peering quotas.
- **C:** Organization policies govern resource constraints (e.g., location, public IP prevention), not specific TCP firewall port permissions.
</details>

---

### Question 12 [Cloud Operations & AI Diagnostics]
You are troubleshooting a complex application failure. You found several cryptic error messages containing no personally identifiable information (PII) in Cloud Logging. You need to understand the meaning of these logs and determine what action to take as quickly as possible. What should you do first?
- [ ] A. Search the application source code to determine the cause of the errors.
- [ ] B. Manually copy the error messages and search for them in public forums.
- [ ] C. Use Cloud Profiler to analyze the application's performance.
- [ ] D. Use the "Explain this log entry" feature in Logs Explorer to start a conversation with Gemini.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `D`

**Why it's correct:**  
Cloud Logging features the native **"Explain this log entry"** capability powered by Gemini Cloud Assist. It provides immediate natural language diagnostic breakdowns of cryptic stack traces and suggests concrete remediation steps directly in the console.

**Why other options are incorrect:**  
- **A & B:** Manual code inspection and forum searching are time-consuming manual workflows.
- **C:** Cloud Profiler measures code-level CPU/heap memory consumption, not log message interpretation.
</details>

---

### Question 13 [Cloud Storage Lifecycle Management]
You manage binary large objects (BLOBs) in a Cloud Storage bucket with Object Versioning enabled. The bucket is updated frequently by an automated process. You need to implement a cost-control measure that automatically and permanently deletes noncurrent object versions 30 days after they are replaced by a newer version. What should you do?
- [ ] A. Create an Object Lifecycle Management rule that specifies the `daysSinceNoncurrentTime` condition set to 30 and uses the Delete action.
- [ ] B. Create an Object Lifecycle Management rule that specifies the `age` condition set to 30 days and uses the Delete action.
- [ ] C. Create a nightly scheduled Cloud Run function that lists and deletes noncurrent versions.
- [ ] D. Configure a Bucket Lock retention policy on the bucket set to 30 days.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `A`

**Why it's correct:**  
The **`daysSinceNoncurrentTime`** condition in Cloud Storage Object Lifecycle Management explicitly measures the elapsed time since an object became noncurrent (i.e., replaced by a newer version) and pairs with the `Delete` action to permanently clean historical versions.

**Why other options are incorrect:**  
- **B:** The `age` condition measures time from original object creation, which would delete current live objects if they were uploaded 30 days ago.
- **C:** Custom scripts introduce unnecessary maintenance and compute costs.
- **D:** Bucket Lock enforces retention to prevent deletion, which is the opposite of cost-control deletion.
</details>

---

### Question 14 [Organization Governance & Resource Constraints]
Your company operates in the European Economic Area (EEA) and adopts Google Cloud. Projects are structured in folders. You need to ensure any resources deployed across your company are restricted strictly to Google Cloud physical locations within the EEA. What should you do?
- [ ] A. Configure the policy at the organization level, and add all allowed EEA locations to the policy.
- [ ] B. Configure the policy at the organization level, and add all disallowed locations to the policy.
- [ ] C. Configure the policy at the folder level, and add all disallowed locations to the policy.
- [ ] D. Configure the policy at the folder level, and add all allowed locations to the policy.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `A`

**Why it's correct:**  
Setting the **Resource Location Restriction (`gcp.resourceLocations`)** constraint at the **organization level** enforces the rule across all child folders and projects through inheritance. Using an **allowlist** (adding all allowed EEA locations) ensures any newly introduced non-EEA Google Cloud regions are blocked by default.
</details>

---

### Question 15 [Project Creation & Billing Delegation]
Your enterprise development team requires the ability to programmatically create Google Cloud projects through a CI/CD pipeline using a dedicated service account. All projects must be created under a folder named `development-projects` and linked to billing account `billing-account-id-12345`. Following least privilege, what IAM roles must be granted to the service account?
- [ ] A. On `development-projects` folder, grant `roles/resourcemanager.projectCreator`. On the Organization node, grant `roles/billing.user`.
- [ ] B. Create a custom IAM role with `resourcemanager.projects.create` and `billing.projects.link`. Grant it on the folder.
- [ ] C. On `development-projects` folder, grant `roles/resourcemanager.projectCreator`. On the billing account `billing-account-id-12345`, grant `roles/billing.user`.
- [ ] D. On `development-projects` folder, grant `roles/owner`. On the billing account, grant `roles/billing.admin`.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `C`

**Why it's correct:**  
- `roles/resourcemanager.projectCreator` granted on the target folder allows creating projects within that folder.
- `roles/billing.user` granted specifically on the target billing account allows linking projects to that specific billing account without administrative billing control.

**Why other options are incorrect:**  
- **A:** Granting `billing.user` on the organization node grants rights to link across all billing accounts in the company.
- **B:** Custom roles cannot bridge resource manager and billing account permission boundaries.
- **D:** `roles/owner` and `roles/billing.admin` massively violate least privilege.
</details>

---

### Question 16 [Security Auditing & Cloud Asset Inventory]
Your organization has created hundreds of service accounts across projects for on-premises and multi-cloud applications. You need to audit all service account keys across the entire organization and identify keys that are older than 90 days. What should you do?
- [ ] A. Execute `gcloud kms keys list --filter="createTime < [DATE_90_DAYS_AGO]"`.
- [ ] B. Execute `gcloud iam service-accounts list --filter="createTime < [DATE_90_DAYS_AGO]"`.
- [ ] C. Execute `gcloud asset search-all-resources --scope="organizations/[ORG_ID]" --query="createTime < [DATE_90_DAYS_AGO]" --asset-types="iam.googleapis.com/ServiceAccountKey" --order-by="createTime"`.
- [ ] D. Execute `gcloud asset search-all-resources --asset-types="apikeys.googleapis.com/Key"`.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `C`

**Why it's correct:**  
**Cloud Asset Inventory** (`gcloud asset search-all-resources`) searches across all projects in an entire Organization in a single query. Querying `iam.googleapis.com/ServiceAccountKey` with a `createTime` filter retrieves all stale user-managed keys organization-wide.

**Why other options are incorrect:**  
- **A:** Audits Cloud KMS encryption keys, not IAM service account keys.
- **B:** `iam service-accounts list` is project-scoped and returns service accounts, not key creation metadata.
- **D:** Audits API Keys, not IAM service account keys.
</details>

---

### Question 17 [Extreme Compute & Storage Performance]
You need to deploy a third-party software application onto a single Compute Engine VM. The application requires the highest speed read and write disk access for its internal database and must recover instance state on failure. What should you do?
- [ ] A. Create an instance template with SSD Persistent Disk as part of a stateless MIG.
- [ ] B. Create an instance template with SSD Persistent Disk as part of a stateful MIG.
- [ ] C. Create an instance template with Hyperdisk Extreme as part of a stateless MIG.
- [ ] D. Create an instance template with Hyperdisk Extreme as part of a stateful Managed Instance Group (MIG).

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `D`

**Why it's correct:**  
- **Hyperdisk Extreme** delivers Google Cloud's highest IOPS and throughput performance (up to 500,000 IOPS) for demanding database workloads.
- A **Stateful Managed Instance Group (MIG)** preserves instance disk state, IP address, and metadata during VM recreation and failure recovery.

**Why other options are incorrect:**  
- **A & C:** Stateless MIGs recreate VMs with blank new disks, causing complete database data loss.
- **B:** Standard SSD Persistent Disks deliver significantly lower peak IOPS than Hyperdisk Extreme.
</details>

---

### Question 18 [Principal Access Boundaries]
You manage security for your Google Cloud organization. You need to prevent all organization users and principals from accessing or creating Google Cloud resources outside of your cloud organization (e.g., in external personal accounts). What should you do?
- [ ] A. Create an entitlement in Privileged Access Manager at the organization node.
- [ ] B. Create an IAM policy at the organization node with an IAM condition restricting resource names.
- [ ] C. Create a Principal Access Boundary policy for your organization node and apply it to your organization's principal set.
- [ ] D. Create an IAM deny policy at the organization node for the organization's principal set.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `C`

**Why it's correct:**  
**Principal Access Boundary (PAB) policies** restrict the set of Google Cloud resources that organization principals (users and service accounts) can access, preventing enterprise identities from accessing or exfiltrating data into external GCP projects outside the approved boundary.
</details>

---

### Question 19 [Private Outbound Internet Connectivity]
Your organization's security policy strictly prohibits assigning external public IP addresses to Compute Engine VMs. However, an application on a private VM must access the public internet to download software patches. What should you do?
- [ ] A. Create a bastion host with an external IP address and connect via SSH.
- [ ] B. Assign an internal static IP address to the VM.
- [ ] C. Configure Cloud NAT for the subnet that the VM resides in.
- [ ] D. Set up a VPC Network Peering connection to an external network.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `C`

**Why it's correct:**  
**Cloud NAT** provides managed outbound-only Network Address Translation for subnets, allowing private VMs without public external IPs to download internet updates while blocking unsolicited inbound connections from the internet.
</details>

---

### Question 20 [Linux Identity & Access Governance]
Developers provision Linux VMs in Compute Engine to execute periodic engineering tasks. You need to manage user Linux accounts and SSH access following Google-recommended practices while minimizing operational costs. What should you do?
- [ ] A. Require developers to generate public SSH keys and assign root ownership.
- [ ] B. Require developers to create public SSH keys and use custom startup scripts.
- [ ] C. Enable OS Login for all VMs and write startup scripts.
- [ ] D. Enable OS Login for all VMs, and use IAM roles to grant user access permissions.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `D`

**Why it's correct:**  
**OS Login** integrates Compute Engine Linux user accounts directly with Google Cloud Identity and IAM. Administrators grant access via standard IAM roles (`roles/compute.osLogin` or `roles/compute.osAdminLogin`), tying SSH access to corporate identity lifecycle and multi-factor authentication.
</details>

---

### Question 21 [Cost-Effective Application Backups]
You need to store application data backups outside of the application server virtual machine and only pay for the exact volume of storage that the backups consume. What should you do?
- [ ] A. Create a Cloud Storage bucket and use client libraries to store the backups.
- [ ] B. Create a Firestore instance in Datastore mode.
- [ ] C. Deploy a Filestore instance and mount via NFS.
- [ ] D. Deploy Google Cloud NetApp Volumes with a dedicated storage pool.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `A`

**Why it's correct:**  
**Cloud Storage** is fully serverless object storage that bills strictly per gigabyte consumed without provisioning fixed storage capacity pools in advance.
</details>

---

### Question 22 [Unified Fleet Database Observability]
You manage a fleet of diverse data storage solutions across Google Cloud, including Cloud Bigtable, Cloud SQL, Memorystore, and Cloud Spanner. You need a single, unified console dashboard to monitor the health of all database instances and identify instances at risk of downtime. What should you use?
- [ ] A. Run `terraform state list` in the IaC deployment directory.
- [ ] B. In the Google Cloud Console, navigate to Database Center.
- [ ] C. Use Dataplex Knowledge Catalog to search database metadata.
- [ ] D. Inspect Cloud Asset Inventory resources.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
**Database Center** in the Google Cloud Console provides a centralized, unified fleet management view across Cloud SQL, Spanner, Bigtable, and Memorystore, proactively surfacing availability, health, security, and backup risks.
</details>

---

### Question 23 [Non-Disruptive Subnet Expansion]
You manage a VPC subnet that is rapidly approaching private IP address capacity. The number of Compute Engine VMs in the region will double within a week. You must implement a Google-recommended solution that minimizes costs and requires zero downtime. What should you do?
- [ ] A. Create a second VPC with the same IP range and peer them.
- [ ] B. Use the Google Cloud CLI (`gcloud compute networks subnets expand-ip-range`) to expand the primary IP range of the subnet.
- [ ] C. Route additional traffic from the expanded range using firewall rules.
- [ ] D. Delete the existing subnet and recreate it with a larger CIDR block.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
`gcloud compute networks subnets expand-ip-range` expands an existing subnet's primary IP prefix (e.g., `/24` to `/22`) in place non-disruptively with **zero downtime** and zero impact on running instances.
</details>

---

### Question 24 [Automated Snapshot Schedules]
A production workload runs on a single Compute Engine VM. Disaster recovery policies mandate daily backups of the entire instance disk data retained for 7 days with minimal setup and configuration. What should you do?
- [ ] A. Configure Cloud Scheduler to trigger a Cloud Run function creating and deleting machine images daily.
- [ ] B. Configure Persistent Disk asynchronous replication.
- [ ] C. Configure daily scheduled Persistent Disk snapshots with a retention period of 7 days.
- [ ] D. Write a bash script using `gsutil` running via cron.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `C`

**Why it's correct:**  
Compute Engine supports native **Scheduled Snapshot Policies**. You define a policy with daily schedule and a 7-day retention period, and attach it to the disk. Google Cloud manages snapshot generation and automatic lifecycle pruning natively.
</details>

---

### Question 25 [Serverless to GKE Private Connectivity]
You are deploying a web service to Cloud Run that must call an internal API running on a private GKE cluster in your VPC. The API must not be exposed to the public internet. What should you do?
- [ ] A. Create an ingress firewall rule on the VPC allowing `0.0.0.0/0` on port 443.
- [ ] B. Deploy an ingress resource exposing GKE to the internet and filter via Cloud Armor.
- [ ] C. Create an egress firewall rule on the VPC to `0.0.0.0/0`.
- [ ] D. Deploy an Internal Application Load Balancer to expose the GKE API internally, configure Cloud DNS, and attach a Serverless VPC Access connector to the Cloud Run service.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `D`

**Why it's correct:**  
To connect a serverless runtime (Cloud Run) privately to an internal VPC service:
1. Expose the GKE workload behind an **Internal Application Load Balancer (ILB)**.
2. Deploy a **Serverless VPC Access connector** on the VPC.
3. Route Cloud Run outbound traffic through the connector to reach the private internal ILB IP or Cloud DNS FQDN.
</details>

---

### Question 26 [Service Account Administration Permissions]
A new service account needs the ability to invoke a Cloud Run service. You need to grant your own user account the minimum necessary permissions to modify IAM policies on service accounts following least privilege. What role should you request?
- [ ] A. `roles/iam.serviceAccountAdmin`
- [ ] B. `roles/owner`
- [ ] C. `roles/editor`
- [ ] D. `roles/run.invoker`

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `A`

**Why it's correct:**  
**`roles/iam.serviceAccountAdmin`** provides administrative permissions to create, update, and manage IAM policy bindings on Service Accounts. `roles/run.invoker` allows invoking the service, but not administering service account IAM policies.
</details>

---

### Question 27 [Lift-and-Shift Migration with Hardcoded IPs]
Your company is migrating on-premises applications to Google Cloud under tight deadlines. Several legacy systems connect to each other via hardcoded internal IP addresses. There is no network connection between on-premises and Google Cloud. You want to migrate quickly without modifying application code. What should you do?
- [ ] A. Create a VPC with the identical CIDR ranges as your on-premises network, and assign each migrated workload its original static internal IP address.
- [ ] B. Migrate workloads to a single subnet and configure Cloud NAT.
- [ ] C. Create non-overlapping CIDR ranges and assign new ephemeral IPs.
- [ ] D. Migrate DNS servers and configure forwarding zones.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `A`

**Why it's correct:**  
Because the on-premises and cloud environments are completely disconnected, you can configure the Google Cloud VPC with the **identical CIDR block** and assign workloads the exact same static private IP addresses, allowing hardcoded internal IPs to function with zero code changes.
</details>

---

### Question 28 [Scalable Multi-VPC Service Publishing]
You have an internal service deployed in its own VPC exposed via an Internal Network Load Balancer. Multiple application teams in separate projects and distinct VPC networks require secure access to this service. What is the most scalable, secure solution?
- [ ] A. Configure a Cloud NAT gateway in the service VPC.
- [ ] B. Expose the service via an External Load Balancer with firewall rules.
- [ ] C. Publish the service using a Private Service Connect (PSC) service attachment.
- [ ] D. Establish full-mesh VPC Network Peering to all consumer VPCs.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `C`

**Why it's correct:**  
**Private Service Connect (PSC)** allows service producers to publish services using service attachments. Consumer VPCs create private endpoints that connect over Google's SDN without complex VPC peering, avoiding IP overlap issues and peering limits.
</details>

---

### Question 29 [FinOps Monitoring & BigQuery Billing Export]
Your company needs to allow different teams to monitor their Google Cloud costs. Teams must receive notifications when spend crosses thresholds and create custom Looker Studio dashboards from detailed billing data with minimum engineering overhead. What should you do?
- [ ] A. Set up budget alerts for each team, write a shell script to query the Billing API, and push to BigQuery.
- [ ] B. Set up budget alerts for each team based on required thresholds, configure automated Cloud Billing export to BigQuery, and grant teams access to BigQuery.
- [ ] C. Deploy Grafana on Compute Engine and query the Cloud Billing API.
- [ ] D. Deploy Grafana and build custom dashboards from the Budget API.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
- **Cloud Budgets:** Provide automated email threshold notifications per project/team.
- **Native BigQuery Billing Export:** Streams daily itemized billing data into BigQuery with zero custom code, allowing teams to query costs and build Looker Studio dashboards.
</details>

---

### Question 30 [GKE Workload Identity Microservice Isolation]
You are deploying two microservices to GKE: `metrics-scraper` (needs `monitoring.viewer`) and `data-processor` (needs `storage.objectCreator` on a bucket). You must ensure neither service can assume the other's permissions following least privilege. What should you do?
- [ ] A. Create a single Google Service Account with both roles and enable Workload Identity.
- [ ] B. Create two Google Service Accounts with keys stored in Kubernetes Secrets.
- [ ] C. Grant the node default service account both roles.
- [ ] D. Enable Workload Identity on GKE. Create two separate Google Service Accounts with their respective roles. Create two Kubernetes Service Accounts, bind each KSA to its respective GSA, and assign each KSA to its corresponding deployment.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `D`

**Why it's correct:**  
Configuring distinct **Kubernetes Service Accounts (KSAs)** mapped 1-to-1 to dedicated **Google Service Accounts (GSAs)** via Workload Identity ensures complete pod-level least-privilege isolation.
</details>

---

### Question 31 [Multi-Application Infrastructure as Code at Scale]
Your DevOps team supports deploying 50 new standard applications (Cloud Run, Compute Engine, Cloud SQL) across dev, test, QA, and prod environments. You need a scalable deployment process that minimizes errors. What should you do?
- [ ] A. Create an operational runbook for manual Console deployment.
- [ ] B. Create a Terraform module with an application template, use a centralized Cloud Storage bucket for remote state, include the module in application code, and deploy via Cloud Build.
- [ ] C. Consolidate deployment instructions into a shared Google Doc.
- [ ] D. Place all 50 applications into a single monolithic Terraform root module.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
Creating reusable **Terraform modules**, storing state in a secured Cloud Storage remote backend, and automating execution via **Cloud Build** CI/CD pipelines ensures repeatable, error-free, version-controlled infrastructure delivery.
</details>

---

### Question 32 [Declarative Serverless Deployment with Terraform]
You are deploying a serverless microservice to Cloud Run using automated Infrastructure as Code. What is the standard declarative approach?
- [ ] A. Write a Python script calling `subprocess.run(["gcloud", "run", "deploy"])`.
- [ ] B. Use Gemini CLI `/deploy`.
- [ ] C. Write a shell script using `gcloud run deploy --source`.
- [ ] D. Write a `main.tf` file defining a `google_cloud_run_v2_service` resource and execute `terraform apply`.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `D`

**Why it's correct:**  
Defining resources declaratively in HCL (`main.tf`) using the Google Cloud Terraform provider (`google_cloud_run_v2_service`) is the standard enterprise Infrastructure as Code methodology.
</details>

---

### Question 33 [Immutable Organizational Governance & Sinks]
You need to configure Organization Policies and Log Sinks that cannot be modified or deleted by departmental Project Owners who hold the `roles/owner` role on their projects. What should you do?
- [ ] A. Use standard project naming conventions and configure policies on projects.
- [ ] B. Configure policies on the organization and sinks on the projects.
- [ ] C. Organize projects under department Folders. Configure both Organization Policies and Log Sinks at the Folder level.
- [ ] D. Ask Project Owners not to modify sinks.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `C`

**Why it's correct:**  
Organization Policies and aggregated Log Sinks configured at the **Folder or Organization level** are inherited by child projects. Project Owners lack permissions to alter policies or sinks attached to parent folder nodes.
</details>

---

### Question 34 [Least Privilege for GKE Developers]
A new developer joins the cloud engineering team. They need to deploy containerized workloads to an existing GKE cluster, but must not manage cluster infrastructure or other GCP services. What role should you grant?
- [ ] A. Add the developer to the Google Group that holds the `roles/container.developer` role.
- [ ] B. Grant the developer `roles/container.admin` on the project.
- [ ] C. Grant the developer `roles/editor` on the project.
- [ ] D. Grant the developer `roles/container.serviceAgent`.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `A`

**Why it's correct:**  
**`roles/container.developer`** provides access to deploy and manage Kubernetes workloads (Deployments, Pods, Services) via the Kubernetes API without granting permissions to resize or configure cluster nodes.
</details>

---

### Question 35 [Scheduled Serverless Batch Processing]
Your accounting team needs to execute an overnight batch data processing workload every day. The solution must minimize cost and automatically retry executions if a failure occurs. What should you do?
- [ ] A. Develop the batch workload and deploy it as a Cloud Run job triggered daily by Cloud Scheduler.
- [ ] B. Deploy a 24/7 web listener on a GKE Deployment.
- [ ] C. Deploy an application as a GKE CronJob on a 3-node cluster.
- [ ] D. Deploy a web application on a Cloud Run service.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `A`

**Why it's correct:**  
**Cloud Run jobs** run containerized batch tasks to completion with built-in retry policies, incurring zero cost when not running. **Cloud Scheduler** triggers the job on a cron schedule.
</details>

---

### Question 36 [Log Views & Scoped Audit Access]
Centralized logging aggregates logs from frontend and backend projects into a central logging project. Frontend developers must be permitted to view all frontend logs and ONLY error-level logs from the backend project. What should you do?
- [ ] A. Write a query in Logs Explorer combining the logs.
- [ ] B. Create two separate log sinks.
- [ ] C. In the centralized project, create a Log View on the destination log bucket filtering for all frontend logs and backend logs with severity >= ERROR. Grant the frontend team `roles/logging.viewAccessor` on this Log View.
- [ ] D. Grant the frontend team `roles/logging.viewer` on both projects.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `C`

**Why it's correct:**  
**Log Views** provide fine-grained, secure access control over log entries within a log bucket. Granting **`roles/logging.viewAccessor`** on a filtered Log View enforces least-privilege log visibility.
</details>

---

### Question 37 [Real-Time Third-Party Log Streaming]
Your enterprise needs to stream application logs from Cloud Logging to an external third-party SaaS SIEM tool with minimal latency. What should you configure?
- [ ] A. Create a Cloud Logging sink with Cloud Pub/Sub as the destination, and configure the SaaS tool to subscribe to the topic.
- [ ] B. Trigger a Cloud Run function via Cloud Scheduler hourly.
- [ ] C. Create a Log Sink routing to Cloud Storage.
- [ ] D. Create a Log Sink routing to BigQuery.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `A`

**Why it's correct:**  
Exporting logs via a Cloud Logging sink to **Cloud Pub/Sub** provides sub-second streaming ingestion for external SIEM and monitoring platforms.
</details>

---

### Question 38 [Enforcing Organization-Wide OS Login]
You need to enforce OS Login across all Linux Compute Engine instances in all projects across your organization to eliminate manual SSH key management. What should you do?
- [ ] A. Add SSH keys to project metadata.
- [ ] B. Enforce the Organization Policy constraint `compute.requireOsLogin` across the organization.
- [ ] C. Enable OS Login with 2FA manually on each VM.
- [ ] D. Configure a service account to push SSH keys.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
Enforcing the **`compute.requireOsLogin`** Organization Policy constraint ensures all existing and future Compute Engine virtual machines require OS Login organization-wide.
</details>

---

### Question 39 [Modernizing Legacy NFS Shared Storage]
Multiple legacy applications share files via an on-premises NFS server. You need to modernize the NFS server on Google Cloud with minimal code changes. What should you do?
- [ ] A. Migrate applications to Cloud Storage client libraries.
- [ ] B. Deploy a Google Cloud Filestore instance and update NFS mount points.
- [ ] C. Migrate data to Firestore.
- [ ] D. Run an NFS daemon on a Compute Engine VM.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
**Filestore** provides managed NFSv3 network-attached storage that mounts directly into Linux filesystems with zero application code refactoring.
</details>

---

### Question 40 [Custom Roles for VM Operations]
An operations team needs permissions strictly to start, stop, and reset Compute Engine instances for troubleshooting, but must be prohibited from deleting instances or altering network settings. What should you do?
- [ ] A. Grant `roles/editor` with an IAM condition.
- [ ] B. Create a Custom IAM Role with `compute.instances.start`, `compute.instances.stop`, and `compute.instances.reset`, and assign it to the team.
- [ ] C. Grant `roles/compute.instanceAdmin.v1` with an exclusion condition.
- [ ] D. Grant `roles/compute.viewer`.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
Creating a **Custom IAM Role** containing only the three exact operational permissions enforces strict least privilege without granting deletion rights.
</details>

---

### Question 41 [Compute Disk Snapshot Policy]
A company needs daily automated disk backups of a Compute Engine instance with a 14-day retention policy. What is the Google-recommended approach?
- [ ] A. A cron job executing `gcloud compute disks snapshot`.
- [ ] B. A Cloud Function triggered by Cloud Scheduler.
- [ ] C. Create a Snapshot Schedule policy in Compute Engine and attach it to the disk.
- [ ] D. Machine Images created manually every week.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `C`

**Why it's correct:**  
Native **Snapshot Schedules** automatically manage snapshot creation, schedule timing, and retention purging with zero external compute infrastructure.
</details>

---

### Question 42 [Private Google Access for Internal Subnets]
A private Compute Engine VM has no external IP address. It needs to read data from a Cloud Storage bucket without leaving Google's private network backbone. What setting must be enabled?
- [ ] A. Cloud NAT on the VPC
- [ ] B. Private Google Access on the VM's subnet
- [ ] C. Direct peering to Cloud Storage
- [ ] D. Internal Load Balancing

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
**Private Google Access** allows private RFC 1918 instances to reach public Google APIs (Cloud Storage, BigQuery) over Google's internal network without requiring public IPs.
</details>

---

### Question 43 [App Engine Traffic Splitting]
You deployed version 2 of an App Engine application and want to send 10% of incoming production traffic to version 2 while monitoring error rates before complete rollout. What should you configure?
- [ ] A. Cloud DNS weighted routing
- [ ] B. App Engine Traffic Splitting by IP address or cookie
- [ ] C. Cloud Load Balancing backend weights
- [ ] D. Two separate App Engine applications

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
App Engine features built-in **Traffic Splitting** to divide traffic across versions based on IP address or cookies for safe canary releases.
</details>

---

### Question 44 [Disaster Recovery Point-in-Time Recovery]
An administrator accidentally runs a data update script that corrupts tables in a Cloud SQL PostgreSQL instance at 11:42 AM. How can the database be restored to its exact state at 11:41 AM?
- [ ] A. Restore from the previous night's automated backup.
- [ ] B. Use Point-in-Time Recovery (PITR) and specify 11:41:00 AM.
- [ ] C. Revert the Persistent Disk snapshot.
- [ ] D. Run an undo query from Query Insights.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
**Point-in-Time Recovery (PITR)** uses transaction write-ahead logs to roll back database state to any specific second within the retention window.
</details>

---

### Question 45 [Storage Class Lifecycle Transition]
You store regulatory documents in Cloud Storage that must be retained for 7 years. Documents are accessed frequently during the first 30 days, rarely for the rest of year 1, and virtually never thereafter. How should lifecycle rules be structured?
- [ ] A. Standard Storage permanently.
- [ ] B. Standard -> Nearline at 30 days -> Archive at 365 days.
- [ ] C. Coldline from day 1.
- [ ] D. Standard -> Delete at 30 days.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
Standard avoids retrieval fees during initial access; Nearline minimizes cost during months 2–12; Archive provides lowest long-term storage fees for remaining years.
</details>

---

### Question 46 [Hard Spend Capping Architecture]
What architecture is required to automatically shut down virtual machines in a test project when spending exceeds $200 in a month?
- [ ] A. Set a Budget threshold of $200 in Cloud Billing.
- [ ] B. Set a Quota on Compute Engine CPUs.
- [ ] C. Cloud Budget publishes to Pub/Sub -> triggers Cloud Function -> disables billing on project.
- [ ] D. Organization Policy budget cap.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `C`

**Why it's correct:**  
Budgets only alert; automated hard spend capping requires connecting the Budget to **Pub/Sub** and triggering a **Cloud Function** to disable billing programmatically.
</details>

---

### Question 47 [Workload Identity vs Static Keys]
Why does Google Cloud recommend Workload Identity over Service Account JSON keys for GKE clusters?
- [ ] A. JSON keys are encrypted by default.
- [ ] B. Workload Identity provides short-lived tokens and eliminates static key exfiltration risks.
- [ ] C. Workload Identity is cheaper.
- [ ] D. JSON keys expire after 24 hours.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
Workload Identity uses short-lived Google-managed tokens, eliminating long-lived static private keys that can be accidentally checked into version control or stolen.
</details>

---

### Question 48 [GKE Cluster Autoscaler vs HPA]
What is the difference between Horizontal Pod Autoscaler (HPA) and Cluster Autoscaler in GKE?
- [ ] A. They are identical.
- [ ] B. HPA scales Pod replicas based on CPU/memory load; Cluster Autoscaler adds/removes worker node VMs when Pods cannot be scheduled.
- [ ] C. Cluster Autoscaler scales Pods; HPA scales nodes.
- [ ] D. HPA requires manual approval.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
HPA operates at the workload layer (adjusting replica count); Cluster Autoscaler operates at the infrastructure layer (adjusting node VM count).
</details>

---

### Question 49 [Cloud Load Balancing for Global Anycast]
Which load balancer terminates HTTP/HTTPS traffic at Google edge Points of Presence globally and routes requests to the nearest healthy backend region over Google's private network?
- [ ] A. Internal Application Load Balancer
- [ ] B. External Passthrough Network Load Balancer
- [ ] C. Global External Application Load Balancer
- [ ] D. Regional External Application Load Balancer

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `C`

**Why it's correct:**  
The **Global External Application Load Balancer** utilizes Google's global Anycast IP infrastructure to terminate connections at the nearest edge PoP worldwide.
</details>

---

### Question 50 [BigQuery Partitioning & Clustering]
How can an analytics engineer optimize BigQuery performance and cost for queries filtering by `transaction_date` and sorting by `customer_id`?
- [ ] A. Partition by `customer_id` and cluster by `transaction_date`.
- [ ] B. Partition table by `transaction_date` and cluster table by `customer_id`.
- [ ] C. Create individual tables per customer.
- [ ] D. Disable query caching.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**  
Partitioning by `transaction_date` limits the data slices scanned; clustering by `customer_id` colocates related records within partitions, maximizing query efficiency.
</details>

---

## Score Tracking Sheet

| Date | Score (/50) | Percentage | Target Met? (≥80%) | Weak Domains Identified |
| :--- | :---: | :---: | :---: | :--- |
| YYYY-MM-DD | -- | --% | [ ] Yes [ ] No | |
