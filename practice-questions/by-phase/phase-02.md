# Phase 02 Practice Drill - Identity & Security (Cloud IAM)

> **Total Questions:** 15 (10 MCQs + 5 MSQs)  
> **Topic Tag:** `#phase-02-iam`  
> **Target Score:** 13/15 (86%+)  
> **Covers:** IAM Formula (Principal + Role = Binding), Basic vs Predefined vs Custom Roles, Service Accounts, Keys vs Impersonation, Workload Identity, IAM Conditions, Principle of Least Privilege, Separation of Duties.

---

## Multiple Choice Questions (MCQs - Single Answer)

### Question 1 [IAM Formula]
Which combination correctly represents the fundamental Google Cloud IAM formula for granting access?
- [ ] A. Permission + Policy = Role
- [ ] B. Principal + Role = Binding
- [ ] C. Binding + Condition = Permission
- [ ] D. Role + Resource = Principal

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**
In Google Cloud IAM:
- **Principal (WHO)** + **Role (WHAT)** = **Binding**.
- Multiple Bindings attached to a specific Resource form an **IAM Policy**.

**Why other options are incorrect:**
- **A:** Permissions grouped together form a Role, not a Policy.
- **C:** A Condition can be attached to a Binding, but does not form a Permission.
- **D:** Syntax is nonsensical in IAM terminology.

> **Key Exam Takeaway:** Principal + Role = Binding. Multiple Bindings = Policy.
</details>

---

### Question 2 [Basic vs Predefined Roles]
Why does Google Cloud explicitly recommend against using Basic (Primitive) roles (Owner, Editor, Viewer) in production environments?
- [ ] A. Basic roles incur additional hourly licensing charges.
- [ ] B. Basic roles grant sweeping access across almost all services in a project, violating the Principle of Least Privilege.
- [ ] C. Basic roles cannot be assigned to Service Accounts.
- [ ] D. Basic roles require manual permission updates every time Google releases a new service.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**
Basic roles (`roles/viewer`, `roles/editor`, `roles/owner`) grant coarse-grained, sweeping access across nearly every Google Cloud service in the project. This introduces severe security vulnerabilities and violates the Principle of Least Privilege.

**Why other options are incorrect:**
- **A:** All IAM roles and assignments in Google Cloud are completely free of charge.
- **C:** Basic roles can be assigned to any valid principal type, including Service Accounts.
- **D:** This describes Custom roles, not Basic roles.

> **Key Exam Takeaway:** Never use Basic roles (Owner/Editor/Viewer) in production. Always default to Predefined roles.
</details>

---

### Question 3 [Service Account Security]
A developer is building a Python application deployed on a Compute Engine virtual machine that must read files from a private Cloud Storage bucket. Corporate security policy strictly forbids downloading or storing Service Account JSON key files. What is the Google-recommended approach?
- [ ] A. Generate a Service Account JSON key file and store it inside the VM startup script.
- [ ] B. Attach a user-managed Service Account with `roles/storage.objectViewer` directly to the Compute Engine VM instance.
- [ ] C. Hardcode the project Owner's personal credentials into the application configuration file.
- [ ] D. Grant the `allUsers` principal read access to the Cloud Storage bucket.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**
Attaching a Service Account directly to a Compute Engine VM allows the application to leverage **Application Default Credentials (ADC)**. The internal Metadata Server provides automatically rotated, short-lived OAuth2 access tokens without requiring long-lived key files.

**Why other options are incorrect:**
- **A:** Storing keys in startup scripts exposes credentials and violates the security mandate.
- **C:** Storing human credentials in code is a critical security vulnerability.
- **D:** Granting `allUsers` makes private data publicly accessible to the entire internet.

> **Key Exam Takeaway:** Attach Service Accounts directly to compute instances; never download JSON key files.
</details>

---

