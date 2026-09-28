[← Previous Phase: Cloud Fundamentals](phase-00-cloud-fundamentals.md) · [Next Phase: Identity & Security (IAM) →](phase-02-identity-security-iam.md)

---

# Phase 1 — Google Cloud Platform Fundamentals ⭐⭐⭐⭐⭐

> **Prerequisite:** [Phase 00 — Cloud Computing Fundamentals](phase-00-cloud-fundamentals.md)  
> **Estimated Study Time:** 3–4 Hours  
> **Exam Weight:** ⭐⭐⭐⭐⭐ (Highest Priority Exam Domain)  
> **Covers:** Module 1.1 (Console, Cloud Shell, Cloud SDK, gcloud CLI, APIs, Marketplace), Module 1.2 (Projects & Billing), Module 1.3 (Resource Hierarchy & Org Policies)

---

## 1. Definition

Google Cloud Console, Projects & Billing, and the Resource Hierarchy together form the operational skeleton of GCP — the tools used to interact with GCP (Console, Cloud Shell, `gcloud` CLI), the fundamental container that holds resources and billing (Projects), and the organizational tree that governs security policies (Organization ➔ Folder ➔ Project ➔ Resources).

> **In short:** These three pillars are the "control room" of GCP — Console and CLI let you interact with GCP, Projects are your billing and resource containers, and the Resource Hierarchy is your organizational chart where security policies flow automatically from top to bottom.

---

## 2. Why does this service exist?

### Problem it solves:
- An enterprise on the cloud might have hundreds of teams, thousands of virtual machines, and dozens of business units. Without a structured hierarchy, this results in administrative chaos — no one knows who owns what, who is paying for what, or how to enforce security rules across all workloads.
- Without a centralized resource hierarchy, security teams would have to apply IAM permissions and firewall policies to every single server and storage bucket individually — slow, error-prone, and unmaintainable.
- Without management interfaces (Console, CLI, SDK), interacting with Google Cloud would require hand-crafting low-level raw HTTP REST requests.

### Why it was created:
- **Console, Cloud Shell, and `gcloud` CLI** were built to provide multiple administrative paths: visual exploration for beginners and scripted automation for DevOps/CI-CD workflows.
- **Projects** were introduced as the fundamental isolation boundary — every single resource in GCP **must** belong to exactly one project.
- **Resource Hierarchy** (Organization ➔ Folders ➔ Projects ➔ Resources) was engineered so enterprises can define a security or compliance rule once at the top level and have it inherit downwards to thousands of child resources automatically.

### What happens if this doesn't exist:
- No centralized billing attribution leads to runaway, unallocated corporate cloud costs.
- No ability to enforce company-wide guardrails (like disabling external IP addresses) creates massive security vulnerabilities.

---

## 3. Architecture / Working

### 3.1 How You Interact with GCP (Unified API Gateway Architecture)
```
[User / Administrator]
         │
         ├── Option A: Google Cloud Web Console (console.cloud.google.com)
         ├── Option B: Cloud Shell (Ephemeral in-browser Linux terminal with gcloud)
         ├── Option C: Local Workstation with Cloud SDK (`gcloud`, `bq`, `gsutil`)
         └── Option D: Terraform / Infrastructure as Code / Client Libraries
         ▼
[Google Cloud APIs / Resource Manager Control Plane]
         │
         │ Authenticates caller identity (OAuth2 / Service Account)
         │ Evaluates IAM Permissions (Role bindings + Hierarchy check)
         ▼
[Target Google Cloud Service API] (Compute Engine API, Cloud Storage API, etc.)
         │
         ▼
[Target Resource is created/managed strictly inside a designated PROJECT]
```

> [!NOTE]
> **Key Insight:** The Cloud Console, Cloud Shell, and local `gcloud` CLI are simply different front doors to the exact same underlying Google Cloud REST APIs. Anything achievable in the web console can be automated via `gcloud` or API calls.

---

### 3.2 Resource Hierarchy & Policy Inheritance Architecture
```
                         ORGANIZATION NODE (e.g., "acme-corp.com")
                                     │
                 ┌───────────────────┴───────────────────┐
                 ▼                                       ▼
            FOLDER: "Engineering"                  FOLDER: "Marketing"
                 │                                       │
         ┌───────┴───────┐                               ▼
         ▼               ▼                       PROJECT: "mktg-web"
   PROJECT: "prod"  PROJECT: "dev"                       │
         │               │                               ▼
    [RESOURCES]     [RESOURCES]                     [RESOURCES]
   (VMs, GCS, DBs) (VMs, GCS, DBs)                (Cloud Run, GCS)
```

