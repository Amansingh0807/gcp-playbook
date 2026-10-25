# Phase 10 Practice Drill - Billing & Cost Optimization

> **Total Questions:** 15 (10 MCQs + 5 MSQs)  
> **Topic Tag:** `#phase-10-billing-cost-optimization`  
> **Target Score:** 13/15 (86%+)  
> **Covers:** Billing Accounts, Budgets & Alerts, Programmatic Spend Capping via Pub/Sub & Cloud Functions, GCP Pricing Calculator, Quotas, Labels vs Tags for FinOps, Sustained Use Discounts (SUD), Committed Use Discounts (CUD), Cost Export to BigQuery.

---

## MCQ Section (Single Correct Answer)

---

### Q1. A project administrator creates a Cloud Budget of $1,000 per month with threshold alerts configured at 50%, 90%, and 100%. If the project accrues $1,050 in charges halfway through the month, what will happen?

- A) All active Compute Engine virtual machines will be stopped automatically.
- B) Google Cloud will reject all subsequent API requests with a billing error.
- C) An alert notification will be sent to billing administrators, but resources will continue running and accruing charges.
- D) The project will be automatically unlinked from the billing account.

<details>
<summary>[Yes] Answer</summary>

**C) An alert notification will be sent to billing administrators, but resources will continue running and accruing charges.**

**Explanation:** Cloud Budgets and alerts are strictly informational. They send notifications when thresholds are crossed, but they never terminate resources, reject API calls, or pause spending automatically. Charges will continue to accrue unless custom event-driven automation is specifically implemented.
</details>

---

### Q2. A company runs an enterprise relational database on Compute Engine that operates 24/7 with a completely predictable workload that will remain active for the next 3 years. Which pricing mechanism provides the greatest cost savings?

- A) Standard On-Demand pricing
- B) Relying solely on automatic Sustained Use Discounts
- C) Purchasing a 3-year Committed Use Discount (CUD)
- D) Running the database on Spot VMs

<details>
<summary>[Yes] Answer</summary>

**C) Purchasing a 3-year Committed Use Discount (CUD)**

**Explanation:** For predictable, steady-state, long-term workloads running continuously for 1 to 3 years, Committed Use Discounts offer the highest savings (up to 57%–70%). Spot VMs (D) are unsuitable for production databases due to preemption risks with only 30 seconds notice.
</details>

---

### Q3. Which statement accurately describes the architectural relationship between Cloud Billing Accounts and Google Cloud Projects?

- A) A project can be linked to multiple billing accounts simultaneously to split costs.
- B) One billing account can be linked to multiple projects, but each project can only link to one billing account at a time.
- C) A billing account must belong to exactly one project.
- D) Projects can only accrue costs if they are hosted in the same region as the billing account.

<details>
<summary>[Yes] Answer</summary>

**B) One billing account can be linked to multiple projects, but each project can only link to one billing account at a time.**

**Explanation:** The relationship is strictly one-to-many: a single Billing Account can fund multiple Projects, but each Project is linked to exactly one Billing Account at any given time.
</details>

---

### Q4. An organization wants to prevent engineers from accidentally creating more than 20 Compute Engine virtual machines in the `us-central1` region. Which Google Cloud mechanism enforces this restriction?

- A) A Cloud Budget with a 100% alert rule
- B) A resource label named `max-vms: 20`
- C) A regional Allocation Quota
- D) The Google Cloud Pricing Calculator

<details>
<summary>[Yes] Answer</summary>

**C) A regional Allocation Quota**

**Explanation:** Allocation Quotas enforce hard limits on the absolute quantity of infrastructure resources (such as CPUs, IP addresses, or VM instances) that can be provisioned in a given region or project. Budgets (A) only track currency expenditures and cannot block provisioning.
</details>

---

### Q5. A DevOps engineer needs to estimate the monthly infrastructure cost of a proposed microservices architecture comprising GKE, Cloud SQL, and Cloud Storage before provisioning any resources. Which tool should be used?

- A) Cloud Billing Reports in the Google Cloud Console
- B) Google Cloud Pricing Calculator
- C) BigQuery Billing Export
- D) Cloud Monitoring Dashboards

<details>
<summary>[Yes] Answer</summary>

**B) Google Cloud Pricing Calculator**

