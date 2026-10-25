[<- Previous: Phase 01 - GCP Fundamentals](phase-01-gcp-fundamentals.md) | [-> Next: Phase 03 - Compute Services](phase-03-compute.md)

---

# Phase 2 - Identity & Security (Cloud IAM) (Exam Weight: 5/5)

> **Prerequisite:** [Phase 01 - Google Cloud Platform Fundamentals](phase-01-gcp-fundamentals.md)  
> **Estimated Study Time:** 4–5 Hours  
> **Exam Weight:** (Exam Weight: 5/5) (The Single Most Heavily Tested Domain on the ACE Exam)  
> **Covers:** IAM Fundamentals (Identity, Principal, User, Group, Domain, Service Account), Permissions/Role/Policy/Binding, IAM Roles (Basic/Predefined/Custom), IAM Policies (Attach/Remove/Inheritance/Conditions), Service Accounts (Creation/Keys/Impersonation), Workload Identity, Security Best Practices, IAM Analyzer, Organization Policies.

---

## 1. Definition

**IAM (Identity and Access Management)** is Google Cloud's centralized authorization system for controlling **WHO** (a person, group, or application) can do **WHAT** (an action, like "create a VM") on **WHICH** resource (a project, a bucket, or an individual VM).

---

## 2. Why does this service exist?

### Problem it solves:
- Without access control, any person with a Google account could create, delete, or inspect any resource in your cloud project - resulting in operational anarchy and catastrophic security breaches.
- Enterprises must enforce role-based access: a junior intern must not delete production databases; a finance manager must not alter VM network firewall rules.
- Applications and backend microservices need identities to communicate securely with cloud APIs (e.g., a web app backend reading files from a private Cloud Storage bucket).

### Why it was created:
- Google created IAM to provide a **unified, centralized permission system** across all GCP services. Rather than having separate, disparate access systems for Compute Engine, Cloud Storage, BigQuery, and Cloud SQL, Cloud IAM uses a single mental model across the entire cloud platform.

### What happens if IAM doesn't exist:
- Zero resource isolation: any authenticated user could accidentally wipe out corporate infrastructure.
- Zero machine-to-machine security: developers would resort to hardcoding database passwords and root credentials in plaintext code files.
- Complete regulatory non-compliance (inability to satisfy SOC2, ISO27001, HIPAA, or PCI-DSS auditing mandates).

> [!IMPORTANT]
> **Remember for ACE Exam (Exam Weight: 5/5):** IAM is embedded in nearly every question on the ACE exam. Questions focused on Compute Engine, GCS, or Networking frequently revolve around IAM role selection (*"Which role should you grant to follow the principle of least privilege?"*).

---

## 3. Architecture / Working

### 3.1 The IAM Request & Enforcement Flow
```
[Principal: User / Group / Service Account]
                     │
                     │ (1) Sends API request: "I want to do X on resource Y"
                     ▼
   [Google Cloud API / IAM Policy Enforcement Point]
                     │
                     │ (2) Checks: Does this principal possess a ROLE
                     │     containing the required PERMISSION bound to them
                     │     on this resource or any ancestor node?
                     ▼
          [Evaluates the IAM POLICY Tree]
                     │
         ┌───────────┴───────────┐
         ▼                       ▼
   YES (Allowed)           NO (Denied)
         │                       │
   Action proceeds        HTTP 403 Forbidden:
                          Permission Denied
```

### 3.2 The Core IAM Formula (Memorize This)
```
   PRINCIPAL   +   ROLE   =   BINDING
    (WHO)         (WHAT)      (The grant connecting WHO and WHAT)

   Multiple BINDINGS attached to a Resource = IAM POLICY
```

### Step-by-Step Internal Evaluation Flow:
1. **Authentication:** A Principal proves their identity via Google OAuth2 credentials or a Service Account token.
2. **Action Attempt:** The principal invokes an API method on a resource (e.g., `compute.instances.delete`).
3. **Hierarchy Traversal:** Google Cloud evaluates the IAM policy on the target resource and traverses upwards through all ancestor nodes (`Project -> Folder -> Organization`).
4. **Union Evaluation:** If **any** binding at **any** level grants a role containing the required permission, access is granted (permissions are additive).
5. **Condition Evaluation:** If an IAM Condition is present (e.g., time-window restriction), it is evaluated via Common Expression Language (CEL).
6. **Default Deny:** If no binding in the hierarchy permits the action, access is denied immediately.

