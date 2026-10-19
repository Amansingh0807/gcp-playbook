# Google Cloud ACE — Top 100 Topic-Wise Master Question Bank

> **Source:** Official Google Cloud Certified Associate Cloud Engineer (ACE) Instructor Master Collection  
> **Structure:** 100 High-Yield Exam Questions Categorized Across the 6 Official Test Domains  
> **Interactive Format:** Click `<details>` on any question to view the correct answer, architectural rationale, and exam takeaways.

---

## Domain Distribution & Table of Contents

| Domain | Topic Area | Question Count | Range | Quick Link |
| :---: | :--- | :---: | :---: | :---: |
| **Domain 1** | Identity, Access & Governance (IAM & Security) | 18 Questions | Q1 – Q18 | [Jump to Domain 1 ->](#domain-1--identity-access--governance-iam--security) |
| **Domain 2** | Networking & Content Delivery (VPC & Connectivity) | 16 Questions | Q19 – Q34 | [Jump to Domain 2 ->](#domain-2--networking--content-delivery-vpc--connectivity) |
| **Domain 3** | Compute & Application Services (VMs, Containers & Serverless) | 24 Questions | Q35 – Q58 | [Jump to Domain 3 ->](#domain-3--compute--application-services-vms-containers--serverless) |
| **Domain 4** | Storage & Migration Services (GCS & Data Transfer) | 14 Questions | Q59 – Q72 | [Jump to Domain 4 ->](#domain-4--storage--migration-services-gcs--data-transfer) |
| **Domain 5** | Databases, Analytics & Messaging (Cloud SQL, BigQuery, Pub/Sub) | 18 Questions | Q73 – Q90 | [Jump to Domain 5 ->](#domain-5--databases-analytics--messaging-cloud-sql-bigquery-pubsub) |
| **Domain 6** | Operations, Billing & Management | 10 Questions | Q91 – Q100 | [Jump to Domain 6 ->](#domain-6--operations-billing--management) |

---

## Domain 1 — Identity, Access & Governance (IAM & Security)

### Question 1 of 100
The security team has locked out SSH access to production VMs. How can operations manage the VMs?
- A) Grant operations team appropriate IAM roles to manage VMs with gcloud commands
- B) Develop a Cloud API application for all operations actions
- C) Develop an application that grants temporary SSH access
- D) Configure a VPN to allow SSH access to VMs

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `A`

**Explanation:**  
When SSH port access is intentionally disabled or restricted, administrators can manage the lifecycle, metadata, disks, and runtime state of Compute Engine instances via the Google Cloud SDK (`gcloud compute instances ...`) or Cloud Console by holding appropriate IAM roles (such as `roles/compute.instanceAdmin.v1` or `roles/compute.operAdmin`).
</details>

---

### Question 2 of 100
You created a new project, but due to an organization policy restriction, no VPC was created. What are the effects of such a project without a VPC? (Choose TWO)
- A) You can still create all GCP resources as normal
- B) You can't create a GCE VM
- C) You cannot create a Cloud Function
- D) You cannot create a GCS bucket
- E) You cannot create a GKE cluster

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Options:** `B` and `E`

**Explanation:**  
Both Compute Engine virtual machines and Google Kubernetes Engine (GKE) clusters require an underlying Virtual Private Cloud (VPC) network and subnet to allocate internal IP addresses to network interfaces and node pools. Cloud Storage buckets and Cloud Functions (by default) do not require a project VPC network to be provisioned.
</details>

---

### Question 3 of 100
What are the options to connect to Linux-based GCE instances that have no external IP? (Choose THREE)
- A) Connect from your Organization node since it has privileges to all the other resources.
- B) Use IAP (Identity-Aware Proxy).
- C) Login via a bastion host.
- D) Directly connect with "ssh" command from Cloud Shell, since it has the visibility into all project instances.
- E) Use Cloud VPN with local visibility of GCP-based resources.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Options:** `B`, `C`, and `E`

**Explanation:**  
To access a private VM lacking an external IP:
1. **Identity-Aware Proxy (IAP) TCP forwarding:** Tunnels SSH traffic securely through Google's edge over port 22 without public IPs.
2. **Bastion Host (Jump Box):** Connect via an intermediate VM possessing an external IP situated in the same VPC.
3. **Cloud VPN / Cloud Interconnect:** Connect directly from an on-premises network using RFC 1918 private routing.
Cloud Shell cannot route directly to private internal IPs without an established network tunnel or IAP.
</details>

---

### Question 4 of 100
BizBank has a requirement to only use data services that ensure data encryption. Which services in GCP can they consider?
- A) GCS
- B) Cloud Spanner
- C) Bigtable
- D) Firestore
- E) Persistent Disks

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Options:** `A`, `B`, `C`, `D`, and `E` (All of the above)

**Explanation:**  
Google Cloud automatically encrypts all customer data at rest by default using 256-bit Advanced Encryption Standard (AES-256) across all storage and database services, including Cloud Storage, Cloud Spanner, Cloud Bigtable, Firestore, and Persistent Disks, with zero extra configuration required.
</details>

---

### Question 5 of 100
When creating a Persistent Disk, you chose a Customer-Supplied Encryption Key (CSEK) model to manage keys. Unfortunately, you stored the encryption key on a company laptop that was stolen. How can you recover the lost key needed to decrypt the data stored on the Persistent Disk?
- A) You need to reach out to Google support and initiate Key Recovery procedure.
- B) You don't have to; Google still stores DEK keys in GCP, which are enough to decrypt the data.
- C) You can generate another encryption key and use this one instead.
- D) Google can't help you recover the key, since it's not stored permanently by Google.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `D`

**Explanation:**  
Under the CSEK model, Google never stores your encryption key on disk. Google only holds the key transiently in memory to perform read/write operations. If you lose your CSEK key, Google cannot recover your data under any circumstances, and the data is permanently lost.
</details>

---

### Question 6 of 100
Business analysts in your team need to run analysis on data that was loaded into BigQuery. You need to follow recommended practices and grant permissions. What role should you grant the business analysts?
- A) `bigquery.resourceViewer` and `bigquery.dataViewer`
- B) `bigquery.user` and `bigquery.dataViewer`
- C) `bigquery.dataOwner`
- D) `storage.objectViewer` and `bigquery.user`

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `B`

**Explanation:**  
To analyze data in BigQuery:
- `roles/bigquery.dataViewer`: Provides read access to inspect datasets, tables, and views.
- `roles/bigquery.user`: Provides permission to run query jobs, create query jobs, and allocate query slots within the project.
</details>

---

### Question 7 of 100
In order to fulfill a compliance requirement, you defined a retention policy on one of your GCS buckets and executed the `gsutil retention lock gs://BUCKET_NAME` command. You now need to modify some objects from the bucket that did not expire yet and you get an error. How do you resolve it?
- A) You need to manually remove the lock from the bucket and update the policy so that the object can be modified.
- B) You need to contact GCP support to remove the lock for you.
- C) You can't remove or modify the objects until they fulfill the expiration policy.
- D) You need to use `--force` parameter when executing the `gsutil rm` command.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `C`

**Explanation:**  
Once a Cloud Storage bucket retention policy is **locked**, it is completely irreversible and permanent. Even Google Cloud Support cannot remove, alter, or reduce the retention period or delete unexpired objects until their retention duration has elapsed.
</details>

---

### Question 8 of 100
You have data stored in a Cloud Storage bucket. You are using both Identity and Access Management (IAM) and Access Control Lists (ACLs) to configure access control. Which statement describes a user's access to objects in the bucket?
- A) The user has no access if IAM denies the permission.
- B) The user only has access if both IAM and ACLs grant a permission.
- C) The user has access if either IAM or ACLs grant a permission.
- D) The user has no access if either IAM or ACLs deny a permission.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `C`

**Explanation:**  
IAM and ACLs operate in a **union** model: access is granted if **either** IAM or an ACL grants the permission. Google strongly recommends enabling **Uniform Bucket-Level Access** to disable legacy ACLs and manage permissions exclusively via IAM.
</details>

---

### Question 9 of 100
A manager at Cymbal Retail expresses concern about unauthorized access to objects in your Cloud Storage bucket. You need to evaluate all access on all objects in the bucket. What should you do?
- A) Review the Admin Activity audit logs.
- B) Enable and then review the Data Access audit logs.
- C) Route the Admin Activity logs to a BigQuery sink and analyze the logs with SQL queries.
- D) Change the permissions on the bucket to only trusted employees.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `B`

**Explanation:**  
Admin Activity audit logs only track metadata operations (such as creating buckets or modifying IAM policies). To audit who read, wrote, or downloaded actual objects (data access), you must explicitly enable and inspect **Data Access Audit Logs** for Cloud Storage (which are disabled by default due to high volume).
</details>

---

### Question 10 of 100
Cymbal Retail has accumulated a large amount of data. Analysts and leadership are finding it difficult to understand the meaning of the data, such as BigQuery columns. Users of the data don't know who owns what. You need to improve the searchability of the data. What should you do?
- A) Create tags for data entries in Dataplex / Data Catalog.
- B) Rename BigQuery columns with more descriptive names.
- C) Export the data to Cloud Storage with descriptive file names.
- D) Add a description column corresponding to each data column.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `A`