### How Inheritance Works:
1. **Top-Down Cascading:** Policies applied at the Organization level cascade down to all child Folders, sub-folders, Projects, and Resources.
2. **Folder Inheritance:** Policies applied at a Folder cascade to all child projects within that folder.
3. **IAM is Purely Additive:** A user's effective permissions are the **UNION** of all permissions granted across all parent nodes. You cannot restrict or remove an IAM permission at a project level if it was granted higher up at the folder or organization level.
4. **Organization Policies (Guardrails):** Unlike IAM (which governs *who* can do what), Organization Policies govern *what* can be done. Org policies can enforce restrictions regardless of a user's IAM privileges.

> **In short:** Think of it like a corporate hierarchy — the CEO's rules apply to everyone (Organization), department heads set rules for their departments (Folders), and team leads govern their teams (Projects). Lower levels automatically inherit the rules from above.

---

## 4. Core Components

### Module 1.1 — Administrative Tools
| Component | What it is | Why it exists | Real-World Analogy |
| :--- | :--- | :--- | :--- |
| **Cloud Console** | Graphical web UI (`console.cloud.google.com`) | Provides a visual, interactive dashboard for exploring and managing services | The **dashboard/touchscreen** of a modern car |
| **Cloud Shell** | Free Debian-based browser Linux VM with pre-installed tools (`gcloud`, `kubectl`, `git`) | Lets you run CLI commands without installing software on your machine | A **free rental laptop** that is always pre-configured |
| **Cloud SDK** | Downloadable CLI software bundle for local operating systems | Enables automation, local scripting, and CI/CD pipeline integration | A **heavy-duty physical toolbox** kept at home |
| **`gcloud` CLI** | Primary command-line tool for Google Cloud services | Enables automated scripting and faster administrative workflows | The **command language** used to communicate with the cloud |
| **Cloud Marketplace** | Digital catalog of pre-packaged, turnkey third-party software solutions | Enables 1-click deployment of production stacks (e.g., WordPress, Redis, Nginx) | An **App Store** for cloud infrastructure |

*Note on bundled utilities:* `gsutil` (for Cloud Storage), `bq` (for BigQuery), and `kubectl` (for Kubernetes Engine).

---

### Module 1.2 — Projects & Billing
| Component | What it is | Why it exists | Real-World Analogy |
| :--- | :--- | :--- | :--- |
| **Project** | Base container for all resources; every resource belongs to exactly ONE project | Isolates resources, security permissions, and billing | A **dedicated labeled folder** holding files for one client |
| **Project ID** | Globally unique, permanent, user-chosen string identifier | Used in CLI commands, API calls, and resource URLs worldwide | A **unique global username** across an entire platform |
| **Project Number** | Globally unique, immutable numeric ID auto-assigned by Google | Used internally by Google services and service agent provisioning | An **auto-generated internal employee ID number** |
| **Billing Account** | Payment entity linked to one or more projects | Decouples resource management from payment instruments | A **corporate credit card** linked to multiple employee expenses |
| **Budgets & Alerts** | Configurable spending thresholds that trigger email notifications | Warns administrators when spending approaches or exceeds defined targets | A **bank SMS alert** when your credit card spending crosses a limit |
| **Quotas** | Hard limits on resource allocation (e.g., max 24 vCPUs per region) | Protects Google's capacity and prevents accidental runaway customer costs | A **speed governor** on a rental vehicle |
| **Labels** | Key-value pairs (e.g., `env:production`, `dept:finance`) | Used for cost filtering, grouping, and billing breakdown reports | **Color-coded sticky notes** on paper folders |
| **Tags** | Hierarchy-aware key-value pairs used for policy enforcement | Allows conditional IAM and Organization Policy enforcement | A **security clearance badge** controlling access doors |

---