> **Authorization Rule:** The IAM evaluation engine inspects the effective policy: if ANY ancestor or resource-level binding explicitly permits the requested action, access is granted. If no binding allows it, Google Cloud defaults to deny.

---

## 4. Core Components

### 4.1 Identity-Related Components
| Component | What it is | Why it exists | Real-World Analogy |
| :--- | :--- | :--- | :--- |
| **Identity** | Verified proof of who is making the request | The bedrock of access control; identity must be validated before authorization | Showing a verified government passport |
| **Principal** | The umbrella term for any entity grantable access (User, Group, Service Account, Domain) | Provides a standardized term across all IAM APIs | "Person or legal entity" |
| **User** | A human Google Account (`@gmail.com` or Workspace email) | Represents an individual human administrator or engineer | An individual employee with an ID card |
| **Group** | A Google Group containing multiple user accounts | Enables managing access for entire teams simultaneously rather than 1-by-1 | A department mailing list |
| **Domain** | A Google Workspace or Cloud Identity domain (`company.com`) | Grants access to every user in an organization domain simultaneously | A master key issued to everyone in a secure corporate campus |
| **Service Account** | A non-human identity used by applications and VMs to authenticate | Enables workloads to call APIs without human interaction | A "robot employee" with its own dedicated security badge |

---

### 4.2 Permission-Related Components
| Component | What it is | Why it exists | Real-World Analogy |
| :--- | :--- | :--- | :--- |
| **Permission** | The atomic unit of access (e.g., `compute.instances.delete`) | Granular building block representing an exact API action | A single key that unlocks one specific cabinet |
| **Role** | A curated collection of permissions bundled together | Prevents administrators from having to manually assign hundreds of individual permissions | A job title (e.g., "Cashier") that comes with standard responsibilities |
| **Policy** | The full collection of bindings attached to a resource | Complete access-control definition for a resource | The master security roster of a building |
| **Binding** | A single pairing of one Role with one or more Principals (optionally with a Condition) | The actual mechanism that connects identities to permissions | An entry in the ledger: "Priya has the Editor role" |

#### Example IAM Policy (JSON representation):
```json
{
  "bindings": [
    {
      "role": "roles/compute.viewer",
      "members": ["user:priya@company.com"]
    },
    {
      "role": "roles/storage.objectViewer",
      "members": ["group:analytics-team@company.com"]
    },
    {
      "role": "roles/storage.objectAdmin",
      "members": ["serviceAccount:backend-sa@prod-proj.iam.gserviceaccount.com"]
    }
  ]
}
```

---

## 5. Key Features

### 5.1 The 3 Types of IAM Roles
1. **Basic (Primitive) Roles:**
   - The original legacy roles: `roles/viewer`, `roles/editor`, `roles/owner`.
   - **`roles/viewer`:** Read-only across nearly all GCP services.
   - **`roles/editor`:** Viewer + deploy/modify/delete most resources.
   - **`roles/owner`:** Editor + billing management + IAM policy management.
   - *ACE Rule:* **Too broad for production environments!** Google explicitly advises against them.
2. **Predefined Roles:**
   - Fine-grained, service-specific roles curated and maintained by Google (e.g., `roles/compute.instanceAdmin.v1`, `roles/storage.objectViewer`).
   - Automatically updated by Google whenever new permissions are introduced.
   - *ACE Rule:* **The recommended default choice for all production workloads.**
3. **Custom Roles:**
   - User-defined roles created by cherry-picking exact permissions.
   - Created at the **Project** or **Organization** level (cannot be created at the Folder level).
   - Do **NOT** auto-update when Google introduces new service permissions; must be maintained manually.

---