**Explanation:**  
**Data Catalog (Dataplex Catalog)** is Google Cloud's centralized metadata management service. It allows organizations to quickly discover, tag, classify, and search data assets across BigQuery, Cloud Storage, and Dataproc using standardized business metadata tag templates.
</details>

---

### Question 11 of 100
Your organization needs to ensure the observability of data processes, monitor planned usage, and troubleshoot error messages and billing issues effectively. Which Google Cloud service provides centralized logging and monitoring capabilities for data processes?
- A) Google Cloud Monitoring
- B) Google Cloud Logging
- C) Google Cloud Trace
- D) Google Cloud Audit Logging

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `A`

**Explanation:**  
**Google Cloud Monitoring** (part of Google Cloud Operations Suite) collects time-series performance metrics, uptime statuses, and system dashboards across data pipelines, allowing administrators to configure alerting policies and track operational health.
</details>

---

### Question 12 of 100
Your data in BigQuery has some columns that are extremely sensitive. You need to enable only some users to see certain columns. What should you do?
- A) Create a new dataset with the column's data.
- B) Create a new table with the column's data.
- C) Use policy tags.
- D) Use Identity and Access Management (IAM) permissions.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `C`

**Explanation:**  
BigQuery supports column-level security using **Policy Tags** created in Dataplex Data Catalog. By associating policy tags with specific table columns and granting the `Fine-Grained Reader` role only to authorized identities, sensitive columns are masked or restricted without duplicating tables.
</details>

---

### Question 13 of 100
You have data pipelines running on BigQuery, Cloud Dataflow, and Cloud Dataproc. You need to perform health checks and monitor their behavior, and then notify the team managing the pipelines if they fail. You also need to be able to work across multiple projects. Your preference is to use managed products or features of the platform. What should you do?
- A) Export the information to Cloud Monitoring, and set up an Alerting policy
- B) Run a Virtual Machine in Compute Engine with Airflow, and export the information to Stackdriver
- C) Export the logs to BigQuery, and set up App Engine to read that information and send emails if you find a failure in the logs
- D) Develop an App Engine application to consume logs using GCP API calls, and send emails if you find a failure in the logs

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `A`

**Explanation:**  
Google Cloud Monitoring natively integrates with BigQuery, Dataflow, and Dataproc across multi-project metrics scopes. Setting up an **Alerting Policy** tied to notification channels (email, Slack, PagerDuty) provides a fully managed, serverless monitoring solution.
</details>

---

### Question 14 of 100
Your organization allows people to create online professional profiles. A key feature is the ability to create clusters of people who are professionally connected to one another. What type of machine learning method is used to create these clusters?
- A) Unsupervised machine learning
- B) Binary classification
- C) Supervised machine learning
- D) Reinforcement learning

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `A`

**Explanation:**  
Clustering algorithms group unlabelled data points based on inherent statistical similarities and feature distances without pre-existing training labels, which is the definition of **unsupervised machine learning**.
</details>

---

### Question 15 of 100
You work for a multinational organization that has recently begun operations in Spain. Teams within your organization will need to work with various Spanish documents. You want to use machine learning to get accurate translations quickly and with the least effort. Your organization does not require domain-specific terms or jargon. What should you do?
- A) Create a Vertex AI Workbench notebook instance. In the notebook, extract sentences from the documents, and train a custom AutoML text model.
- B) Use Google Cloud Translation API (pre-trained).
- C) Train a custom translation model from scratch using TensorFlow.
- D) Use Google Translate to translate 1,000 phrases from Spanish to English, and train an AutoML model.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `B` *(Pre-trained Translation API / Cloud Translation)*

**Explanation:**  
When no specialized industry jargon or proprietary domain vocabulary is required, using Google's pre-trained **Cloud Translation API** delivers instant, highly accurate translation with minimum development effort. Custom AutoML models are only necessary for domain-specific terminology.
</details>

---

### Question 16 of 100
You are training an ML model using data stored in BigQuery that contains several values that are considered Personally Identifiable Information (PII). You need to reduce the sensitivity of the dataset before training your model. Every column is critical to your model. How should you proceed?
- A) Using Dataflow, ingest the columns with sensitive data from BigQuery, and then randomize the values in each sensitive column.
- B) Use the Cloud Data Loss Prevention (DLP) API to scan for sensitive data, and use Dataflow with the DLP API to encrypt sensitive values with Format Preserving Encryption.
- C) Use the Cloud Data Loss Prevention (DLP) API to scan for sensitive data, and use Dataflow to replace all sensitive data by using the encryption algorithm AES-256 with a salt.
- D) Before training, use BigQuery to select only the columns that do not contain sensitive data.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `B`

**Explanation:**  
**Format-Preserving Encryption (FPE)** via the Cloud Data Loss Prevention (Sensitive Data Protection) API encrypts PII data while maintaining the original string format, length, and statistical properties, allowing machine learning models to train effectively without exposing raw identifiable data.
</details>

---

### Question 17 of 100
You developed a Vertex AI ML pipeline that consists of preprocessing and training steps where each set of steps runs on a separate custom Docker image. Your organization uses GitHub Actions as CI/CD. You need to automate the model retraining workflow so that it can be initiated both manually and when a new version of the code is merged in the main branch. You want to minimize steps while allowing maximum flexibility. How should you configure the CI/CD workflow?
- A) Trigger a Cloud Build workflow to run tests, build custom Docker images, push the images to Artifact Registry, and launch the pipeline in Vertex AI Pipelines.
- B) Trigger GitHub Actions to run the tests, launch a job on Cloud Run to build custom Docker images, push the images to Artifact Registry, and launch the pipeline in Vertex AI Pipelines.
- C) Trigger GitHub Actions to run the tests, build custom Docker images, push the images to Artifact Registry, and launch the pipeline in Vertex AI Pipelines.
- D) Trigger GitHub Actions to run the tests, launch a Cloud Build workflow to build custom Docker images, push the images to Artifact Registry, and launch the pipeline in Vertex AI Pipelines.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `D`

**Explanation:**  
Leveraging GitHub Actions to execute CI unit tests and delegating container image compilation to **Cloud Build** offloads heavy compilation to Google's serverless build infrastructure. Cloud Build pushes the images to Artifact Registry and kicks off the Vertex AI Pipeline.
</details>

---

### Question 18 of 100
You work for an organization that operates a streaming music service. Your custom recommendation model is deployed on a Vertex AI endpoint. You recently retrained the model with fresh data and offline test results were positive. You now want to test the new model in production while minimizing complexity. What should you do?
- A) Create a new Vertex AI endpoint and route 5% of traffic using a custom load balancer.
- B) Capture incoming prediction requests in BigQuery and run batch predictions side-by-side.
- C) Deploy the new model to the existing Vertex AI endpoint and use traffic splitting to send 5% of production traffic to the new model.
- D) Configure a model monitoring job for drift detection and replace the model immediately.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `C`

**Explanation:**  
Vertex AI Endpoints natively support **traffic splitting** across multiple deployed model versions on the same endpoint URL. Routing 5% of live traffic to the canary model allows SREs to monitor user metrics in production with zero code changes on client applications.
</details>

---

## Domain 2 — Networking & Content Delivery (VPC & Connectivity)

### Question 19 of 100
What are examples of global GCP resources? (Choose TWO)
- A) Global Persistent Disks
- B) Subnets
- C) Some Load Balancers (External Application Load Balancer)
- D) Cloud SQL
- E) Global GCS Buckets
- F) VPC Networks
- G) Anthos GKE Clusters

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Options:** `C` and `F`

**Explanation:**  
In Google Cloud, **VPC Networks** and **External Application Load Balancers** (Global HTTP/S) are global resources. Subnets are regional; VMs and Persistent Disks are zonal.
</details>

---

### Question 20 of 100
What are examples of regional GCP resources? (Choose THREE)
- A) GCE Instances
- B) Static External IP Addresses (Regional)
- C) All Persistent Disks
- D) Regional Persistent Disks
- E) Regional Managed Instance Groups (MIGs)
- F) Zonal GKE clusters

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Options:** `B`, `D`, and `E`

**Explanation:**  
- Regional Persistent Disks synchronously replicate data across two zones in a region.
- Regional MIGs distribute VM instances across multiple zones in a region.
- Regional Static IP addresses are bound to specific regions.
Individual Compute Engine VMs and Zonal GKE clusters are zonal resources.
</details>

---

### Question 21 of 100
How can application components, owned by different project teams, communicate over RFC 1918 private IP addresses with centralized network administration?
- A) Parts communicate using HTTPS over public IPs
- B) Communicate over global load balancers, one per project
- C) Single project, same VPC
- D) Shared VPC, with each team's project configured as a Service Project of the Host VPC

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `D`