**Explanation:** The Google Cloud Pricing Calculator is a standalone web-based modeling tool designed specifically to estimate and forecast infrastructure costs prior to actual resource deployment without requiring any provisioned resources.
</details>

---

### Q6. What is required to automatically stop all infrastructure spending in a development project when a budget threshold is exceeded?

- A) Set a Budget threshold rule to 100% and check the "Auto-stop" box in the console.
- B) Connect the Budget to a Pub/Sub topic that triggers a Cloud Function to disable billing on the project.
- C) Apply a Resource Manager Tag that disables Compute Engine APIs.
- D) Request a quota decrease to zero from Google Support.

<details>
<summary>[Yes] Answer</summary>

**B) Connect the Budget to a Pub/Sub topic that triggers a Cloud Function to disable billing on the project.**

**Explanation:** Because Budgets do not natively stop spending, enforcing a hard spend cap requires configuring the Budget to publish to a Pub/Sub topic, which triggers a Cloud Function that programmatically unlinks or disables billing on the project via the Cloud Billing API.
</details>

---

### Q7. Which IAM role allows a user to inspect cost reports and configure budget alert thresholds without granting the ability to view credit card details or link projects?

- A) `roles/billing.admin`
- B) `roles/billing.user`
- C) `roles/billing.costsManager`
- D) `roles/billing.viewer`

<details>
<summary>[Yes] Answer</summary>

**C) `roles/billing.costsManager`**

**Explanation:** `roles/billing.costsManager` grants permissions to view cost reports and create/manage budgets, while intentionally withholding permissions to view sensitive payment instruments (credit cards, bank accounts) or link/unlink projects.
</details>

---

### Q8. How are Sustained Use Discounts (SUD) applied to eligible Compute Engine virtual machines?

- A) By submitting a formal reservation request to Google Cloud Support.
- B) Automatically applied by Google at the end of the billing cycle based on continuous runtime during the month.
- C) By purchasing a 1-year upfront commitment in the Google Cloud Console.
- D) By attaching the label `discount: sustained-use` to the VM instance.

<details>
<summary>[Yes] Answer</summary>

**B) Automatically applied by Google at the end of the billing cycle based on continuous runtime during the month.**

**Explanation:** Sustained Use Discounts are 100% automatic. Google calculates instance running duration across the billing month and applies tiered discounts automatically if the VM runs for more than 25% of the month, with zero administrative configuration needed.
</details>

---

### Q9. A FinOps team needs to analyze line-item infrastructure costs across departments by querying resource labels with SQL. What must be configured?

- A) Cloud Trace telemetry export
- B) Cloud Monitoring custom log metrics
- C) Cloud Billing Export to BigQuery
- D) Google Cloud Ops Agent on all virtual machines

<details>
<summary>[Yes] Answer</summary>

**C) Cloud Billing Export to BigQuery**

**Explanation:** Exporting Cloud Billing data to BigQuery streams detailed line-item cost, usage, and label telemetry into BigQuery tables, allowing teams to run complex analytical SQL queries and build Looker Studio dashboards.
</details>

---

### Q10. A cloud engineer deletes a Compute Engine virtual machine but discovers that the project continues to accrue storage charges. What is the most likely cause?

- A) The VM's IP address was automatically converted to a subnetwork.
- B) The attached Persistent Disk was not set to auto-delete and remains provisioned as an orphaned disk.
- C) Compute Engine charges a mandatory 30-day decommissioning fee.
- D) The billing account has not synchronized with the project.

<details>
<summary>[Yes] Answer</summary>

**B) The attached Persistent Disk was not set to auto-delete and remains provisioned as an orphaned disk.**

**Explanation:** By default, detached or unattached Persistent Disks are retained when a VM is deleted unless the `auto-delete` flag was explicitly enabled. Unattached disks continue to accrue standard monthly storage fees until explicitly deleted.
</details>

---

## MSQ Section (Multiple Select Questions)

---

### Q11. Which of the following statements regarding Google Cloud Budgets are TRUE? (Select TWO)

- A) Budgets can be scoped to specific projects, products, or resource labels.
- B) Budget alerts can be evaluated against both Actual Spend and Forecasted Spend.
- C) Budgets automatically terminate virtual machines when spend reaches 100%.
- D) A project can only have a single budget configured at any time.
- E) Budgets cannot send programmatic notifications to Cloud Pub/Sub.

<details>
<summary>[Yes] Answer</summary>