### Question 4 [Predefined vs Custom Roles]
What is the PRIMARY operational difference between Google Cloud Predefined Roles and Custom Roles?
- [ ] A. Predefined roles are free, whereas Custom roles incur a monthly maintenance fee.
- [ ] B. Predefined roles are curated and automatically updated with new permissions by Google; Custom roles must be manually maintained by the customer.
- [ ] C. Custom roles can only be granted to Service Accounts.
- [ ] D. Predefined roles cannot be applied at the Organization level.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**
When Google introduces new features or permissions to a service, existing Predefined roles are automatically updated to include them. Custom roles are static snapshots created by the customer; they never auto-update and require ongoing manual administration.

**Why other options are incorrect:**
- **A:** Both Predefined and Custom roles are completely free.
- **C:** Custom roles can be bound to any principal type (Users, Groups, SAs).
- **D:** Predefined roles can be granted at all levels of the hierarchy (Org, Folder, Project, Resource).

> **Key Exam Takeaway:** Predefined roles auto-update; Custom roles require manual updates by you.
</details>

---

### Question 5 [Temporary Access Governance]
Your enterprise hires an external consulting firm to audit a Cloud Storage bucket for 30 days. You need to grant the consultants read access that terminates automatically after 30 days without requiring an administrator to manually revoke the permission. What feature should you implement?
- [ ] A. Create a Custom Role and set its launch stage to `DELETED` after 30 days.
- [ ] B. Bind the `roles/storage.objectViewer` role with an **IAM Condition** containing a time-based Common Expression Language (CEL) expression.
- [ ] C. Grant the `allAuthenticatedUsers` principal access to the bucket.
- [ ] D. Assign the `roles/viewer` basic role at the project level.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**
**IAM Conditions** support Common Expression Language (CEL) expressions based on timestamps (e.g., `request.time < timestamp("2026-10-31T00:00:00Z")`). When the timestamp passes, the binding is automatically invalidated without manual intervention.

**Why other options are incorrect:**
- **A:** Setting a custom role to deleted disables it immediately for all bound users, and does not happen automatically.
- **C:** Grants access to any Google-authenticated user worldwide, creating a massive data breach.
- **D:** Grants project-wide read access permanently without automatic expiration.

> **Key Exam Takeaway:** Temporary, self-expiring access = IAM Conditions with CEL time expressions.
</details>

---

### Question 6 [Security Principles]
An enterprise policy mandates that developers who write and deploy application code must not have permission to modify production billing configurations, and billing administrators must not have permissions to deploy code. Which security principle does this implement?
- [ ] A. Defense in Depth
- [ ] B. Separation of Duties
- [ ] C. Multi-Factor Authentication
- [ ] D. High Availability

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**
**Separation of Duties** is the security practice of dividing critical responsibilities among multiple people or roles to prevent fraud, unauthorized changes, and single points of failure.

**Why other options are incorrect:**
- **A (Defense in Depth):** Layering multiple security controls (e.g., firewalls + IAM + encryption).
- **C & D:** Unrelated to partitioning administrative responsibilities.

> **Key Exam Takeaway:** Dividing tasks across distinct roles = Separation of Duties.
</details>

---

### Question 7 [Public Exposure Red Flags]
What does the principal identifier `allUsers` represent when configured in an IAM policy binding on a Cloud Storage bucket?
- [ ] A. All authenticated employees within your Google Workspace domain.
- [ ] B. All users who are members of an approved project Google Group.
- [ ] C. Literally anyone on the public internet, whether authenticated or anonymous.
- [ ] D. All service accounts created within the project.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `C`

**Why it's correct:**
`allUsers` represents public, anonymous internet access. Anyone in the world with an internet connection can invoke the bound permission without providing credentials.

**Why other options are incorrect:**
- **A:** Represented by `domain:company.com`.
- **B:** Represented by `group:group-name@company.com`.
- **D:** Service accounts must be specified individually or via groups.