**Explanation:**  
**Shared VPC** allows an organization to connect resources from multiple service projects to a common, centrally administered Host VPC network, enabling private RFC 1918 communication across project boundaries with centralized security governance.
</details>

---

### Question 22 of 100
A customer provisioned a Cloud SQL instance with private IP in Project A, but cannot connect to it from Project B, even though VPC Peering is configured between the VPCs in Project A and Project B. What is the reason?
- A) The connection must use instance name instead of IP address.
- B) Appropriate Cloud SQL roles are missing.
- C) VPC Network Peering is non-transitive; traffic cannot hop through Project A into the Google-managed Cloud SQL VPC.
- D) Cloud SQL needs to be registered in Cloud Console.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `C`

**Explanation:**  
**VPC Network Peering is strictly non-transitive.** When Cloud SQL is configured with Private IP, Google creates a private services access peering connection between Project A's VPC and Google's internal Cloud SQL tenant VPC. Traffic from Project B's VPC cannot traverse Project A to reach the peered Cloud SQL network.
</details>

---

### Question 23 of 100
You are training a neural network model with TensorFlow on large datasets. Your mentor suggests data normalization. Which of the following is NOT a data normalization or feature scaling technique?
- A) Scaling to a range (Min-Max scaling)
- B) Feature Clipping
- C) Z-test
- D) Log scaling
- E) Z-score normalization

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `C`

**Explanation:**  
A **Z-test** is an inferential statistical hypothesis test used to determine whether two population means are different, not a data normalization or feature transformation technique.
</details>

---

### Question 24 of 100
The learning rate is a configurable hyperparameter used in the training of neural networks that has a small positive value, typically in the range between:
- A) < 0.0 and > 1.00
- B) 10.0 and 100.00
- C) 0.0 and 1.0
- D) 1.0 and 3.0

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `C`

**Explanation:**  
In gradient descent optimization, the learning rate determines the step size taken towards a minimum at each iteration and is typically set between `0.0` and `1.0` (commonly `0.001` to `0.1`).
</details>

---

### Question 25 of 100
You are working on tuning learning rates, number of hidden layers, and nodes per layer to optimize convergence speed. In machine learning terminology, what is this process called?
- A) Cross Validation
- B) Regularization
- C) Hyperparameter tuning
- D) Drift detection management

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `C`

**Explanation:**  
**Hyperparameter tuning** is the automated process of selecting the optimal configuration of external model parameters (such as learning rate, batch size, and network depth) that govern the learning process.
</details>

---

### Question 26 of 100
You need to automatically identify optimal values for learning rate, number of layers, and kernel size by running multiple parallel trials with minimal custom code and zero infrastructure management. What should you use?
- A) Train an AutoML image classification model.
- B) Create a custom training job that uses the Vertex AI Vizier SDK.
- C) Create a Vertex AI hyperparameter tuning job.
- D) Create a Vertex AI pipeline that runs different model training jobs in parallel.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `C`

**Explanation:**  
A **Vertex AI hyperparameter tuning job** runs multiple trials of your training application in parallel, searching through user-defined parameter ranges using Bayesian optimization algorithms with zero infrastructure overhead.
</details>

---

### Question 27 of 100
You have a Linear Regression model with many driving features. You want to synthesize and compress the features to reduce dimensionality without losing significant information. Which technique should you use?
- A) Feature Crosses
- B) Principal Component Analysis (PCA)
- C) Embeddings
- D) Functional Data Analysis

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `B`

**Explanation:**  
**Principal Component Analysis (PCA)** is an unsupervised dimensionality reduction technique that transforms correlated features into a smaller set of orthogonal, uncorrelated variables (principal components) that preserve maximum variance.
</details>

---

### Question 28 of 100
You recently trained an XGBoost model for online inference. You need to perform a simple data preprocessing step that accepts requests within VPC Service Controls with minimal cost and effort. What should you do?
- A) Store a pickled model in Cloud Storage and deploy a Flask app on Vertex AI Endpoints.
- B) Build a Flask-based app packaging the pickled model in a custom container image.
- C) Build a custom predictor class based on XGBoost Predictor from the Vertex AI SDK, and package it with the model.
- D) Build a custom predictor class based on XGBoost Predictor from the Vertex AI SDK, package the handler in a container image, store the pickled model in Cloud Storage, and deploy to Vertex AI Endpoints.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `D`

**Explanation:**  
Vertex AI supports custom prediction routines (CPR) using the Vertex AI SDK's `Predictor` interface. Packaging only the lightweight handler logic into a container based on Vertex AI pre-built images and referencing the model artifact in Cloud Storage maximizes efficiency and security.
</details>

---

### Question 29 of 100
You are deploying a credit risk neural network model on Vertex AI. Regulatory compliance mandates feature attribution explanations and continuous monitoring for feature distribution drift over time. What should you configure?
- A) Use Vertex Explainable AI with the sampled Shapley method, and enable Vertex AI Model Monitoring to check for feature distribution drift.
- B) Use Vertex Explainable AI with the sampled Shapley method, and enable Vertex AI Model Monitoring to check for feature distribution skew.
- C) Use Vertex Explainable AI with the XRAI method, and enable Vertex AI Model Monitoring to check for feature distribution drift.
- D) Use Vertex Explainable AI with the XRAI method, and enable Vertex AI Model Monitoring to check for feature distribution skew.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `A`

**Explanation:**  
The **Sampled Shapley** method in Vertex Explainable AI provides cooperative game-theory feature attribution for tabular neural network models, and **Vertex AI Model Monitoring** detects feature distribution drift between incoming production requests and baseline training distributions.
</details>

---

### Question 30 of 100
Which machine learning technique synthesizes and compresses tabular features without losing essential information?
- A) Feature Crosses
- B) Principal Component Analysis (PCA)
- C) Embeddings
- D) Functional Data Analysis

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `B`

**Explanation:**  
Principal Component Analysis (PCA) is the standard statistical technique for linear dimensionality reduction and feature synthesis.
</details>

---

### Question 31 of 100
You need to define and deploy a set of GCP resources (a VM, a firewall rule, and a static IP) as a single repeatable unit using a Google-native Infrastructure as Code (IaC) tool. What service should you use?
- A) Terraform
- B) Cloud Run
- C) Cloud Deployment Manager
- D) Ansible

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `C`

**Explanation:**  
**Cloud Deployment Manager** is Google Cloud's native, declarative Infrastructure as Code service that uses YAML or Python/Jinja2 templates to manage GCP resources as unified deployment stacks. (Terraform is multi-cloud open-source).
</details>

---

### Question 32 of 100
You are designing a custom-mode VPC for a 3-tier application: Web tier in `us-central1`, App tier in `us-central1` and `us-east1`, and Database tier in `us-central1` without internet access. All tiers must communicate with each other. Which statements are TRUE? (Select THREE)
- A) The VPC's dynamic routing mode must be 'Regional'.
- B) The VPC's dynamic routing mode must be 'Global'.
- C) You must create a subnet for the database tier and disable Private Google Access.
- D) You must create a Cloud NAT gateway for the database subnet.
- E) You must create firewall rules to allow traffic between the tiers' subnets (e.g., using network tags).
- F) You must create separate VPCs for each region.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Options:** `B`, `C`, and `E`

**Explanation:**  
1. **Global Dynamic Routing:** Required for subnets across multiple regions (`us-central1` and `us-east1`) to communicate dynamically over Cloud Routers.
2. **Private Subnet:** The database subnet should have no internet route and no external IPs.
3. **VPC Firewall Rules:** By default, all ingress is denied; firewall rules referencing target tags must explicitly allow inter-tier traffic.
</details>

---

### Question 33 of 100
You are provisioning a new GKE cluster. The security team requires that nodes and pods are not reachable directly from the public internet. What configurations should you select? (Select TWO)
- A) Create a Private Cluster.
- B) Configure Authorized Networks and leave the master endpoint public.
- C) Create a GKE Autopilot cluster, as they are private by default.
- D) Disable "Enable node auto-upgrades" in the node pool.
- E) Use a Shared VPC and place the GKE cluster in a private subnet.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Options:** `A` and `B`

**Explanation:**  
Creating a **Private Cluster** ensures worker nodes receive only internal RFC 1918 IP addresses. Enabling **Master Authorized Networks** restricts control plane API access strictly to designated trusted CIDR ranges while allowing administrators to connect.
</details>

---

### Question 34 of 100
Users report that a web application running on a Compute Engine instance is unreachable. You check and the VM is in a `RUNNING` state. What are valid troubleshooting steps? (Select THREE)
- A) Check the VPC firewall rules to ensure ingress traffic is allowed on TCP:80 or TCP:443.
- B) Check if the VM has a valid external IP address (or is behind a working Load Balancer).
- C) Check the VM's serial port output for boot or application errors.
- D) Run `gcloud compute instances stop` and `start` to migrate the VM.
- E) Check the project's billing account to see if it's suspended.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Options:** `A`, `B`, and `C`