### 5.2 IAM Conditions
Allows role bindings to activate only when specific conditions are met using Common Expression Language (CEL):
- **Temporary Access:** Restrict access based on date/time timestamps (e.g., contractor access expiring in 30 days).
- **Resource Attributes:** Restrict role assignment to resources matching specific name prefixes or Tags.

```bash
# Example condition: Access expires on Dec 31, 2026
request.time < timestamp("2026-12-31T00:00:00Z")
```

---

### 5.3 Service Account Keys vs. Impersonation
- **Downloaded Service Account Keys (JSON):** Long-lived credentials stored on physical machines. High security risk if leaked to GitHub or compromised machines.
- **Service Account Impersonation:** Allows a user or service account to temporarily assume the identity of another service account using short-lived OAuth2 tokens. No physical key file is ever downloaded.
- **Workload Identity Federation:** Enables workloads running outside Google Cloud (AWS, Azure, on-premises, GitHub Actions) to authenticate and exchange their native tokens for short-lived GCP tokens without service account keys.

> **Security Best Practice:** Never download static credential files to workstations or CI/CD pipelines. Workload Identity establishes mutual OIDC federation so external workloads dynamically exchange native tokens for ephemeral, short-lived GCP tokens.

---

## 6. Types / Variants

### 6.1 IAM Role Types Comparison
| Feature | Basic (Primitive) | Predefined | Custom |
| :--- | :--- | :--- | :--- |
| **Granularity** | Sweeping (Whole Project) | Granular (Per Service) | Exact (Cherry-picked permissions) |
| **Maintained By** | Google | Google (Auto-updated) | Customer (Manual maintenance) |
| **Scope Levels** | Org, Folder, Project, Resource | Org, Folder, Project, Resource | Org and Project only |
| **Production Use** | [No] Avoid | [Yes] Recommended Default | Note: Only when Predefined doesn't fit |
| **Lifecycle States** | Always GA | Always GA | Alpha, Beta, GA, Disabled, Deleted |

---

### 6.2 Principal Types
| Principal Identifier | Format Example | Description / Use Case |
| :--- | :--- | :--- |
| **User** | `user:alex@company.com` | Individual human with a Google Account |
| **Group** | `group:devs@company.com` | Google Group containing multiple users |
| **Domain** | `domain:company.com` | All users belonging to a Cloud Identity / Workspace domain |
| **Service Account** | `serviceAccount:sa@proj.iam.gserviceaccount.com` | Non-human machine/application identity |
| **`allUsers`** | `allUsers` | Public internet access; anyone with or without authentication |
| **`allAuthenticatedUsers`** | `allAuthenticatedUsers` | Anyone with any valid Google account anywhere worldwide |

> [!CAUTION]
> **ACE Exam Alert:** Granting permissions to `allUsers` or `allAuthenticatedUsers` makes resources publicly accessible to anyone on the internet. This is a massive security trap on the exam.

---

### 6.3 Service Account Types
| Type | Description | Best Practice |
| :--- | :--- | :--- |
| **User-Managed** | Explicitly created by developers for specific microservices | **Always prefer this.** Name descriptively and apply least privilege. |
| **Default Service Account** | Auto-created when APIs are enabled (e.g., Compute Engine default SA) | Historically granted `Editor` role. **Revoke broad access or disable.** |
| **Google-Managed (Service Agents)** | Internal identities Google uses to perform actions on your behalf | Identified by `@cloudservices.gserviceaccount.com`. Do not modify. |

---

## 7. Configuration & Important Settings

### Key Settings for IAM Role Bindings
- **Member:** Identity string (`user:`, `group:`, `serviceAccount:`, `domain:`).
- **Role:** Full role identifier (`roles/storage.objectViewer` or `projects/[ID]/roles/[CustomRole]`).
- **Condition:** CEL expression specifying constraints.

### Crucial IAM Administrative Roles
- `roles/resourcemanager.projectIamAdmin`: Administer IAM policies on projects without full resource control.
- `roles/iam.securityAdmin`: Administer IAM policies across all resource types.
- `roles/iam.serviceAccountAdmin`: Create, update, and delete service accounts.
- `roles/iam.serviceAccountUser`: Allows a user to attach or run operations *as* a service account.
- `roles/iam.serviceAccountTokenCreator`: Allows generating short-lived credentials / impersonating a service account.