### Module 1.3 — Resource Hierarchy ⭐⭐⭐⭐⭐
| Component | What it is | Why it exists | Real-World Analogy |
| :--- | :--- | :--- | :--- |
| **Organization** | Root node of the hierarchy, linked to a Google Workspace or Cloud Identity domain | Represents the entire corporate enterprise; top-level governance boundary | The **global corporate headquarters** |
| **Folder** | Optional grouping layer between Organization and Projects; can nest up to 10 levels | Organizes projects by business unit, department, or environment (`dev`/`stage`/`prod`) | A **department or division** inside a corporation |
| **Inheritance** | Mechanism where IAM permissions and Org Policies cascade down the tree | Saves administrative time and guarantees enterprise-wide baseline compliance | A **company-wide policy** that all branches must adhere to |
| **Organization Policies** | Centralized, restriction-oriented guardrails (not identity-based) | Enforces hard constraints (e.g., "no public IPs", "restricted resource locations") | A **strict municipal building code** that applies to everyone |
| **Cloud Asset Inventory** | Searchable metadata database of all GCP resources and IAM policies over time | Provides comprehensive security auditing, compliance verification, and time-travel | A **CCTV archive and inventory ledger** of all corporate property |

---

## 5. Key Features

### Cloud Shell Features
- Includes **5GB of free persistent storage** in your `$HOME` directory that persists across sessions.
- Pre-authenticated with your Google account credentials — zero configuration needed.
- Ephemeral VM instance: VM tears down after 20–60 minutes of inactivity, but disk contents remain safe.
- Built-in web preview feature on port 8080.

### `gcloud` CLI Features
- Supports configuration profiles via `gcloud config configurations create`.
- Output formatting via `--format=json`, `--format=table(...)`, and `--format=yaml`.
- Query filtering via `--filter="key:value"`.

### Projects & Billing Features
- **Strict Single-Project Ownership:** Resources can never exist outside a project or belong to multiple projects simultaneously.
- **Project ID is Permanent:** Cannot be modified once created.
- **30-Day Recovery Window:** Deleted projects remain in a "pending deletion" state for 30 days before permanent purging.
- **Centralized Billing:** A single Billing Account can fund multiple projects, but a project connects to **only one** billing account at a time.

> [!IMPORTANT]
> **ACE Exam Rule:** The Resource Hierarchy is a **strict tree**. Every project and folder has exactly **one parent**.

---

## 6. Types / Variants

### 6.1 Ways to Access GCP
| Method | Interface | Best For | Setup Needed? |
| :--- | :--- | :--- | :--- |
| **Cloud Console** | Web GUI | Visual exploration, one-off setups, architecture viewing | No (Web browser) |
| **Cloud Shell** | In-Browser Terminal | Quick CLI actions, testing, troubleshooting | No (Zero-install) |
| **Cloud SDK** | Local Machine CLI | Local automation, CI/CD pipelines, long scripts | Yes (Download & install) |
| **REST APIs / SDKs** | Code libraries (Python, Go, Java) | Custom application integration | Yes (Client libraries) |
| **Terraform** | Declarative Code | Repeatable, enterprise-grade Infrastructure-as-Code | Yes (Terraform provider) |

### 6.2 Free Tier vs. Free Trial
| Aspect | Always Free Tier | 90-Day Free Trial |
| :--- | :--- | :--- |
| **Duration** | Forever | 90 Days from activation |
| **Allowance** | Specified resource quotas (e.g., 1 e2-micro VM, 5GB GCS) | $300 USD spending credit |
| **Eligibility** | All Google Cloud accounts | New Google Cloud customers only |
| **Scope** | Restricted to eligible products and specific regions | Usable across almost all Google Cloud services |
| **Auto-Billing** | No auto-charge unless quotas are exceeded | Never auto-bills upon expiration unless explicitly upgraded |

---

## 7. Configuration & Important Settings

### Key `gcloud` Configuration Commands
```bash
# Set default project for all subsequent CLI commands
gcloud config set project [PROJECT_ID]

# Set default compute region
gcloud config set compute/region asia-south1

# Set default compute zone
gcloud config set compute/zone asia-south1-a

# Set default account identity
gcloud config set account user@example.com

# Display active CLI configurations
gcloud config list
```

### Important Project Creation Settings
- **Project Name:** Human-readable display name (e.g., "E-Commerce Backend"). Can be updated at any time.
- **Project ID:** 6–30 characters, lowercase letters, digits, hyphens. Globally unique across all of Google Cloud worldwide. **Immutable.**
- **Project Number:** Auto-generated numeric identifier. Assigned by Google. **Immutable.**

---

## 8. Pricing & Cost Considerations

- The **Cloud Console, Cloud Shell, and `gcloud` CLI** are 100% free to use.
- Cloud Shell storage (5GB) is completely free.
- Billing is always calculated at the project level and charged to the associated Billing Account.

### Cost Optimization Tips
- Tag resources using consistent **Labels** (`environment=production`, `cost-center=1042`) to enable granular cost breakdowns in Cloud Billing exports to BigQuery.
- Set up automated spending alerts at 50%, 90%, and 100% of budgeted amounts.