**Explanation:**  
Standard network and VM troubleshooting involves:
1. Verifying VPC ingress firewall rules permit TCP traffic on ports 80/443.
2. Confirming network reachability via external IP or healthy load balancer backend.
3. Inspecting guest OS serial console output (`serial-port-output`) to verify web server services (e.g., NGINX/Apache) started successfully.
</details>

---

## Domain 3 — Compute & Application Services (VMs, Containers & Serverless)

### Question 35 of 100
What are your options to automatically deploy Compute Engine instances that already contain specific software and configuration? (Choose THREE)
- A) Create a custom OS image with all the software installed and deploy a GCE instance using this image.
- B) Deploy a VM with a public OS image, SSH into it, and trigger the configuration manually from a script.
- C) Deploy a VM with a public OS image and use startup scripts to install/configure the software.
- D) Deploy a pre-configured solution from Google Cloud Marketplace.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Options:** `A`, `C`, and `D`

**Explanation:**  
Automated provisioning options include:
- **Custom golden images:** Fast boot times with pre-compiled software.
- **Startup scripts:** Dynamic initialization on first boot via instance metadata.
- **Cloud Marketplace:** Turnkey pre-packaged image solutions.
Manual SSH (B) is not automated.
</details>

---

### Question 36 of 100
How can an architecture avoid dropped incoming data during workload spikes without increasing the number of Compute Engine worker VMs?
- A) Add memory to the VMs
- B) Write incoming data to Cloud Pub/Sub and have VMs process records from the subscription queue
- C) Write to Cloud Memorystore and have VMs read from cache
- D) Attach Local SSDs to the VMs

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `B`

**Explanation:**  
**Cloud Pub/Sub** provides asynchronous message buffering that absorbs massive ingestion spikes, decoupling ingestion from worker processing and preventing data loss when worker compute capacity is temporarily saturated.
</details>

---

### Question 37 of 100
A cron job checks a Cloud Storage bucket every 10 minutes for new uploaded files. How can you improve response time and reduce polling overhead?
- A) Trigger a Cloud Function (or Cloud Run function) on Cloud Storage object finalize events
- B) Deploy a Kubernetes pod with an infinite loop
- C) Run the cron job every minute
- D) Use an App Engine Flexible application

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `A`

**Explanation:**  
Cloud Storage supports native event notifications via Eventarc / Pub/Sub that trigger **Cloud Functions** instantly when an object is created (`google.storage.object.finalize`), replacing inefficient polling with real-time event-driven execution.
</details>

---

### Question 38 of 100
Which of the following is an advantage of App Engine Standard over App Engine Flexible?
- A) Allows ephemeral disk
- B) Supports custom third-party binaries and Dockerfiles
- C) Automatically scales down to zero instances when idle
- D) Provides root SSH access to instances

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `C`

**Explanation:**  
**App Engine Standard** runs within pre-configured sandboxed language runtimes that can start instances in milliseconds and **scale down to zero** when traffic ceases, minimizing costs. App Engine Flexible runs on Docker VMs with a minimum of 1 active instance.
</details>

---

### Question 39 of 100
What is the most cost-effective way to run non-critical, fault-tolerant Apache Spark batch jobs on Cloud Dataproc?
- A) Set up a cluster in high availability mode with high-memory machine types and 10 local SSDs.
- B) Set up a cluster in high availability mode with default machine types and 10 preemptible worker nodes.
- C) Set up a cluster in standard mode with high-memory master nodes and preemptible (Spot) worker nodes.
- D) Set up a cluster in standard mode with default machine types and 10 local SSDs.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `C`

**Explanation:**  
Dataproc allows worker nodes to run on **Preemptible / Spot VMs**. For non-critical Spark batch jobs, preemptible workers provide massive cost reductions, while standard primary master nodes maintain job coordination.
</details>

---

### Question 40 of 100
What are examples of regional GCP resources? (Choose TWO)
- A) GCE Instances
- B) IP Addresses (Global)
- C) All Persistent Disks
- D) Regional Persistent Disks
- E) Regional Managed Instance Groups (MIGs)
- F) Zonal GKE clusters

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Options:** `D` and `E`

**Explanation:**  
Regional Persistent Disks and Regional MIGs are defined at the regional level and span multiple availability zones within that region. Single GCE instances and Zonal GKE clusters are zonal.
</details>

---

### Question 41 of 100
You want to optimize Compute Engine costs by purchasing a 3-year commitment. However, you anticipate that your workload requirements will change and you may need to resize or change the number of VMs. What should you do?
- A) Buy a 1-year commitment and accept the loss when VMs are resized.
- B) Buy a 3-year commitment with an early opt-out clause.
- C) Purchase a resource-based or flexible spend-based Committed Use Discount (CUD), which applies discounts across varying instance sizes and counts within the same family/region.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `C`

**Explanation:**  
Committed Use Discounts (CUDs) apply to aggregate vCPU and RAM commitments within an instance family and region. You are not locked into a specific instance ID; you can change instance sizes and counts freely as long as aggregate usage matches the commitment.
</details>

---

### Question 42 of 100
You would like to deploy a Compute Engine instance with an attached GPU in your local region, but the Cloud Console indicates GPUs are not available. How do you deploy this VM?
- A) Deploy it using `gcloud compute instances create` via CLI.
- B) Contact GCP support to enable GPUs in the project.
- C) Select another zone or region, as specific GPU hardware models are only physically installed in select data centers.
- D) Purchase a commitment first to unlock GPU hardware.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `C`

**Explanation:**  
Specific GPU models (e.g., NVIDIA H100, A100, T4) are physically deployed only in designated zones and regions globally. If a GPU model is unavailable in a zone, you must select an alternate zone/region that hosts that hardware.
</details>

---

### Question 43 of 100
As part of your disaster recovery plan, you want to be able to restore Compute Engine instances using Persistent Disk snapshots with the fewest steps possible. How should you proceed?
- A) Export the snapshots to Cloud Storage, create disks from the files, and create images.
- B) Export the snapshots to Cloud Storage and create images.
- C) Use the snapshots to create replacement disks, and use the disks to create instances as needed.
- D) Create replacement instances directly from snapshots using the Cloud Console or `gcloud compute instances create --source-snapshot`.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `D`

**Explanation:**  
Compute Engine allows creating a replacement instance directly from a snapshot in a single step using `gcloud compute instances create [VM_NAME] --source-snapshot=[SNAPSHOT_NAME]` or the Cloud Console.
</details>

---

### Question 44 of 100
How would you design a latency-sensitive Compute Engine application that ensures business continuity even if an entire availability zone suffers an outage?
- A) Deploy in the same zone using multiple VMs behind an internal load balancer.
- B) Deploy multiple VMs distributed across separate availability zones in the same region behind a regional load balancer.
- C) Deploy multiple VMs in separate regions.
- D) Deploy a single VM and schedule snapshots to restore in another zone.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `B`

**Explanation:**  
Deploying across multiple zones in the **same region** provides zone-level fault tolerance while keeping inter-instance network latency extremely low (< 1–2 ms), satisfying both low-latency requirements and disaster survivability.
</details>

---

### Question 45 of 100
A Linux Compute Engine instance fails to boot properly. What actions can you take to diagnose and validate the errors? (Choose TWO)
- A) Enable Serial Console connectivity and inspect the boot log.
- B) Resize the VM to a larger machine type.
- C) Connect via Identity-Aware Proxy (IAP) instead of SSH.
- D) Detach the boot disk from the failing VM, attach it as a secondary non-boot disk to a healthy VM, and inspect the filesystem logs.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Options:** `A` and `D`

**Explanation:**  
When an OS kernel panics or fails before SSH starts:
1. Viewing the **interactive Serial Console** output captures kernel boot errors.
2. Detaching the corrupted boot disk and mounting it onto a secondary rescue VM allows inspecting `/var/log/syslog` and fixing filesystem configurations.
</details>

---

### Question 46 of 100
How can an organization optimize Compute Engine infrastructure spending? (Choose FIVE)
- A) Purchase Committed Use Discounts (CUDs) for predictable steady workloads.
- B) Automate scheduled shutdowns and startups for development instances during off-hours.
- C) Review and apply Cloud Recommender VM right-sizing suggestions.
- D) Limit the number of licensed users who log into VMs.
- E) Use Preemptible / Spot VMs for batch, fault-tolerant tasks.
- F) Deploy non-latency-sensitive workloads in lower-cost regions.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Options:** `A`, `B`, `C`, `E`, and `F`

**Explanation:**  
Core FinOps optimization levers include CUDs, automated schedules, Recommender right-sizing, Spot VMs, and deploying in cost-effective regions (e.g., `us-central1` vs. higher-cost regions).
</details>

---

### Question 47 of 100
YourHealth company has strict compliance regulations that prohibit sharing physical server hardware with other cloud tenants. How can they run Compute Engine virtual machines?
- A) They must use on-premises bare metal servers only.
- B) Deploy on Sole-Tenant Nodes.
- C) Deploy custom machine types of the maximum possible CPU size.
- D) Query GCP hypervisor metadata IDs.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `B`