> **Key Exam Takeaway:** `allUsers` = completely public internet access.
</details>

---

### Question 8 [Access Auditing & Optimization]
Which Google Cloud tool analyzes actual 90-day permission usage logs to identify over-privileged accounts and recommends removing unused permissions to enforce least privilege?
- [ ] A. Cloud Billing Reports
- [ ] B. IAM Recommender
- [ ] C. Cloud Trace
- [ ] D. Cloud Marketplace

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**
**IAM Recommender** continuously evaluates actual permission usage against granted roles. If a user has `roles/editor` but only uses Compute Engine viewing permissions, IAM Recommender automatically suggests downgrading to `roles/compute.viewer`.

**Why other options are incorrect:**
- **A:** Tracks financial spending and costs.
- **C:** Distributed application latency tracing.
- **D:** Commercial software deployment catalog.

> **Key Exam Takeaway:** Analyzing excess access and right-sizing roles = IAM Recommender.
</details>

---

### Question 9 [Default Service Account Risks]
A developer creates a new Compute Engine virtual machine via the Google Cloud Console without specifying a Service Account. Which Service Account is automatically assigned to the instance?
- [ ] A. No Service Account is attached; the VM cannot access any GCP APIs.
- [ ] B. The Compute Engine default service account, which historically possesses broad `Editor` permissions.
- [ ] C. A temporary anonymous service account with read-only access.
- [ ] D. The Organization root administrator service account.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**
If unconfigured, Compute Engine automatically attaches the **Default Compute Engine Service Account** (`[PROJECT_NUMBER]-compute@developer.gserviceaccount.com`), which historically has broad `roles/editor` access. Google recommends replacing it with a user-managed least-privilege service account.

**Why other options are incorrect:**
- **A:** Google Cloud automatically attaches a default SA unless explicitly configured otherwise.
- **C & D:** Fabricated non-existent service account concepts.

> **Key Exam Takeaway:** The default Compute Engine SA has broad access; always replace with custom least-privilege SAs.
</details>

---

### Question 10 [Role Maintenance Responsibility]
Which of the following statements accurately summarizes the maintenance responsibility for Predefined versus Custom IAM roles?
- [ ] A. Both Predefined and Custom roles automatically receive new permissions when Google updates services.
- [ ] B. Predefined roles automatically receive new permissions from Google; Custom roles require manual maintenance by the customer.
- [ ] C. Custom roles auto-update; Predefined roles require manual updates.
- [ ] D. Neither role type can ever be modified after creation.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `B`

**Why it's correct:**
Google actively manages Predefined roles. When a service adds new API capabilities, Google updates the corresponding predefined roles. Custom roles are customer-managed and static; they never receive new permissions automatically.

**Why other options are incorrect:**
- **A, C, D:** Inaccurate statements regarding role management ownership.

> **Key Exam Takeaway:** Predefined = Google maintained. Custom = Customer maintained.
</details>

---

## Multiple Select Questions (MSQs - Multiple Correct Answers)

### Question 11 [Valid Principal Types]
Which of the following are valid IAM Principal types in Google Cloud? (Select THREE)
- [ ] A. User (`user:email@example.com`)
- [ ] B. Group (`group:team@example.com`)
- [ ] C. Region (`region:asia-south1`)
- [ ] D. Service Account (`serviceAccount:sa@proj.iam.gserviceaccount.com`)
- [ ] E. VPC Network (`network:default`)

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answers:** `A`, `B`, and `D`

**Why they are correct:**
IAM principals represent identities that can be authenticated: Users, Google Groups, Cloud Identity Domains, and Service Accounts.

**Why other options are incorrect:**
- **C & E:** Regions and VPC Networks are physical and virtual infrastructure resources, not IAM identities.

> **Key Exam Takeaway:** Principals: Users, Groups, Domains, Service Accounts, and public identifiers (`allUsers`).
</details>

---