> [!WARNING]
> **Number One ACE Exam Trap:** **Budgets and Alerts do NOT stop spending automatically!**  
> A budget alert only sends an email or a Pub/Sub message. To actually halt workloads and cap spend, you must implement automated logic (e.g., Cloud Function triggered via Pub/Sub that detaches the billing account or disables service APIs).

---

## 9. Security Considerations

### Crucial IAM Roles
- `roles/resourcemanager.organizationAdmin`: Full administrative control over the Organization.
- `roles/resourcemanager.folderAdmin`: Ability to create, edit, and move Folders.
- `roles/resourcemanager.projectCreator`: Permission to create new projects within an Org or Folder.
- `roles/billing.admin`: Manage billing accounts, payment methods, and project links.
- `roles/orgpolicy.policyAdmin`: Define and apply Organization Policy constraints.

### Security Best Practices
- Apply permissions at the **lowest necessary level** in the resource hierarchy (Least Privilege).
- Use Organization Policies to create non-negotiable guardrails (e.g., `compute.vmExternalIpAccess` set to Deny).
- Regularly inspect resource and IAM history with **Cloud Asset Inventory**.

---

## 10. Advantages

1. **Centralized Governance:** Organization Policies allow enterprise security teams to enforce compliance across thousands of projects simultaneously.
2. **Unified Billing Visibility:** Multiple projects roll up their costs into a single billing account for simplified corporate accounting.
3. **Zero-Setup Management:** Cloud Shell allows administrators to manage infrastructure securely from any device without local software installations.
4. **Hierarchical Inheritance:** Eliminates redundant configuration by allowing permissions and policies to inherit downwards automatically.

---

## 11. Limitations

1. **Project ID Immutability:** If a typo is made during Project ID creation, it can never be altered; the project must be deleted and recreated.
2. **Budgets Do Not Hard-Cap:** Requires manual serverless automation to stop resources when spending targets are exceeded.
3. **Hierarchy Requires Cloud Identity:** Folders and Organization Policies are unavailable to standalone Gmail accounts without Google Workspace or Cloud Identity.

---

## 12. Real-World Use Cases

1. **Multi-Department Enterprises:** Uses `Org ➔ Department Folders ➔ Environment Folders (Dev/Stage/Prod) ➔ Projects` to enforce strict isolation.
2. **Centralized FinOps Teams:** Ingests billing export data from dozens of projects into BigQuery to generate Looker Studio executive cost dashboards.
3. **Regulated Financial Services:** Implements an Organization Policy restricting all resource deployments strictly to compliant geographic regions.
4. **Automated CI/CD Workflows:** Spin up ephemeral test projects via `gcloud` CLI in automated testing pipelines and delete them upon completion.
5. **Vendor Management:** Agencies create separate projects for distinct clients, linking each client's project to their respective corporate billing account.

---

## 13. Hands-on (Console + CLI)

### Console Walkthrough
1. **Create a Project:** In Google Cloud Console, click the project selector dropdown in the top bar ➔ **New Project** ➔ Set Project Name ➔ Click **Create**.
2. **Link Billing Account:** Go to **Billing** in the Navigation Menu ➔ Select **My Projects** ➔ Select project ➔ **Change Billing Account**.
3. **Set a Budget:** Navigation Menu ➔ **Billing** ➔ **Budgets & Alerts** ➔ Click **Create Budget** ➔ Name budget ➔ Set target amount ➔ Configure 50%, 90%, 100% threshold rules ➔ Save.
4. **View Resource Hierarchy:** Navigation Menu ➔ **IAM & Admin** ➔ **Manage Resources**.

### CLI (`gcloud`) Master Commands
```bash
# 1. Create a new project
gcloud projects create my-ace-demo-project --name="ACE Demo Project"

# 2. List all accessible projects
gcloud projects list

# 3. View detailed project metadata
gcloud projects describe my-ace-demo-project

# 4. List available billing accounts
gcloud billing accounts list

# 5. Link a project to a billing account
gcloud billing projects link my-ace-demo-project --billing-account=012345-6789AB-CDEF01

# 6. Verify billing configuration
gcloud billing projects describe my-ace-demo-project

# 7. Create a folder under an Organization
gcloud resource-manager folders create --display-name="Engineering" --organization=[ORGANIZATION_ID]

# 8. Move a project into a specific folder
gcloud beta projects move my-ace-demo-project --folder=[FOLDER_ID]

# 9. Apply organizational labels for billing tracking
gcloud alpha projects update my-ace-demo-project --update-labels=environment=production,team=backend
```