**Explanation:**  
**Sole-Tenant Nodes** provide dedicated physical Compute Engine servers reserved exclusively for a single customer organization, satisfying strict hardware isolation, compliance, and licensing (BYOL) mandates.
</details>

---

### Question 48 of 100
How should you arrange a low-cost, one-time migration of a 100 TB on-premises file server to Google Cloud where data will be frequently accessed from a single region?
- A) Transfer Appliance to a Cloud Storage Standard bucket.
- B) Transfer Appliance to a Cloud Storage Nearline bucket.
- C) Storage Transfer Service agent to a Cloud Storage Standard bucket.
- D) Storage Transfer Service agent to a Cloud Storage Coldline bucket.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `C`

**Explanation:**  
**Storage Transfer Service (on-premises)** uses lightweight software agents installed in your data center to transfer large datasets (100 TB) over existing network connections into a **Standard Storage** bucket (since data will be frequently accessed). Transfer Appliance is reserved for bandwidth-constrained sites > 100–200 TB.
</details>

---

### Question 49 of 100
You need to run massive scientific batch jobs that take several days to complete. You do not want to manage or provision the underlying virtual machine clusters. What should you use?
- A) Cloud Scheduler
- B) Cloud Workflows
- C) Cloud Batch
- D) Cloud Run

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `C`

**Explanation:**  
**Cloud Batch** is Google Cloud's fully managed batch service that automatically provisions compute resources, schedules tasks across Spot/standard instances, executes batch processing containers, and tears down infrastructure when jobs complete.
</details>

---

### Question 50 of 100
A weekly 30-minute Apache Spark job on a 15-node Dataproc cluster reads data from Cloud Storage and writes results to BigQuery. How should you optimize the cluster for cost?
- A) Migrate the workload to Google Cloud Dataflow.
- B) Use Preemptible / Spot VMs for the secondary worker nodes in the cluster.
- C) Use higher-memory master nodes.
- D) Attach SSD Persistent Disks to all worker nodes.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `B`

**Explanation:**  
Because Spark jobs on Dataproc tolerate worker node preemption, configuring cluster worker nodes as **Preemptible / Spot VMs** slashes compute costs by up to 80% with minimal job completion impact.
</details>

---

### Question 51 of 100
A company has 280 TB of data stored on on-premises HDFS clusters and needs to migrate the data to Cloud Storage securely over a limited, low-bandwidth internet connection. What should you use?
- A) Install `gsutil` on servers and copy data over the public internet.
- B) Use Cloud Storage Transfer Service over the WAN.
- C) Export data to BigQuery first.
- D) Use Google Cloud Transfer Appliance.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `D`

**Explanation:**  
When migrating hundreds of terabytes (> 100 TB) over **low-bandwidth network connections**, physical shipping of encrypted hardware via **Google Cloud Transfer Appliance** is significantly faster, more reliable, and more economical than internet transfers.
</details>

---

### Question 52 of 100
An online education platform offers 2,500 video courses in French, German, Turkish, and Thai, and needs to automatically generate captions across all courses at scale. Which Google Cloud service should they use?
- A) Cloud Speech-to-Text API
- B) Cloud Natural Language API
- C) Vertex AI Machine Learning Engine
- D) AutoML Vision API

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `A`

**Explanation:**  
**Cloud Speech-to-Text API** converts spoken audio into written text across 125+ languages and variants, providing automated punctuation and timestamped speech transcription for video captioning.
</details>

---

### Question 53 of 100
You need to migrate an on-premises Redis database to Memorystore for Redis with minimal cost, time, and effort following Google-recommended practices. What should you do?
- A) Export an RDB snapshot backup of the Redis database, upload the RDB file to a Cloud Storage bucket, and import the RDB file into Memorystore for Redis.
- B) Deploy a secondary Redis instance on Compute Engine and perform manual cutover.
- C) Create an Apache Beam Dataflow job to stream Redis keys.
- D) Write custom shell scripts to iterate through all keys.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `A`

**Explanation:**  
Memorystore for Redis natively supports the **Import / Export** feature using standard Redis Database (RDB) snapshot files stored in a Cloud Storage bucket.
</details>

---

### Question 54 of 100
A startup serves web traffic in Asia and plans to scale globally after securing funding. Current priority is cost; future goal is global consistency with a native JDBC driver. What is the recommended database roadmap?
- A) Deploy a single-region Cloud Spanner instance initially, and expand to multi-region Cloud Spanner after funding.
- B) Use Cloud SQL for PostgreSQL HA first, then Bigtable.
- C) Use Cloud SQL for PostgreSQL zonal first, then Bigtable.
- D) Use Cloud SQL zonal first, then Cloud SQL HA.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `A`

**Explanation:**  
**Cloud Spanner** provides standard ANSI SQL, relational ACID compliance, and native JDBC/ODBC connectivity. Starting with a single-region Spanner instance satisfies regional requirements without requiring architectural rewrites when expanding to a multi-region Spanner configuration later.
</details>

---

### Question 55 of 100
Which Looker feature allows business teams to automatically send recurring analytical reports to stakeholders via email or webhook on a configured schedule?
- A) Looker Scheduling
- B) Looker Alert
- C) Looker Dataflows
- D) Looker Dashboards

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `A`

**Explanation:**  
**Looker Scheduling** enables automated recurring distribution of Looks and Dashboards to email recipients, Amazon S3, SFTP servers, or webhooks.
</details>

---

### Question 56 of 100
In production, an ML model trained on enterprise tabular data is delivering unsatisfactory accuracy because raw columns do not capture complex business patterns. What should you do?
- A) Train the model with more of the same raw data.
- B) Apply L2 regularization.
- C) Perform feature engineering, combining and transforming columns using domain knowledge.
- D) Increase training epochs.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `C`

**Explanation:**  
**Feature engineering** transforms raw data into more informative signals (e.g., feature crosses, ratios, bucketization) using domain expertise, which is the most impactful method to improve model accuracy.
</details>

---

### Question 57 of 100
Your enterprise has collected historical industry datasets that external business partners want to query and purchase. You need to grant secure, governed data access without exporting raw copies. What should you use?
- A) Export data to zip files in Cloud Storage.
- B) Publish data exchanges using BigQuery Analytics Hub.
- C) Export data to Persistent Disks over FTP.
- D) Host the data on Cloud SQL with public IPs.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `B`

**Explanation:**  
**Analytics Hub** (built on BigQuery) allows organizations to securely publish and share curated data assets, tables, and views with external partners with built-in access governance and zero data duplication.
</details>

---

### Question 58 of 100
Analysts need to query, model, and visualize data originating from diverse relational, NoSQL, and analytical stores through a unified semantic business data layer. What tool should be recommended?
- A) Looker Studio
- B) Connected Sheets
- C) D3.js
- D) Looker (LookML semantic model)

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `D`

**Explanation:**  
**Looker** provides an enterprise semantic modeling layer (**LookML**) that abstracts and standardizes business logic across disparate underlying databases, ensuring centralized metrics definitions.
</details>

---

## Domain 4 — Storage & Migration Services (GCS & Data Transfer)

### Question 59 of 100
How can you minimize the storage cost of security video recordings in Cloud Storage that are accessed repeatedly during the first 30 days after recording, but rarely accessed thereafter?
- A) Store in Nearline Storage immediately, then move to Coldline after 30 days.
- B) Store in Standard Storage, then delete after 30 days.
- C) Store in Standard Storage, then use Object Lifecycle Management to transition to Coldline or Archive Storage after 30 days.
- D) Keep the files in Standard Storage permanently.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `C`

**Explanation:**  
Standard Storage avoids retrieval fees during the high-frequency first 30 days. An automated **Object Lifecycle Management** policy transitions objects to Coldline or Archive after 30 days to minimize ongoing monthly capacity charges.
</details>

---

### Question 60 of 100
A customer wants to establish automated daily backups of multi-TB databases hosted in another public cloud into Google Cloud, including conducting monthly disaster recovery restoration drills. What should be used?
- A) Transfer Appliance to Cloud Storage Nearline.
- B) Transfer Appliance to Cloud Storage Coldline.
- C) Storage Transfer Service to a Cloud Storage Nearline bucket.
- D) Storage Transfer Service to a Cloud Storage Coldline bucket.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `C`

**Explanation:**  
**Storage Transfer Service** automates scheduled cloud-to-cloud data synchronization. **Nearline Storage** is ideal for data accessed approximately once per month (such as monthly DR drills) with a 30-day minimum storage commitment.
</details>

---

### Question 61 of 100
Your data engineering team receives daily JSON files from external partners and needs to design a secure, automated data ingestion architecture into a data warehouse. What should you do?
- A) Ingest files into Cloud Storage, and execute an ETL/ELT pipeline (Dataflow or BigQuery load jobs) into BigQuery.
- B) Expose your BigQuery dataset with public write permissions.
- C) Create a public API writing directly to disk.
- D) Store files on Persistent Disks attached to VMs.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `A`

**Explanation:**  
Landing raw files into an isolated **Cloud Storage** staging bucket and executing automated Cloud Dataflow or native BigQuery batch load jobs is the standard, secure data engineering architecture.
</details>

