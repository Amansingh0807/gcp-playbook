# Phase 09 Practice Drill - DevOps & Automation

> **Total Questions:** 15 (10 MCQs + 5 MSQs)  
> **Topic Tag:** `#phase-09-devops-automation`  
> **Target Score:** 13/15 (86%+)  
> **Covers:** Cloud Build (triggers, `cloudbuild.yaml`, build steps), Artifact Registry (Docker images, package repos, security scanning), CI/CD pipelines (CI vs. Delivery vs. Deployment), Infrastructure as Code (Terraform fundamentals, remote state in GCS), Gemini Cloud Assist.

---

## MCQ Section (Single Correct Answer)

---

### Q1. What is the fundamental operational difference between Continuous Delivery and Continuous Deployment?

- A) Continuous Delivery only applies to virtual machines, whereas Continuous Deployment applies to containers.
- B) Continuous Delivery requires a manual approval step before releasing to production, whereas Continuous Deployment is fully automated.
- C) Continuous Delivery builds code automatically, whereas Continuous Deployment requires developers to manually trigger builds.
- D) Continuous Delivery is open source, whereas Continuous Deployment is proprietary to Google Cloud.

<details>
<summary>[Yes] Answer</summary>

**B) Continuous Delivery requires a manual approval step before releasing to production, whereas Continuous Deployment is fully automated.**

**Explanation:** Continuous Delivery automates the entire software build, test, and staging deployment pipeline, but deliberately retains a manual human verification or approval gate prior to promoting changes into live production. Continuous Deployment eliminates this manual gate, automatically shipping all code changes that pass automated test suites directly to production users.
</details>

---

### Q2. A software enterprise needs a centralized, managed artifact repository on Google Cloud capable of storing Docker container images, npm packages, and Python PyPI dependencies with granular IAM permissions. Which service should they select?

- A) Google Container Registry (`gcr.io`)
- B) Cloud Storage Standard Bucket
- C) Artifact Registry
- D) Compute Engine Custom Images

<details>
<summary>[Yes] Answer</summary>

**C) Artifact Registry**

**Explanation:** Artifact Registry is Google Cloud's modern, universal artifact repository service. It supports Docker and OCI container images alongside language packages (npm, Maven, Gradle, Python PyPI, NuGet) and OS packages (Apt, Yum) with granular repository-level IAM access controls. Container Registry (A) is deprecated and only supported container images.
</details>

---

### Q3. An organization wants to provision, update, and manage Google Cloud resources declaratively using version-controlled code that works across multiple cloud providers. Which technology best fulfills this requirement?

- A) Google Cloud Deployment Manager
- B) HashiCorp Terraform
- C) Google Cloud Shell Scripts
- D) Google Cloud Build triggers

<details>
<summary>[Yes] Answer</summary>

**B) HashiCorp Terraform**

**Explanation:** Terraform is an open-source, declarative Infrastructure as Code (IaC) tool created by HashiCorp that supports multiple cloud providers (Google Cloud, AWS, Azure). Deployment Manager (A) is Google-specific and considered legacy.
</details>

---

### Q4. Why is it an essential security and operational best practice to store the `terraform.tfstate` file in a Google Cloud Storage bucket with object versioning and state locking?

- A) To automatically convert HCL declarative configurations into Python scripts.
- B) To eliminate the need for Google Cloud IAM permissions.
- C) To provide a centralized source of truth, prevent race conditions during concurrent executions, and preserve recovery history.
- D) To allow Terraform to run offline without an active network connection.

<details>
<summary>[Yes] Answer</summary>

**C) To provide a centralized source of truth, prevent race conditions during concurrent executions, and preserve recovery history.**

**Explanation:** In a collaborative team setting, storing Terraform's state file in a remote Google Cloud Storage bucket ensures all engineers operate on the exact same infrastructure snapshot. Cloud Storage backends provide state locking (preventing simultaneous conflicting executions) and object versioning (enabling recovery if state data becomes corrupted).
</details>

---

### Q5. A Cloud Build trigger runs a pipeline that builds a container image and executes `gcloud run deploy` to update a Cloud Run service. The build fails during the deploy step with an IAM permission error stating the caller cannot act as the runtime service account. Which IAM role is missing?

- A) `roles/viewer` on the project
- B) `roles/storage.admin` on the build bucket
- C) `roles/iam.serviceAccountUser` on the Cloud Run runtime service account
- D) `roles/compute.networkAdmin` on the VPC

<details>
<summary>[Yes] Answer</summary>

**C) `roles/iam.serviceAccountUser` on the Cloud Run runtime service account**