---

## 8. Pricing & Cost Considerations

- **Cloud IAM is 100% FREE:** There are zero fees for creating users, groups, service accounts, roles, custom roles, or policy bindings.
- **Indirect Cost Risks:** Over-privileged users (e.g., granting `Owner` or `Editor` indiscriminately) can accidentally spin up massive multi-GPU compute instances, leading to runaway bills.

> [!TIP]
> **Exam Distractor:** Any exam question suggesting that Custom Roles or IAM policy bindings incur monthly charges is completely false.

---

## 9. Security Considerations

### Security Principles:
1. **Principle of Least Privilege:** Grant only the exact permissions needed to execute a job, for the minimum duration required.
2. **Separation of Duties:** Partition high-risk responsibilities (e.g., code deployment, billing admin, security auditing) across separate roles and identities.
3. **Use Groups Instead of Direct Users:** Bind roles to Google Groups; manage user onboarding/offboarding via group memberships.
4. **Eliminate Downloaded Service Account Keys:** Attach service accounts directly to compute runtimes (Compute Engine, GKE, Cloud Run) so credentials are auto-rotated by the Metadata Server.
5. **De-privilege Default Service Accounts:** Immediately revoke the default `Editor` role granted to the Compute Engine default service account.

---

## 10. Advantages

1. **Granular Access Control:** Precision permissions reduce internal blast radiuses.
2. **Centralized Identity Management:** Single point of governance across all Google Cloud services.
3. **Auditable Traceability:** Every IAM check and administrative modification is permanently recorded in Cloud Audit Logs.
4. **Zero Key Management via Workload Identity:** Eliminates leaked credential vectors for cloud and hybrid applications.
5. **Automated Recommendations:** IAM Recommender automatically identifies and flags unused permissions based on 90-day activity logs.

---

## 11. Limitations

1. **Inheritance Cannot Be Blocked by IAM Alone:** Lower nodes cannot revoke permissions inherited from parent folders or organizations (requires Organization Policies or IAM Deny policies).
2. **Custom Roles Do Not Auto-Update:** When GCP introduces new features or permissions, custom roles remain static and must be updated manually.
3. **No Folder-Level Custom Roles:** Custom roles can only be scoped to Projects or Organizations, not Folders.

---

## 12. Real-World Use Cases

1. **Contractor Onboarding:** Grant a vendor access to a storage bucket using an IAM Condition configured to auto-expire at midnight on the contract end date.
2. **CI/CD Deployment via GitHub Actions:** Authenticate GitHub Actions pipelines into GCP using Workload Identity Federation, eliminating JSON keys.
3. **Production Database Protection:** Grant application backend service accounts `roles/cloudsql.client` while granting human DBAs `roles/cloudsql.admin`.
4. **Data Analytics Isolation:** Give business analysts `roles/bigquery.dataViewer` and `roles/bigquery.jobUser` on specific datasets without granting VM or network access.
5. **Least-Privilege VM Operation:** Attach a user-managed service account to a Compute Engine VM with write-only permissions (`roles/storage.objectCreator`) to an ingestion bucket.

---

## 13. Hands-on (Console + CLI)

### Console Walkthrough
1. **Grant IAM Role:** Navigation Menu -> **IAM & Admin** -> **IAM** -> Click **Grant Access** -> Enter principal email -> Select role (e.g., `roles/viewer`) -> Click **Save**.
2. **Create Service Account:** Navigation Menu -> **IAM & Admin** -> **Service Accounts** -> Click **Create Service Account** -> Provide ID and description -> Assign role -> Click **Done**.
3. **Create Custom Role:** Navigation Menu -> **IAM & Admin** -> **Roles** -> Click **Create Role** -> Set Title and ID -> Click **Add Permissions** -> Filter and select permissions -> Click **Create**.