**A) Budgets can be scoped to specific projects, products, or resource labels, B) Budget alerts can be evaluated against both Actual Spend and Forecasted Spend.**

**Explanation:** Budgets can be granularly scoped and can trigger alerts based on actual or forecasted spend. They do not terminate VMs (C is false), support multiple budgets per project (D is false), and natively integrate with Pub/Sub (E is false).
</details>

---

### Q12. Which of the following are valid categories of Google Cloud Quotas? (Select TWO)

- A) Rate Quotas (limiting API requests over a short time window)
- B) Allocation Quotas (limiting the number of provisioned resources in a region)
- C) Financial Quotas (capping maximum monthly dollar spend on an invoice)
- D) Discount Quotas (limiting the number of Sustained Use Discounts applied)
- E) Network Bandwidth Ingress Quotas

<details>
<summary>[Yes] Answer</summary>

**A) Rate Quotas (limiting API requests over a short time window), B) Allocation Quotas (limiting the number of provisioned resources in a region).**

**Explanation:** Google Cloud enforces Rate Quotas (e.g., API requests per minute) and Allocation Quotas (e.g., CPUs per region). There are no native "Financial Quotas" or "Discount Quotas".
</details>

---

### Q13. Which statements regarding Committed Use Discounts (CUD) are ACCURATE? (Select TWO)

- A) They require an upfront contractual commitment of either 1 year or 3 years.
- B) The customer is financially obligated to pay for the committed capacity even if actual resource usage is lower.
- C) They apply automatically to any VM that runs for more than 25% of the month.
- D) They can be canceled at any time with a prorated refund from Google.
- E) They only apply to Cloud Storage buckets and cannot be used for Compute Engine.

<details>
<summary>[Yes] Answer</summary>

**A) They require an upfront contractual commitment of either 1 year or 3 years, B) The customer is financially obligated to pay for the committed capacity even if actual resource usage is lower.**

**Explanation:** CUDs require a 1- or 3-year commitment and obligate payment regardless of utilization. Option C describes Sustained Use Discounts, and CUDs are non-refundable contracts (D is false).
</details>

---

### Q14. Which IAM roles grant administrative or operational permissions specifically over Cloud Billing Accounts? (Select THREE)

- A) `roles/billing.admin`
- B) `roles/billing.costsManager`
- C) `roles/billing.user`
- D) `roles/compute.admin`
- E) `roles/resourcemanager.organizationAdmin`

<details>
<summary>[Yes] Answer</summary>

**A) `roles/billing.admin`, B) `roles/billing.costsManager`, C) `roles/billing.user`.**

**Explanation:** `billing.admin`, `billing.costsManager`, and `billing.user` are core billing IAM roles. `compute.admin` and `organizationAdmin` are resource management roles, not billing-specific roles.
</details>

---

### Q15. Which of the following practices represent effective Google Cloud cost optimization strategies? (Select TWO)

- A) Applying consistent resource labels to enable granular cost attribution in BigQuery billing exports.
- B) Deleting unassigned static external IP addresses and detached persistent disks.
- C) Purchasing 3-year Committed Use Discounts immediately for experimental development projects.
- D) Running production enterprise databases exclusively on Spot VMs.
- E) Relying exclusively on standard on-demand pricing for predictable 24/7 production workloads.

<details>
<summary>[Yes] Answer</summary>

**A) Applying consistent resource labels to enable granular cost attribution in BigQuery billing exports, B) Deleting unassigned static external IP addresses and detached persistent disks.**

**Explanation:** Standardized labeling enables FinOps cost attribution, and deleting unused storage/IPs eliminates idle waste. Purchasing long-term CUDs for experimental projects (C) and running databases on Spot VMs (D) are severe anti-patterns.
</details>

---

## Score Tracker

| Score | Verdict |
| :---: | :--- |
| **15/15** | **FinOps Master:** 100% prepared for Cloud Billing, Budgets, Quotas, and Discount models. |
| **13–14** | [Yes] **Solid Readiness:** Excellent grasp of billing concepts; minor review of IAM roles or automated spend caps needed. |
| **10–12** | Note: **Needs Review:** Re-read Section 6 (Discounts Comparison) and Section 14 (Exam Focus & Traps). |
| **< 10** | **Re-study Phase 10:** Review the core distinction between Budgets (alerts) and Quotas (hard limits). |