---

### Question 62 of 100
You need to archive business financial records for multi-year compliance, accessing the records only to generate quarterly compliance audit reports. What Cloud Storage class is optimal?
- A) Standard Storage
- B) Nearline Storage
- C) Coldline Storage
- D) Archive Storage

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `C`

**Explanation:**  
**Coldline Storage** is specifically designed for data accessed at most once per quarter (90 days) with significantly lower monthly storage rates than Standard and Nearline.
</details>

---

### Question 63 of 100
You have large volumes of unprocessed and processed data residing in Cloud Storage and BigQuery organized under a Dataplex data mesh. How should you structure data discovery?
- A) Create a lake for Cloud Storage and a zone for BigQuery.
- B) Create a lake for BigQuery and a zone for Cloud Storage.
- C) Create a lake for unprocessed data and assets for processed data.
- D) Create a Raw Data Zone for unprocessed data and a Curated Data Zone for refined data within Dataplex.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `D`

**Explanation:**  
Dataplex structures data lakes into logical **Zones**: **Raw Zones** store unprocessed landing assets (Cloud Storage buckets), and **Curated Zones** store cleansed, validated, and structured datasets (BigQuery tables).
</details>

---

### Question 64 of 100
A retail company stores 10 years of historical sales data in Cloud Storage in Avro format. Data scientists want to rapidly train an ML sales forecasting time-series model with minimum code. What should they do?
- A) Load data into BigQuery and train an `ARIMA_PLUS` model using BigQuery ML.
- B) Convert data to CSV and train a regression model on AutoML Tables.
- C) Convert data to TFRecords and train an RNN on custom TensorFlow notebooks.
- D) Refactor data into CSV and train XGBoost on custom containers.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `A`

**Explanation:**  
**BigQuery ML** supports native time-series forecasting via the `ARIMA_PLUS` algorithm directly in SQL. It automatically handles anomaly detection, holiday effects, and hyperparameter tuning with minimal effort.
</details>

---

### Question 65 of 100
You have a BigQuery transaction dataset containing categorical columns (product category, payment method). You plan to train a model in BigQuery ML and export it for online prediction as fast as possible. What should you do?
- A) Use the `TRANSFORM` clause with `ML.ONE_HOT_ENCODER` during model creation.
- B) Manually preprocess and encode columns into a new intermediate table.
- C) Omit feature encoding and use default settings.
- D) Use `ML.MULTI_HOT_ENCODER` on all categorical columns.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `A`

**Explanation:**  
Using the `TRANSFORM` clause with `ML.ONE_HOT_ENCODER` embeds the categorical preprocessing transformations directly inside the exported model artifact, ensuring identical transformations during online inference.
</details>

---

### Question 66 of 100
A gaming platform stores terabytes of user gameplay history in Cloud Storage and wants to build a collaborative filtering game recommendation model with the least custom coding. What should they do?
- A) Load data into BigQuery and train an Autoencoder model.
- B) Load data into BigQuery and train a Matrix Factorization model using BigQuery ML.
- C) Build a custom TensorFlow recommendation model on Compute Engine.
- D) Use Cloud Functions to compute user similarity matrices.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `B`

**Explanation:**  
BigQuery ML provides a built-in **Matrix Factorization** algorithm (`CREATE MODEL ... OPTIONS(model_type='matrix_factorization')`) for building collaborative filtering recommendation engines using simple SQL queries.
</details>

---

### Question 67 of 100
Data scientists need to query BigQuery and Cloud Storage datasets for exploratory data analysis (EDA) and run Python ML experiments while minimizing infrastructure management costs. What environment should you provision?
- A) User-managed Vertex AI Workbench notebook with default VM.
- B) Managed Vertex AI Workbench notebook to query tables directly via the integrated JupyterLab interface.
- C) User-managed notebook on Dataproc Hub.
- D) Managed notebook connected to a persistent 10-node Dataproc cluster.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `B`

**Explanation:**  
**Managed Vertex AI Workbench notebooks** provide a Google-managed infrastructure environment with built-in integrations to browse, query, and visualize BigQuery and Cloud Storage assets directly from JupyterLab with automated idle shutdown.
</details>

---

### Question 68 of 100
You are training a deep learning image classification model in Vertex AI on 1,000,000 images stored in Cloud Storage. You need to minimize network I/O bottlenecks during training. What should you do?
- A) Mount the bucket using Cloud Storage FUSE.
- B) Use `tf.data.Dataset.list_files` directly on individual image URIs.
- C) Pre-convert images into binary TFRecord files stored in Cloud Storage and stream them using `tf.data.TFRecordDataset`.
- D) Store URLs in a CSV file and read them via `CsvDataset`.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `C`

**Explanation:**  
Converting individual small files into **TFRecord** files (combining multiple images into sequential binary records of 100–200 MB each) optimizes sequential I/O reads and eliminates per-file HTTP connection overhead in TensorFlow.
</details>

---

### Question 69 of 100
You want to parameterize an Apache Spark data transformation step within an automated Kubeflow / Vertex AI Pipeline. What should you configure?
- A) Remove data transformation from the pipeline.
- B) Run Spark transformations directly on the Kubeflow master pod.
- C) Add a pipeline component that dynamically launches an ephemeral Cloud Dataproc cluster, runs the PySpark transformation, saves output to Cloud Storage, and tears down the cluster.
- D) Deploy Apache Spark on a separate GKE node pool permanently.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `C`

**Explanation:**  
Google Cloud pipeline best practice uses pre-built Google Cloud Pipeline Components (GCPC) to orchestrate **ephemeral Cloud Dataproc clusters** that spin up, execute PySpark jobs, write output to GCS, and immediately terminate to save compute costs.
</details>

---

### Question 70 of 100
A two-step Vertex AI Pipeline runs a 1-hour preprocessing step on 10 TB of data, followed by model training. You need to iterate on training algorithm parameters without waiting for data preprocessing to re-run every time. What should you do?
- A) Add pipeline parameters to manually skip steps.
- B) Hardcode preprocessed paths into a new pipeline.
- C) Use larger compute-optimized VMs for preprocessing.
- D) Enable component caching for the preprocessing step, and disable caching for the training step.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `D`

**Explanation:**  
Vertex AI Pipelines supports **execution caching**. If the inputs and code of the preprocessing component have not changed, the pipeline reuses the cached output from the previous run, saving 1 hour of runtime and substantial compute costs.
</details>

---

### Question 71 of 100
You want to deploy an image classification model into an iOS mobile application with offline support and lowest possible prediction latency. What should you configure?
- A) Train on Vertex AI AutoML and invoke batch predictions.
- B) Train with AutoML Vision Edge, export the model as a Core ML (`.mlmodel`) package, and bundle it directly into the iOS application.
- C) Export as TFLite and build an Android service.
- D) Deploy to a cloud Vertex AI Endpoint and call it over mobile cellular networks.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `B`

**Explanation:**  
**AutoML Vision Edge** allows training lightweight models optimized for mobile devices and exporting them directly into **Core ML** format for native, low-latency offline execution on Apple iOS hardware.
</details>

---

### Question 72 of 100
Your company wants to grant data scientists access to query aggregated datasets in BigQuery while strictly prohibiting them from reading the underlying raw source data files stored in Cloud Storage. How should you configure access?
- A) Grant `roles/bigquery.dataViewer` and deny `roles/storage.objectViewer` at project level.
- B) Grant `roles/bigquery.user` on the project and `roles/bigquery.dataViewer` on specific datasets. Do not grant Storage permissions.
- C) Grant `roles/bigquery.jobUser` and `roles/storage.objectViewer`.
- D) Create a BigQuery Authorized View and grant data scientists access to the view only.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `D`

**Explanation:**  
An **Authorized View** in BigQuery allows sharing query results with specific users without granting them read permissions on the underlying source tables or underlying storage files.
</details>

---

## Domain 5 — Databases, Analytics & Messaging (Cloud SQL, BigQuery, Pub/Sub)

### Question 73 of 100
Which Google Cloud services work together to ingest, process, and analyze real-time delivery truck GPS location streams? (Choose THREE)
- A) Cloud Pub/Sub
- B) Cloud Dataflow
- C) Cloud Dataproc
- D) BigQuery / Bigtable for analytics

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Options:** `A`, `B`, and `D`

**Explanation:**  
The canonical Google Cloud streaming architecture is:
1. **Cloud Pub/Sub:** Ingests high-throughput telemetry streams.
2. **Cloud Dataflow (Apache Beam):** Processes, transforms, and window-aggregates streaming records.
3. **BigQuery / Bigtable:** Stores and serves real-time analytics.
</details>

---

### Question 74 of 100
A company needs a scalable NoSQL storage solution for ingesting massive volumes of time-series telemetry from millions of IoT sensors with sub-10ms write latency. Which service should you recommend?
- A) Cloud Storage
- B) Cloud Spanner
- C) Cloud Bigtable
- D) Cloud Memorystore

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `C`