### CLI (`gcloud`) Master Commands
```bash
# 1. Grant a predefined role to a user on a project
gcloud projects add-iam-policy-binding [PROJECT_ID] \
  --member="user:priya@company.com" \
  --role="roles/compute.viewer"

# 2. Remove an IAM role binding from a user
gcloud projects remove-iam-policy-binding [PROJECT_ID] \
  --member="user:priya@company.com" \
  --role="roles/compute.viewer"

# 3. Grant a role with a time-based expiration condition
gcloud projects add-iam-policy-binding [PROJECT_ID] \
  --member="user:contractor@vendor.com" \
  --role="roles/storage.objectViewer" \
  --condition='expression=request.time < timestamp("2026-12-31T00:00:00Z"),title=temp-vendor-grant,description=Expires end of 2026'

# 4. View effective IAM policy on a project in JSON format
gcloud projects get-iam-policy [PROJECT_ID] --format=json

# 5. Create a user-managed service account
gcloud iam service-accounts create backend-runner-sa \
  --display-name="Backend Microservice Runner"

# 6. List all service accounts in the project
gcloud iam service-accounts list

# 7. Impersonate a service account to generate a short-lived access token
gcloud auth print-access-token \
  --impersonate-service-account=backend-runner-sa@[PROJECT_ID].iam.gserviceaccount.com

# 8. Create a custom IAM role from a YAML definition file
gcloud iam roles create customVmManager \
  --project=[PROJECT_ID] \
  --file=custom-role-def.yaml
```

---

## 14. ACE Exam Focus (Exam Weight: 5/5)

### Most Commonly Tested Scenarios:
- **Least Privilege Role Selection:** Given a job description, pick the narrowest predefined role (e.g., `roles/storage.objectViewer` vs `roles/storage.admin`).
- **Service Account Key Replacement:** Scenarios emphasizing security best practices will always choose **attaching service accounts directly to VMs** or using **Workload Identity Federation** over downloading JSON key files.
- **The Role That Allows Attaching SAs:** `roles/iam.serviceAccountUser` is mandatory for a developer to deploy a VM or Cloud Run service that runs as a designated service account.
- **Temporary Access:** Always implemented via **IAM Conditions with time stamps**.

---

## 15. Service Comparisons

### Basic vs. Predefined vs. Custom Roles
| Dimension | Basic Roles | Predefined Roles | Custom Roles |
| :--- | :--- | :--- | :--- |
| **Maintenance** | None (Google) | Automatic (Google) | Manual (Customer) |
| **Permissions** | Broad (1000s) | Curated (Job-specific) | Precise (User-selected) |
| **Best Practice** | Avoid in production | **Preferred default** | Use when no predefined role matches |

### Service Account Authentication Methods
| Method | Credential Lifetime | Risk Profile | Best Practice Recommendation |
| :--- | :--- | :--- | :--- |
| **Attached to VM/Cloud Run** | Short-lived (Metadata Server) | Zero key leakage risk | **Highest Recommendation** |
| **Impersonation** | Short-lived (1 hour max) | Low risk | **Recommended for human workflows** |
| **Workload Identity Federation**| Short-lived (Federated tokens)| Low risk | **Recommended for Multi-cloud / CI-CD**|
| **Downloaded JSON Key File** | Long-lived (Until revoked) | High risk (leakage) | **Avoid where possible** |

---

## 16. Best Practices

- Enforce the **Principle of Least Privilege** across all environments.
- Assign roles to **Google Groups**, never directly to individual email accounts.
- Use **Predefined Roles** by default; create Custom Roles only when regulatory mandates require exact least privilege.
- Never download long-lived Service Account JSON keys to client laptops or VM disks.
- Use **IAM Conditions** to time-box contractor and temporary support access.
- Audit permission sprawl continuously using **IAM Recommender** and **Cloud Asset Inventory**.

---

## 17. Common Mistakes

- Granting `Owner` or `Editor` to developers to quickly bypass permission errors.
- Committing Service Account JSON keys into public GitHub repositories.
- Leaving the **Default Compute Engine Service Account** with its default broad `Editor` permissions.
- Accidentally making Cloud Storage buckets public by granting roles to `allUsers`.
- Forgetting that custom roles do not update automatically when Google adds new service permissions.

---

## 18. Quick Revision Sheet