---

## 14. ACE Exam Focus ⭐⭐⭐⭐⭐

### Top Tested Concepts:
- **Resource Hierarchy Order:** `Organization ➔ Folder ➔ Project ➔ Resource`.
- **Project ID vs. Project Number:** User-chosen string vs. Google-assigned integer.
- **IAM Inheritance:** Effective permissions are always the **Union** of all ancestor grants.
- **Budgets:** Budgets notify; they do not automatically shut off resources.
- **Labels vs. Tags:** Labels are for cost reporting/metadata; Tags are hierarchy-aware objects used for conditional policy enforcement.

---

## 15. Service Comparisons

### Console vs. Cloud Shell vs. Local `gcloud`
| Feature | Cloud Console | Cloud Shell | Local `gcloud` SDK |
| :--- | :--- | :--- | :--- |
| **Interface** | Web Graphical UI | In-Browser Terminal | Local Terminal |
| **Installation** | None | None | Requires local install |
| **Persistence** | Session-based state | 5GB persistent `$HOME` | Full local disk storage |
| **Best For** | Visual monitoring | Quick CLI execution | Enterprise CI/CD automation |

### IAM vs. Organization Policies
| Feature | Cloud IAM | Organization Policies |
| :--- | :--- | :--- |
| **Focus** | **WHO** can do what (Identity-based) | **WHAT** can be done (Restriction-based) |
| **Orientation** | Granting access permissions (Allow) | Enforcing compliance guardrails (Deny / Restrict) |
| **Inheritance** | Purely additive (Union across tree) | Can override or restrict further down hierarchy |
| **Example** | Granting Sarah `roles/compute.admin` | Restricting all VMs from having public IP addresses |

### Labels vs. Tags
| Feature | Labels | Tags |
| :--- | :--- | :--- |
| **Primary Purpose** | Cost tracking, billing categorization | Conditional IAM & Org Policy enforcement |
| **Security Enforcement**| Cannot enforce security rules | Can enforce security policies conditionally |
| **Hierarchy Awareness** | Not inherited down hierarchy | Inherited down the Resource Hierarchy |

---

## 16. Best Practices

- Always establish a formal Organization node using Cloud Identity or Google Workspace prior to production deployments.
- Enforce strict environment isolation by separating `Production`, `Staging`, and `Development` into distinct Folders and Projects.
- Attach standard **Labels** (`environment`, `owner`, `cost-center`) to every project and resource.
- Grant administrative roles only at the project level unless organization-wide access is explicitly mandatory.
- Pair Budget threshold alerts with Pub/Sub topics to automate notifications to Slack or monitoring systems.

---

## 17. Common Mistakes

- Assuming that creating a Budget automatically caps cloud spending.
- Trying to change a Project ID after creation (it is permanent).
- Granting IAM roles at the Organization level for employees who only require access to a single project.
- Attempting to revoke an inherited IAM permission at the project level (inherited permissions cannot be restricted by IAM).
- Forgetting that unlinked projects cannot provision paid Google Cloud services.

---

## 18. Quick Revision Sheet

- **Hierarchy Order:** Organization ➔ Folder ➔ Project ➔ Resource.
- **Mnemonic:** **O**h **F**ine, **P**lease **R**elax (**O**rg, **F**older, **P**roject, **R**esource).
- **Identifiers:**
  - **Project Name:** Editable, non-unique display name.
  - **Project ID:** User-chosen, globally unique, permanent string.
  - **Project Number:** Auto-generated, globally unique, permanent integer.
- **Billing Rule:** 1 Billing Account can pay for many Projects. 1 Project links to exactly 1 Billing Account.
- **Budgets Rule:** Alerts only send notifications; they never shut down resources automatically.
- **IAM Inheritance Rule:** Permissions are **additive (Union)** down the hierarchy.
- **Org Policies:** Identity-agnostic guardrails; define *what* actions are allowed across the organization.
- **Cloud Shell:** Free Debian Linux terminal, 5GB persistent storage, pre-authenticated.

---

## 19. Interview Questions

**Q1: What is the exact sequence of the Google Cloud Resource Hierarchy?**  
*Answer:* Organization (root) ➔ Folder (optional, up to 10 levels) ➔ Project (mandatory) ➔ Resource (leaf node).