**Explanation:**  
**Cloud Bigtable** is Google Cloud's high-throughput, low-latency NoSQL wide-column database optimized for large-scale analytical and operational time-series workloads (> 1 TB to petabytes).
</details>

---

### Question 75 of 100
Which Google Cloud service provides fully managed Apache Hadoop and Apache Spark clusters?
- A) Cloud Pub/Sub
- B) BigQuery
- C) Cloud Dataproc
- D) Cloud Dataflow

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `C`

**Explanation:**  
**Cloud Dataproc** is a managed service for deploying, running, and autoscaling open-source big data frameworks like Apache Hadoop, Spark, Hive, and Pig.
</details>

---

### Question 76 of 100
What three Google Cloud services are most commonly combined in standard modern streaming data pipelines?
- A) Dataproc, Cloud SQL, BigQuery
- B) Pub/Sub, Dataflow, BigQuery
- C) Pub/Sub, GKE, Cloud Spanner
- D) Cloud Bigtable, Dataproc, Cloud Spanner

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `B`

**Explanation:**  
**Pub/Sub -> Dataflow -> BigQuery** is Google Cloud's core reference architecture for streaming ingest, transformation, and analytical data warehousing.
</details>

---

### Question 77 of 100
Business analysts who lack programming skills need to clean, deduplicate, and transform large input data files through an interactive visual interface in an automated pipeline. What tool should they use?
- A) Cloud Dataprep by Trifacta
- B) Cloud Dataproc
- C) Cloud Dataflow
- D) Google Sheets

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `A`

**Explanation:**  
**Cloud Dataprep** is an intelligent, visual data preparation tool that allows non-programmers to explore, clean, and prepare structured and unstructured data for analysis without writing code.
</details>

---

### Question 78 of 100
Which practices help optimize BigQuery query performance and reduce scan costs? (Select THREE)
- A) Avoid using `SELECT *`; select only necessary columns.
- B) In SQL joins, place the largest table first (on the left) of the join.
- C) Filter early using partitioned and clustered columns in the `WHERE` clause.
- D) Avoid `COUNT(DISTINCT)` on massive datasets when approximate metrics (`APPROX_COUNT_DISTINCT`) are acceptable.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Options:** `A`, `B`, and `C`

**Explanation:**  
BigQuery best practices include:
1. Column projection pruning (avoid `SELECT *` on columnar storage).
2. Placing the largest table on the left of `JOIN` clauses to optimize distributed join distribution.
3. Filtering early on partitioned date/time columns to minimize bytes scanned.
</details>

---

### Question 79 of 100
You are deploying 10,000 IoT sensors collecting temperature readings globally. You must ingest, process, and analyze this continuous stream in real time. What architecture should you deploy?
- A) Send data to Cloud Datastore and export to BigQuery.
- B) Ingest via Cloud Pub/Sub, stream through Cloud Dataflow, and write into BigQuery.
- C) Upload to Cloud Storage and spin up on-demand Dataproc clusters.
- D) Batch logs to Cloud Storage, import to Cloud SQL, and analyze.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `B`

**Explanation:**  
**Pub/Sub -> Dataflow -> BigQuery** handles massive concurrent streaming writes with automatic autoscaling, windowing, and immediate SQL analytical availability.
</details>

---

### Question 80 of 100
An existing MapReduce ETL pipeline running on Hadoop requires a newly discovered sensor calibration step to be applied systematically to raw data before any computationally expensive transformations occur. How should the pipeline be modified?
- A) Modify existing transform jobs.
- B) Insert a dedicated calibration MapReduce job that processes raw input data first, chaining downstream jobs after it.
- C) Require end users to calibrate outputs manually.
- D) Apply mathematical variance predictions post-hoc.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `B`

**Explanation:**  
In modular data engineering pipelines, prepending a dedicated calibration job at the head of the DAG ensures raw data is corrected once before downstream transformations execute.
</details>

---

### Question 81 of 100
Which built-in Cloud SQL feature helps database administrators detect query bottlenecks, identify slow SQL statements, and analyze database wait events?
- A) Cloud SQL Query Insights
- B) Automatic Index Tuning
- C) Native Dataproc federation
- D) Query cache optimizer

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `A`

**Explanation:**  
**Cloud SQL Query Insights** provides visual dashboards and diagnostics to detect slow queries, inspect query execution plans, and identify wait event bottlenecks for PostgreSQL and MySQL instances.
</details>

---

### Question 82 of 100
How does Cloud Spanner achieve unlimited horizontal write scalability while maintaining strict ACID transactional consistency?
- A) By sharding data across multiple Cloud SQL instances.
- B) By using BigQuery partition slots.
- C) By sharding data into "splits" distributed across nodes in a regional or multi-regional cluster using TrueTime synchronization.
- D) By replicating data to Cloud Storage buckets.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `C`

**Explanation:**  
Cloud Spanner transparently partitions data into **splits** distributed across multiple compute nodes, synchronizing distributed multi-region transactions using Google's **TrueTime** atomic clocks and GPS infrastructure.
</details>

---

### Question 83 of 100
You need a managed relational database to support an enterprise transactional system where customers reside in a single geographical region. You want minimal administrative overhead. What should you choose?
- A) Cloud Spanner
- B) Cloud SQL
- C) Self-managed PostgreSQL on Compute Engine
- D) Cloud Storage regional bucket

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `B`

**Explanation:**  
**Cloud SQL** is Google Cloud's fully managed relational database service for MySQL, PostgreSQL, and SQL Server in single-region deployments, offering automated backups, replication, patching, and failover with lower cost than Cloud Spanner.
</details>

---

### Question 84 of 100
You are designing an analytical data warehouse in BigQuery from normalized transactional database sources. How should you structure schemas for optimal query performance?
- A) Retain the exact normalized relational schema with foreign key constraints.
- B) Create a single flat table using outer joins.
- C) Normalize all tables to Third Normal Form (3NF).
- D) Denormalize schemas by utilizing nested and repeated fields (`STRUCT` and `ARRAY`).

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `D`

**Explanation:**  
BigQuery performs best with **denormalized** data structures. Using **nested and repeated fields** (`RECORD`/`STRUCT` and `ARRAY`) preserves parent-child relationships within single rows, avoiding expensive relational table `JOIN` operations across columnar storage.
</details>

---

### Question 85 of 100
You are ingesting millions of daily event records into BigQuery and need to optimize query performance and reduce query scan costs for date-range queries. What should you configure?
- A) Ingestion-time partitioned table with daily partitioning.
- B) Ingestion-time partitioned table with yearly partitioning.
- C) Integer-range partitioned table.
- D) Time-unit column-partitioned table with yearly partitioning.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `A`

**Explanation:**  
**Ingestion-time daily partitioning** automatically routes rows into individual partition segments corresponding to the date of arrival, allowing queries filtering by `_PARTITIONDATE` to scan only matching days.
</details>

---

### Question 86 of 100
Analysts repeatedly run complex, multi-table queries that join and filter large datasets in BigQuery where data updates frequently. How can you simplify their workflow?
- A) Create a static dataset for frequent queries.
- B) Create a BigQuery logical View defining the query.
- C) Export data to a new static table hourly.
- D) Export data to Cloud SQL.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `B`

**Explanation:**  
A **BigQuery View** is a virtual table defined by a SQL query. Creating a view encapsulates complex join and filter logic, allowing analysts to query the view directly while ensuring they always see the latest underlying data.
</details>

---

### Question 87 of 100
You are building an executive Data Studio / Looker Studio dashboard that queries aggregated BigQuery tables with thousands of concurrent users. You need sub-second visualization latency. What should you configure?
- A) BigQuery BI Engine with Materialized Views.
- B) BigQuery BI Engine with logical views.
- C) Streaming buffer tables.
- D) BigQuery Authorized Views.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `A`

**Explanation:**  
**BigQuery BI Engine** is a fast in-memory analytical service. Pairing BI Engine with **Materialized Views** pre-computes aggregations and caches results in memory, delivering sub-second query response times for concurrent dashboards.
</details>

---

### Question 88 of 100
A financial institution operating across North America requires an enterprise transactional database with multi-region high availability, strict ACID compliance, and SQL query access. Which solution should be deployed?
- A) Cloud Spanner with stale reads.
- B) Cloud Spanner with read-write transactions.
- C) BigQuery with disabled cache.
- D) Cloud SQL with federated queries.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `B`

**Explanation:**  
**Cloud Spanner** using locking read-write transactions provides external global consistency, strict ACID transactions, and horizontal scalability across multi-region configurations.
</details>

---

### Question 89 of 100
A mid-sized enterprise is migrating a 20 TB transactional relational database from on-premises to Google Cloud within a single region. Which database service is appropriate?
- A) Cloud SQL
- B) Cloud Bigtable
- C) Cloud Spanner
- D) Cloud Datastore

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `A`

**Explanation:**  
Cloud SQL supports database storage capacities up to **64 TB** per instance for MySQL, PostgreSQL, and SQL Server, making it the cost-effective and operationally simple choice for a 20 TB single-region relational workload.
</details>

---