- **Core Formula:** `Principal + Role = Binding` -> `Set of Bindings = Policy`.
- **Role Types:** Basic (Owner/Editor/Viewer) -> Predefined (Google-curated) -> Custom (User-curated).
- **Service Account Golden Rule:** Attach to resources directly or use Workload Identity; **do not download JSON keys**.
- **Important Role Names:**
  - `roles/iam.serviceAccountUser`: Allows attaching an SA to a resource.
  - `roles/iam.serviceAccountTokenCreator`: Allows impersonating an SA.
  - `roles/resourcemanager.projectIamAdmin`: Allows modifying IAM policies on projects.
- **IAM Inheritance:** Strictly **additive (Union)** down the hierarchy (`Org -> Folder -> Project -> Resource`).
- **Temporary Access:** Implemented via **IAM Conditions** with CEL timestamp expressions.

---

## 19. Interview Questions

**Q1: What is the difference between a Permission and a Role in Google Cloud IAM?**  
*Answer:* A permission is an atomic action on a service (e.g., `compute.instances.start`). A role is a named collection of permissions bundled together for assignment to principals.

**Q2: Why does Google Cloud recommend avoiding Basic roles (Owner, Editor, Viewer)?**  
*Answer:* Basic roles grant sweeping access across nearly all services in a project, violating the Principle of Least Privilege and increasing the risk of accidental outages or data breaches.

**Q3: How should an application on Compute Engine authenticate to Cloud Storage without service account keys?**  
*Answer:* Attach a user-managed service account directly to the Compute Engine VM. The application can then use Application Default Credentials (ADC) to fetch short-lived tokens from the internal Metadata Server.

**Q4: What is the purpose of the `roles/iam.serviceAccountUser` role?**  
*Answer:* It grants a user permission to attach a service account to a compute instance (like a VM or Cloud Run service) so the instance can run operations under that identity.

**Q5: What happens when a new permission is added to a service? Do Custom Roles receive it?**  
*Answer:* No. Predefined roles are automatically updated by Google, but Custom Roles are static and must be manually updated by an administrator.

---

## 20. ACE Practice Questions

*(Full practice drill available in [practice-questions/by-phase/phase-02.md](../practice-questions/by-phase/phase-02.md))*

### Question 1 (IAM Formula)
Which combination correctly represents the fundamental Google Cloud IAM relationship?
- [ ] A. Permission + Policy = Role
- [x] B. Principal + Role = Binding
- [ ] C. Binding + Condition = Permission
- [ ] D. Role + Resource = Principal
*Explanation: A Principal combined with a Role creates a Binding; multiple bindings on a resource form an IAM Policy.*

### Question 2 (Least Privilege)
A junior data analyst requires access to run queries against BigQuery datasets in a production project, but should not be allowed to modify tables or view compute instances. What roles should be granted?
- [ ] A. `roles/editor`
- [ ] B. `roles/bigquery.admin`
- [x] C. `roles/bigquery.dataViewer` and `roles/bigquery.jobUser`
- [ ] D. `roles/viewer`
*Explanation: `bigquery.dataViewer` grants read-only table access and `bigquery.jobUser` provides permission to execute query jobs, adhering strictly to least privilege.*

---

## 21. Related Topics & Navigation

- **Study Before This:** [Phase 01 - GCP Fundamentals & Resource Hierarchy](phase-01-gcp-fundamentals.md).
- **Study After This:** [Phase 03 - Compute Services](phase-03-compute.md).
- **Directly Related GCP Services:** Cloud IAM, Resource Manager, Service Accounts, Cloud KMS, Security Command Center, Policy Analyzer, Cloud Asset Inventory.
- **Official ACE Exam Guide Mappings:**
  - *Section 5.1:* Managing Identity and Access Management (IAM) (Managing IAM role bindings, custom roles, service accounts).
  - *Section 5.2:* Managing service accounts (Granting scopes, service account user roles, impersonation).

---

[<- Previous: Phase 01 - GCP Fundamentals](phase-01-gcp-fundamentals.md) | [-> Next: Phase 03 - Compute Services](phase-03-compute.md)