**Explanation:** Whenever a service or deployment engine (such as the Cloud Build service account) assigns an identity to a managed compute service like Cloud Run or Compute Engine, the deploying identity must hold the Service Account User role (`roles/iam.serviceAccountUser`) on the target service account.
</details>

---

### Q6. Which repository URL represents the correct standard naming format for pulling a container image from Google Artifact Registry?

- A) `gcr.io/my-project/my-image:v1`
- B) `docker.io/my-project/my-image:v1`
- C) `asia-south1-docker.pkg.dev/my-project/my-repo/my-image:v1`
- D) `storage.googleapis.com/my-project/my-repo/my-image:v1`

<details>
<summary>[Yes] Answer</summary>

**C) `asia-south1-docker.pkg.dev/my-project/my-repo/my-image:v1`**

**Explanation:** Artifact Registry container image paths adhere to the format: `[REGION]-docker.pkg.dev/[PROJECT_ID]/[REPOSITORY]/[IMAGE]:[TAG]`. The `gcr.io` path (A) belongs to deprecated Container Registry, and `docker.io` (B) is Docker Hub.
</details>

---

### Q7. What is the primary configuration file used to define the sequential or parallel containerized execution steps for Google Cloud Build?

- A) `Dockerfile`
- B) `cloudbuild.yaml`
- C) `terraform.tfstate`
- D) `pom.xml`

<details>
<summary>[Yes] Answer</summary>

**B) `cloudbuild.yaml`**

**Explanation:** Google Cloud Build uses `cloudbuild.yaml` (or `cloudbuild.json`) to define the sequence of execution steps, container builders, environment variables, arguments, and published artifact targets.
</details>

---

### Q8. A security compliance policy mandates that container images must be scanned for known security vulnerabilities prior to deployment in production GKE clusters. Where is this automated vulnerability scanning performed natively on Google Cloud?

- A) Cloud Logging Log Explorer
- B) Cloud Source Repositories
- C) Artifact Registry via Container Analysis
- D) Compute Engine Persistent Disks

<details>
<summary>[Yes] Answer</summary>

**C) Artifact Registry via Container Analysis**

**Explanation:** Artifact Registry natively integrates with Google Container Analysis to automatically scan uploaded container images for known Common Vulnerabilities and Exposures (CVEs) and display severity metrics directly within the console.
</details>

---

### Q9. How does Google Cloud calculate charges for Cloud Build pipeline execution?

- A) Flat monthly fee per registered Git repository.
- B) Billed by the total lines of code compiled during the pipeline.
- C) Billed per active build-minute based on the machine type, with 120 free build-minutes per day.
- D) Billed per gigabyte of RAM allocated to the source code repository.

<details>
<summary>[Yes] Answer</summary>

**C) Billed per active build-minute based on the machine type, with 120 free build-minutes per day.**

**Explanation:** Cloud Build is completely serverless and bills strictly based on the build-minutes consumed by the worker VM during execution. Every Google Cloud billing account receives 120 free build-minutes per day for standard virtual machine types.
</details>

---

### Q10. What is the role of Gemini Cloud Assist within the Google Cloud Console?

- A) An automated billing payment gateway that approves credit card charges.
- B) An integrated AI assistant providing natural language guidance, CLI syntax generation, and troubleshooting insights.
- C) A replacement build daemon that compiles C++ code on local developer machines.
- D) A specialized physical server installed inside customer data centers.

<details>
<summary>[Yes] Answer</summary>

**B) An integrated AI assistant providing natural language guidance, CLI syntax generation, and troubleshooting insights.**

**Explanation:** Gemini Cloud Assist is Google Cloud's conversational AI assistant built into the Cloud Console. It helps engineers troubleshoot errors, generate `gcloud` and Terraform snippets, and optimize architecture patterns using conversational natural language.
</details>

---

## MSQ Section (Multiple Select Questions)

---

### Q11. Which package and artifact formats are natively supported by Google Artifact Registry? (Select THREE)

- A) Docker / OCI container images
- B) npm (Node.js) packages
- C) Maven / Gradle (Java) artifacts
- D) VMware VMDK virtual machine disks
- E) Raw tape drive backup formats

<details>
<summary>[Yes] Answer</summary>

**A) Docker / OCI container images, B) npm (Node.js) packages, C) Maven / Gradle (Java) artifacts**

**Explanation:** Artifact Registry is a multi-format repository supporting Docker/OCI, npm, Maven/Gradle, Python (PyPI), NuGet (.NET), and OS packages (Apt and Yum). Virtual disk formats and tape drive formats are not artifact repository packages.
</details>

---