### Question 90 of 100
An I/O-intensive Apache Spark transformation pipeline processes CSV files and outputs curated data for analysts who query using standard ANSI SQL. What architecture should be selected?
- A) Cloud Storage for storage; Cloud Dataflow for transformations.
- B) BigQuery for storage; Cloud Dataproc for transformations.
- C) Cloud Storage for storage; Cloud Dataproc for transformations.
- D) Cloud Bigtable for storage; Cloud Dataproc for transformations.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `B`

**Explanation:**  
Running custom Apache Spark jobs on **Cloud Dataproc** performs the heavy transformation logic, and writing output directly into **BigQuery** enables analysts to execute interactive ANSI SQL queries.
</details>

---

## Domain 6 — Operations, Billing & Management

### Question 91 of 100
You configured Dataplex to automatically discover and catalog files across Cloud Storage lakes and zones, but noticed that certain data files are missing from catalog discovery. What is the most likely cause?
- A) An exclude regex pattern is configured on the zone matching those file paths.
- B) Discovery is scheduled hourly.
- C) The files are in ORC format.
- D) The files are in Parquet format.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `A`

**Explanation:**  
Dataplex discovery jobs support **Exclude Patterns** (glob or regex paths). If an exclude pattern matches the directory or file prefix, Dataplex skips indexing those files during discovery.
</details>

---

### Question 92 of 100
In what chronological order are the lifecycle phases of a production machine learning project executed?
1. Selection of ML algorithm
2. Data Exploration
3. Definition of the business use case
4. Model monitoring
5. Model operationalization
6. Model Development

- A) I, II, III, IV, V, VI
- B) III, II, I, VI, V, IV
- C) II, III, I, VI, IV, V
- D) II, I, III, VI, IV, V

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `B`

**Explanation:**  
The standard MLOps lifecycle proceeds:
1. **Business Use Case Definition** (III)
2. **Data Exploration & Preparation** (II)
3. **Algorithm Selection** (I)
4. **Model Development & Training** (VI)
5. **Model Operationalization & Deployment** (V)
6. **Model Monitoring & Maintenance** (IV)
</details>

---

### Question 93 of 100
You need to deploy a scikit-learn model to a Vertex AI Endpoint serving 24/7 with peak traffic surges of millions of requests between 8 AM and 7 PM. You must minimize costs during off-peak hours. What should you configure?
- A) Set max replica count to 1.
- B) Configure an auto-scaling Vertex AI prediction endpoint with minimum replica count of 1 and maximum replica count of 100.
- C) Attach 1 GPU per replica with max replica count of 1.
- D) Attach 1 GPU per replica with max replica count of 100.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `B`

**Explanation:**  
Configuring an auto-scaling CPU prediction endpoint with autoscaling replicas (e.g., min 1, max 100) automatically scales compute capacity out during daytime traffic peaks and scales in to 1 replica overnight to minimize costs. Scikit-learn CPU inference does not require expensive GPUs.
</details>

---

### Question 94 of 100
You are deploying an XGBoost model to Vertex AI Model Registry for online prediction. You need feature attribution explanations returned with minimal latency and want alerts if feature attributions change significantly over time. What should you configure?
- A) Sampled Shapley explanation with path count of 5; deploy to Vertex AI Endpoints; configure Model Monitoring for prediction drift.
- B) Integrated Gradients with path count of 5; deploy to Endpoints; configure Model Monitoring for prediction drift.
- C) Sampled Shapley with path count of 50; configure training-serving skew.
- D) Integrated Gradients with path count of 50; configure training-serving skew.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `A`

**Explanation:**  
- **Sampled Shapley** is the appropriate attribution method for tree-based models like XGBoost (Integrated Gradients is for differentiable neural networks). Setting a lower path count (e.g., 5) reduces computational latency.
- **Prediction drift monitoring** alerts on attribution distribution changes over time in production.
</details>

---

### Question 95 of 100
You developed a sales forecasting model on Vertex AI. You anticipate feature distribution shifts and expect high query volumes. You want to configure Vertex AI Model Monitoring while minimizing operational monitoring costs. What should you do?
- A) Monitor features with a higher monitoring frequency.
- B) Set prediction sampling rate close to 1.0 (100%).
- C) Monitor features and attributions with lower frequency.
- D) Monitor features and feature attributions, but set a `prediction-sampling-rate` closer to 0 than 1 (e.g., 0.05 or 5% sampling).

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `D`

**Explanation:**  
In high-throughput prediction environments, logging and analyzing 100% of prediction traffic is expensive. Configuring a low **sampling rate** (e.g., 5% to 10% sampling) provides statistically sound drift detection at a fraction of the logging and compute cost.
</details>

---

### Question 96 of 100
You are retraining regression models using Vertex AI Pipelines. You want an automated email alert whenever production feature data distribution shifts significantly so engineers can trigger retraining. What should you configure?
- A) Enable Vertex AI Model Monitoring with prediction drift on the endpoint and specify notification emails.
- B) Create a logs-based alert in Cloud Logging.
- C) Create a threshold alert in Cloud Monitoring.
- D) Export container logs to BigQuery and query with Cloud Scheduler.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `A`

**Explanation:**  
**Vertex AI Model Monitoring** provides native, automated feature drift detection against baseline training statistics and can send email notifications directly when statistical distance exceeds defined drift thresholds.
</details>

---

### Question 97 of 100
A high-traffic Vertex AI endpoint with Model Monitoring enabled is generating higher monitoring charges than anticipated. You need to reduce monitoring costs while continuing to detect drift. What parameter should you adjust?
- A) Replace the monitoring job with Dataflow TFDV.
- B) Calculate manual statistics via BigQuery.
- C) Decrease the `sample_rate` parameter in the `RandomSampleConfig` of the monitoring job.
- D) Increase the monitoring interval schedule.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `C`

**Explanation:**  
Decreasing the `sample_rate` (e.g., from 0.5 to 0.1) reduces the percentage of logged prediction requests processed by the monitoring pipeline, directly lowering Cloud Storage logging volume and analytical computing expenses.
</details>

---

### Question 98 of 100
What is the primary architectural and business trade-off when selecting a larger foundation model (e.g., with hundreds of billions of parameters)?
- A) It is faster and cheaper, but less accurate.
- B) It is slower and more expensive to run (higher inference cost and latency), but generally more capable, versatile, and accurate.
- C) It only supports text data.
- D) It has a smaller context window.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Option:** `B`

**Explanation:**  
Larger foundation models require more GPU/TPU memory and floating-point operations per token, increasing inference latency and operational cost, while delivering superior reasoning, nuanced comprehension, and task generalization.
</details>

---

### Question 99 of 100
What are recognized fundamental limitations of Large Language Models (LLMs) that cloud architects and business leaders must account for? (Select FOUR)
- A) They can "hallucinate" and generate plausible but factually incorrect outputs.
- B) They can reflect biases present in their training corpora.
- C) Their parametric knowledge is "frozen" at their training cutoff date unless augmented with RAG.
- D) High-throughput inference can be computationally expensive.
- E) They cannot process programming code.
- F) They are physically restricted from processing text.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Options:** `A`, `B`, `C`, and `D`

**Explanation:**  
Hallucinations, training bias, frozen cutoff dates, and inference infrastructure costs are standard LLM limitations. Modern LLMs are proficient in code generation and multimodal processing.
</details>

---

### Question 100 of 100
When evaluating a new generative AI enterprise initiative, which business metrics are most critical to define upfront? (Select THREE)
- A) The total parameter count of the model.
- B) Cost savings (e.g., reduction in customer service agent handling time).
- C) The programming language the model was compiled in.
- D) Revenue growth (e.g., increased conversion rates from personalized copy).
- E) Customer satisfaction (CSAT) score improvement.
- F) The size of the raw training dataset.

<details>
<summary><b>[Yes] Answer & Detailed Explanation</b></summary>

**Correct Options:** `B`, `D`, and `E`

**Explanation:**  
Business value in generative AI is measured through practical business outcomes: **operational cost savings**, **revenue generation**, and **customer satisfaction (CSAT)**, rather than vanity engineering metrics like parameter count.
</details>

---

## Score Tracking & Performance Analysis

| Domain | Total | Your Score | Mastery Benchmark (≥85%) | Status |
| :--- | :---: | :---: | :---: | :---: |
| **1. Identity, Access & Governance** | 18 | /18 | 15/18 | [ ] Pass [ ] Review |
| **2. Networking & Content Delivery** | 16 | /16 | 14/16 | [ ] Pass [ ] Review |
| **3. Compute & Application Services** | 24 | /24 | 20/24 | [ ] Pass [ ] Review |
| **4. Storage & Migration Services** | 14 | /14 | 12/14 | [ ] Pass [ ] Review |
| **5. Databases, Analytics & Messaging** | 18 | /18 | 15/18 | [ ] Pass [ ] Review |
| **6. Operations, Billing & Management** | 10 | /10 | 8/10 | [ ] Pass [ ] Review |
| **TOTAL** | **100** | **/100** | **85/100** | Target Achieved |