**Q2: What is the difference between a Project ID and a Project Number?**  
*Answer:* A Project ID is a globally unique, user-chosen string identifier used in APIs and CLI commands. A Project Number is a globally unique, auto-generated numeric identifier assigned by Google for internal orchestration. Both are immutable.

**Q3: Can an IAM permission granted at the Organization level be revoked at a child Project level?**  
*Answer:* No. IAM permissions are purely additive across the resource hierarchy. A user's effective permissions are the union of all permissions granted at parent levels.

**Q4: How do Organization Policies differ from Cloud IAM?**  
*Answer:* IAM controls *who* has access to perform actions (identity-based allowances). Organization Policies enforce *what* actions or configurations are permitted across resources, regardless of the user's IAM permissions (restriction-based guardrails).

**Q5: If a budget limit is reached, does Google Cloud terminate running VMs?**  
*Answer:* No. Budgets and alerts only generate notifications. To shut down resources upon reaching a budget limit, you must configure a Pub/Sub topic and a Cloud Function to automate resource termination.

**Q6: What happens to a deleted Google Cloud project?**  
*Answer:* It enters a 30-day recovery grace period during which it can be restored. After 30 days, the project and all its resources are permanently purged.

**Q7: Can a single Google Cloud Project be linked to two different Billing Accounts simultaneously?**  
*Answer:* No. A project can only be associated with one Billing Account at a time. However, one Billing Account can fund multiple projects.

**Q8: What is the primary difference between Labels and Tags?**  
*Answer:* Labels are key-value pairs used for organizing resources and categorizing billing reports. Tags are hierarchy-aware resources that can be used to conditionally apply Organization Policies and IAM policies.

**Q9: What storage persistence does Cloud Shell offer?**  
*Answer:* Cloud Shell provides 5GB of free persistent disk storage for your `$HOME` directory. Files outside `$HOME` are discarded when the ephemeral VM shuts down.

**Q10: What role is required to manage Organization Policies?**  
*Answer:* `roles/orgpolicy.policyAdmin` (Organization Policy Administrator).

---

## 20. ACE Practice Questions

*(Full practice drill available in [practice-questions/by-phase/phase-01.md](../practice-questions/by-phase/phase-01.md))*

### Question 1 (Resource Hierarchy)
What is the correct order of the Google Cloud resource hierarchy from top to bottom?
- [ ] A. Project ➔ Folder ➔ Organization ➔ Resource
- [x] B. Organization ➔ Folder ➔ Project ➔ Resource
- [ ] C. Folder ➔ Organization ➔ Project ➔ Resource
- [ ] D. Organization ➔ Project ➔ Folder ➔ Resource
*Explanation: Organization is the root node, followed by optional Folders, mandatory Projects, and individual Resources.*

### Question 2 (IAM Inheritance)
A user is granted the `Viewer` role at the Organization node and the `Editor` role on a specific Project. What is their effective access on that project?
- [ ] A. Viewer only
- [x] B. Editor (Union of both roles)
- [ ] C. Access denied due to conflict
- [ ] D. Owner
*Explanation: IAM permissions are additive across the hierarchy; the user receives the union of all granted roles.*

### Question 3 (Budget Automation)
Your company wants to guarantee that cloud spending in a development project stops immediately if it crosses $1,000 this month. What is the Google-recommended approach?
- [ ] A. Set a Budget of $1,000 in the Billing console.
- [ ] B. Set a Budget threshold alert at 100%.
- [x] C. Configure a Budget with Pub/Sub notification to trigger a Cloud Function that disables billing on the project.
- [ ] D. Submit a quota reduction request to $1,000.
*Explanation: Budgets only alert. Automated capping requires a Pub/Sub notification triggering a Cloud Function to disable billing or tear down resources.*

---

## 21. Related Topics

- **Study Before This:** [Phase 00 — Cloud Computing Fundamentals](phase-00-cloud-fundamentals.md).
- **Study After This:** [Phase 02 — Identity & Security (IAM)](phase-02-identity-security-iam.md).
- **Directly Related GCP Services:** Cloud Resource Manager, Cloud Billing, Cloud IAM, Organization Policy Service, Cloud Shell, Cloud SDK.
- **Official ACE Exam Guide Mappings:**
  - *Section 1.1:* Setting up cloud solution environments (Managing projects, assigning billing accounts, establishing resource hierarchy).
  - *Section 1.2:* Planning and configuring compute, data, and network resources.

---

[← Previous Phase: Cloud Fundamentals](phase-00-cloud-fundamentals.md) · [Next Phase: Identity & Security (IAM) →](phase-02-identity-security-iam.md)