### Q12. Which statements regarding Google Cloud Build's architecture and capabilities are TRUE? (Select TWO)

- A) Every single build step runs inside an isolated, containerized environment.
- B) Build steps can be configured to run concurrently in parallel using the `waitFor` attribute.
- C) Cloud Build requires administrators to manually manage, patch, and scale underlying build-agent virtual machines.
- D) Cloud Build only supports building applications developed in Java or Python.
- E) Cloud Build cannot connect to private on-premises code repositories.

<details>
<summary>[Yes] Answer</summary>

**A) Every single build step runs inside an isolated, containerized environment, B) Build steps can be configured to run concurrently in parallel using the `waitFor` attribute.**

**Explanation:** Cloud Build is serverless and executes steps inside isolated containers. By default steps run sequentially, but developers can configure `waitFor: ['-']` to execute steps in parallel. Cloud Build requires zero VM management (C is false) and supports any language or container runtime (D is false).
</details>

---

### Q13. Which security practices are strongly recommended for continuous delivery pipelines built with Cloud Build and Artifact Registry? (Select TWO)

- A) Create a dedicated, least-privileged Service Account for Cloud Build rather than relying on the broad default Editor service account.
- B) Store sensitive deployment credentials in Google Cloud Secret Manager and reference them dynamically during the build.
- C) Hardcode production database passwords and API tokens directly in the `cloudbuild.yaml` file.
- D) Configure all Artifact Registry repositories with public read/write permissions to simplify developer access.
- E) Commit unencrypted `terraform.tfstate` files containing plaintext secrets to public GitHub repositories.

<details>
<summary>[Yes] Answer</summary>

**A) Create a dedicated, least-privileged Service Account for Cloud Build rather than relying on the broad default Editor service account, B) Store sensitive deployment credentials in Google Cloud Secret Manager and reference them dynamically during the build.**

**Explanation:** Using scoped service accounts and storing secrets inside Secret Manager follow Google Cloud least-privilege security principles. Hardcoding credentials in config files or public repositories creates severe security vulnerabilities.
</details>

---

### Q14. Which characteristics accurately describe HashiCorp Terraform when used to manage Google Cloud infrastructure? (Select TWO)

- A) It uses a declarative configuration syntax to express the desired end-state of infrastructure.
- B) It maintains a centralized state file mapping code resources to real-world cloud resources.
- C) It is a proprietary service developed, owned, and maintained exclusively by Google Cloud.
- D) It cannot provision networking or storage resources on Google Cloud.
- E) Google Cloud charges hourly licensing fees for downloading and running the Terraform CLI binary.

<details>
<summary>[Yes] Answer</summary>

**A) It uses a declarative configuration syntax to express the desired end-state of infrastructure, B) It maintains a centralized state file mapping code resources to real-world cloud resources.**

**Explanation:** Terraform is an open-source, multi-cloud declarative tool developed by HashiCorp that tracks provisioned infrastructure using a state file. It is not owned by Google (C is false) and Google does not charge software licensing fees for using Terraform (E is false).
</details>

---

### Q15. Which practices effectively optimize costs for Cloud Build and Artifact Registry? (Select TWO)

- A) Setting up automated Cleanup Policies in Artifact Registry to delete obsolete untagged container images.
- B) Creating Artifact Registry repositories in the same region as the target GKE or Cloud Run deployment runtimes.
- C) Disabling Docker layer caching to force full rebuilds on every commit.
- D) Storing all container images across three continents in multi-regional repositories.
- E) Provisioning 32-core custom build pools for trivial linting checks.

<details>
<summary>[Yes] Answer</summary>

**A) Setting up automated Cleanup Policies in Artifact Registry to delete obsolete untagged container images, B) Creating Artifact Registry repositories in the same region as the target GKE or Cloud Run deployment runtimes.**

**Explanation:** Cleanup policies remove unneeded container image layers, preventing cumulative storage bloat. Co-locating repositories in the same region as compute workloads eliminates cross-region data transfer egress fees.
</details>

---

## Score Tracker

| Score | Verdict |
| :---: | :--- |
| **15/15** | **DevOps & Automation Master:** 100% prepared for Cloud Build, Artifact Registry, and Terraform exam questions. |
| **13–14** | [Yes] **Solid Readiness:** Excellent grasp of CI/CD concepts; minor review of IAM roles or naming syntax needed. |
| **10–12** | Note: **Needs Review:** Re-read Section 6 (CI/CD Spectrum) and Section 9 (IAM Roles & Security). |
| **< 10** | **Re-study Phase 09:** Review the distinction between Continuous Delivery and Continuous Deployment, and Terraform remote state. |