### Question 12 [Service Account Best Practices]
Which of the following are recommended Google Cloud security best practices for authenticating applications and developers? (Select TWO)
- [ ] A. Attaching user-managed Service Accounts directly to compute resources (VMs, Cloud Run) to use Application Default Credentials.
- [ ] B. Generating JSON Service Account keys and committing them to the project code repository.
- [ ] C. Using Service Account Impersonation to obtain short-lived credentials for human administrative tasks.
- [ ] D. Emailing JSON key files to contracted development teams.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answers:** `A` and `C`

**Why they are correct:**
- **A:** Directly attaching SAs allows automated, secure credential provisioning via the internal Metadata Server.
- **C:** Impersonation provides temporary (up to 1 hour) credentials without downloading or managing static key files.

**Why other options are incorrect:**
- **B & D:** Committing or emailing static JSON keys are critical security anti-patterns that frequently result in compromised cloud accounts.

> **Key Exam Takeaway:** Never download or email keys; prefer attached SAs and impersonation.
</details>

---

### Question 13 [IAM Condition Capabilities]
Which of the following statements about Google Cloud IAM Conditions are TRUE? (Select TWO)
- [ ] A. They can restrict role bindings based on request timestamps (e.g., date-based expiration).
- [ ] B. They can restrict access based on resource name prefixes or Resource Tags.
- [ ] C. They completely eliminate the need to assign IAM Roles.
- [ ] D. They can only be written in Python or Bash scripts.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answers:** `A` and `B`

**Why they are correct:**
- **A:** Time-based expressions allow automatic expiration of role assignments.
- **B:** Resource attribute expressions allow restricting roles to resources with specific names or Tags.

**Why other options are incorrect:**
- **C:** Conditions are applied *to* a role binding; they do not replace roles.
- **D:** Conditions are written in Common Expression Language (CEL), not Python or Bash.

> **Key Exam Takeaway:** IAM Conditions use Common Expression Language (CEL) to filter when bindings apply.
</details>

---

### Question 14 [Basic Primitive Roles]
Which of the following are classified as Basic (Primitive) IAM roles in Google Cloud? (Select THREE)
- [ ] A. `roles/viewer`
- [ ] B. `roles/editor`
- [ ] C. `roles/owner`
- [ ] D. `roles/storage.objectViewer`
- [ ] E. `roles/compute.networkAdmin`

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answers:** `A`, `B`, and `C`

**Why they are correct:**
`roles/viewer`, `roles/editor`, and `roles/owner` are the original three Basic/Primitive roles established before granular IAM existed.

**Why other options are incorrect:**
- **D & E:** These are Predefined roles curated for Cloud Storage and Compute Engine.

> **Key Exam Takeaway:** Primitive roles: Owner, Editor, Viewer.
</details>

---

### Question 15 [IAM Administration Best Practices]
Which of the following reflect official Google Cloud IAM security best practices? (Select THREE)
- [ ] A. Assign IAM roles to Google Groups rather than directly to individual user accounts.
- [ ] B. Grant the `roles/owner` role broadly to avoid permission disruption during development.
- [ ] C. Enforce the Principle of Least Privilege by granting only the minimum permissions required.
- [ ] D. Regularly audit effective access and remove obsolete permissions using IAM Recommender.
- [ ] E. Share a single Service Account across all applications in an organization.

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answers:** `A`, `C`, and `D`

**Why they are correct:**
- **A:** Binding roles to Google Groups simplifies onboarding and offboarding at scale.
- **C:** Least privilege minimizes security blast radiuses.
- **D:** IAM Recommender automates ongoing permission hygiene.

**Why other options are incorrect:**
- **B:** Granting Owner broadly is a severe security violation.
- **E:** Sharing one service account across multiple applications breaks workload isolation.

> **Key Exam Takeaway:** Groups + Least Privilege + Regular Auditing = Secure IAM posture.
</details>
