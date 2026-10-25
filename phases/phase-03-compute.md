[<- Previous: Phase 02 - Identity & Security (IAM)](phase-02-identity-security-iam.md) | [-> Next: Phase 04 - Storage Services](phase-04-storage.md)

---

# Phase 3 - Compute Services (Exam Weight: 5/5)

> **Prerequisite:** [Phase 02 - Identity & Security (Cloud IAM)](phase-02-identity-security-iam.md)  
> **Estimated Study Time:** 5–6 Hours  
> **Exam Weight:** (Exam Weight: 5/5) (GCP's Biggest & Most Heavily Tested Compute Domain)  
> **Covers:** Compute Engine (VM Basics, Machine Types, Storage, Images/Snapshots, Networking, Lifecycle, Metadata, Access, Pricing), Managed Instance Groups (Templates, Autoscaling, Autohealing, Rolling Updates, Health Checks), App Engine (Standard/Flexible, Versions, Traffic Splitting), Cloud Run (Containers, Revisions, Concurrency), Cloud Functions (Event-driven, Triggers).

---

## 1. Definition

**Compute** in GCP is the overarching umbrella term for all the services that execute your application code - ranging from full virtual machines that you completely control (Compute Engine) to fully managed, serverless platforms where you simply deploy code or containers without managing infrastructure (App Engine, Cloud Run, Cloud Functions).

> **Core Concept:** Compute answers the question: **"Where and how does your application execute?"** Compute Engine gives you complete low-level control over OS, memory, and disks, while serverless offerings like Cloud Run and Cloud Functions abstract away infrastructure entirely so you pay only for code execution time.

---

## 2. Why does this service exist?

### Problem it solves:
- Applications possess diverse operational and architectural needs. A monolithic legacy enterprise app cannot run inside a 60-second serverless function, and a simple web-hook listener does not justify paying 24/7 for a dedicated Linux VM.
- GCP provides a **spectrum of control vs. convenience** so you can pick the exact compute runtime fitting your operational overhead tolerance and cost budget.

### Why it was created:
- A "one-size-fits-all" compute paradigm fails in production:
  - **Full Control:** Legacy software, specific OS kernels, custom network interfaces -> **Compute Engine**.
  - **Scalable VM Fleets:** Identical VMs with auto-healing and rolling updates -> **Managed Instance Groups (MIGs)**.
  - **Containerized Serverless:** Stateless microservices and APIs -> **Cloud Run**.
  - **PaaS Web Applications:** Monolithic web frameworks with rapid traffic splitting -> **App Engine**.
  - **Event-Driven Micro-logic:** Reacting to cloud storage uploads or Pub/Sub queues -> **Cloud Functions**.

> [!IMPORTANT]
> **Remember for ACE Exam (Exam Weight: 5/5):** The exam tests your ability to match a business scenario to the exact right compute service on the spectrum based on operational overhead, cost, and architecture constraints.

---

## 3. Architecture / Working

### 3.1 The GCP Compute Spectrum
```
MOST CONTROL                                                      LEAST CONTROL
MOST MANAGEMENT                                                LEAST MANAGEMENT
     │                                                                │
Compute Engine   ─-->   GKE (Kubernetes)   ─-->   Cloud Run   ─-->   Cloud Functions
  (Raw VMs, OS,          (Orchestrated             (Stateless          (Single Event
   full control)          Containers)               Containers)          Functions)
  
                    App Engine PaaS Layer:
                    ├── App Engine Flexible (Custom runtimes in Docker, VM-based)
                    └── App Engine Standard (Sandboxed, fast scaling, scales to zero)
```

---

### 3.2 Compute Engine VM Request Lifecycle
```
[User: Console / gcloud CLI / Terraform]
                   │
                   │ (1) Specifies: machine-type, OS image, disks, VPC subnet, zone
                   ▼
         [Compute Engine API]
                   │
                   │ (2) Evaluates IAM permissions & verifies regional quota
                   ▼
     [Resource Manager Host Placement]
                   │
                   │ (3) Identifies physical host rack; KVM Hypervisor provisions VM slice
                   ▼
        [Disk & Network Binding]
                   │
                   │ (4) Attaches Persistent Disk; assigns internal/external IP
                   ▼
       [Boot & Metadata Execution]
                   │
                   │ (5) Boots guest OS image; executes startup scripts from Metadata Server
                   ▼
             [VM is RUNNING]
```

---

### 3.3 VM Lifecycle State Machine
```
   [PROVISIONING] --> [STAGING] --> [RUNNING] --> [STOPPING] --> [TERMINATED]
                                       │
                                (Suspend) --> [SUSPENDED] --> (Resume) --> [RUNNING]
                                       │
                                  (Reset) --> Reboots guest OS, stays RUNNING
                                       │
                                  (Delete) --> Permanently destroyed
```

- **Start:** Boots a stopped instance (`TERMINATED` -> `RUNNING`).
- **Stop:** Halts compute execution. **Compute billing stops, but attached Persistent Disks continue to accrue storage charges.**
- **Suspend:** Pauses the VM, writing RAM contents to disk (faster resume than a full cold boot). Storage for memory image is billed.
- **Reset:** Hard reset (similar to holding the physical power button). Memory is wiped; ephemeral IP and disk attachments remain intact.
- **Delete:** Permanently destroys the VM. Boot disk is deleted by default unless "Keep boot disk" was enabled.

> **Lifecycle State Summary:**  
> - **Stop:** Powers down CPU/RAM; compute billing halts, but attached Persistent Disks continue to accrue storage charges.  
> - **Suspend:** Hibernates the instance by persisting RAM contents to disk, allowing near-instantaneous resume.  
> - **Reset:** Hard reboot of the guest OS; memory is cleared, but disks and ephemeral IP remain attached.  
> - **Delete:** Permanently terminates the VM and cleans up unpreserved resources.

---

### 3.4 Managed Instance Group (MIG) Architecture & Autoscaling
```
[Instance Template]  (Immutable blueprint: machine type, OS image, startup script, tags)
         │
         ▼ creates
[Managed Instance Group (MIG)]  (Fleet of identical, homogenous VMs)
         │
         ├─--> [Autoscaler]  (Watches CPU, Load Balancer traffic, or Cloud Monitoring metrics)
         │        ├── Load spikes -> Automatically provisions new VM instances (Scale Out)
         │        └── Load drops  -> Safely terminates excess instances (Scale In)
         │
         └─--> [Health Checker] (Pings HTTP/TCP endpoints periodically)
                  └── Instance unhealthy -> Autohealing automatically deletes & recreates it
```

---

## 4. Core Components

### 4.1 Compute Engine Virtual Machine Basics
| Component | What it is | Why it exists | Real-World Analogy |
| :--- | :--- | :--- | :--- |
| **Virtual Machine (VM)** | Software-based emulation of physical computing hardware | Allows multiple isolated operating environments to run on shared hardware | An individual apartment in a residential tower |
| **Virtualization** | Technology decoupling software from physical servers | Maximizes physical server utilization and resource pooling | Dividing a large land parcel into multi-tenant units |
| **Hypervisor (KVM)** | Low-level software layer abstracting physical CPU/RAM | Provisions and governs virtual hardware slices safely | The building structure and walls dividing apartments |

---

### 4.2 Machine Types & Architectural Families
| Family | Architectural Profile | Target Workloads | Example Types |
| :--- | :--- | :--- | :--- |
| **E2 (Cost-Optimized)** | General-purpose, shared-core burstable or standard | Dev/test, lightweight web servers, microservices | `e2-micro`, `e2-medium`, `e2-standard-4` |
| **N2 / N2D (Balanced)** | High performance, balanced CPU-to-memory ratio | Enterprise databases, web backends, caching | `n2-standard-8`, `n2d-highmem-16` |
| **C2 / C2D (Compute)** | Ultra-high single-thread performance, compute-heavy | AAA gaming servers, high-performance computing (HPC) | `c2-standard-16`, `c2d-standard-8` |
| **M2 / M3 (Memory)** | Massive RAM capacity (up to multiple terabytes) | In-memory databases (SAP HANA), large data caches | `m2-ultramem-416`, `m3-megamem-128` |
| **A2 / G2 (Accelerator)** | NVIDIA GPU-accelerated (A100, L4) | Deep learning training, LLM inference, 3D rendering | `a2-highgpu-1g`, `g2-standard-8` |
| **Custom Machine Types** | Tailored vCPU and RAM combinations defined by you | Workloads that don't match fixed ratios; eliminates idle RAM | Custom `4 vCPU / 22 GB RAM` |

---

### 4.3 VM Disk & Storage Subsystems
| Disk Type | Underpinning | Latency / IOPS | Data Durability Behavior | Best For |
| :--- | :--- | :--- | :--- | :--- |
| **Standard PD (`pd-standard`)** | Standard HDD | Low IOPS | Durable; survives VM stop & delete | Sequential logs, big data backups |
| **Balanced PD (`pd-balanced`)** | Mainstream SSD | Balanced IOPS | Durable; survives VM stop & delete | Default choice for standard workloads |
| **SSD PD (`pd-ssd`)** | High-performance SSD | High IOPS | Durable; survives VM stop & delete | High-transaction databases, MySQL, PG |
| **Local SSD** | NVMe physically in host server | Sub-millisecond, Extreme IOPS | **EPHEMERAL! Data is completely lost on VM Stop/Terminate** | L1 cache, scratch disks, swap space |

> [!CAUTION]
> **Number One Compute Exam Trap:** **Local SSD data is strictly EPHEMERAL.** If a VM is stopped, terminated, or undergoes a non-live-migration hardware event, all Local SSD data is erased permanently. Persistent Disk (PD) data is network-attached and persists across VM stop/start cycles.

---

### 4.4 Networking for Virtual Machines
| IP Type | Persistence | Public Internet Reachable? | Billing Behavior |
| :--- | :--- | :--- | :--- |
| **Internal IP** | Retained while VM exists in subnet | No (VPC internal only) | Free |
| **Ephemeral External IP** | Changes whenever the VM stops and restarts | Yes | Free while attached to a running instance |
| **Static External IP** | Reserved permanently; does not change on VM reboot | Yes | **Charged if unattached! Free while attached to running VM** |

---

### 4.5 VM Metadata & Access Methods
- **Metadata Server (`http://metadata.google.internal/computeMetadata/v1/`):** Internal HTTP endpoint queryable only by processes inside the VM to securely fetch instance attributes, project metadata, and temporary OAuth2 Service Account tokens.
- **Startup Script (`startup-script`):** Script executed automatically as root every time the VM boots up. Used to install software, mount disks, and pull code.
- **Shutdown Script (`shutdown-script`):** Script executed when a VM is commanded to halt. Allows a 90-second window for graceful connection draining and log flushing.
- **OS Login:** Google Cloud feature mapping SSH user access directly to Google Cloud IAM permissions (`roles/compute.osLogin` or `roles/compute.osAdminLogin`), completely replacing manual SSH key management.

---

## 5. Key Features

### 5.1 Managed Instance Groups (MIGs)
- **Instance Template:** An **immutable** configuration blueprint (machine type, boot disk image, network tags, startup scripts). To modify a MIG, you must create a new Instance Template version.
- **Autoscaling:** Adds or removes instances based on CPU utilization, Cloud Monitoring metrics, or Cloud Load Balancing capacity.
- **Autohealing:** Uses an application-level Health Check (HTTP/TCP) to evaluate VM responsiveness. If a VM fails the check, the MIG destroys and recreates that specific VM automatically.
- **Rolling Updates:** Gradually replaces old instances with instances created from a new Instance Template, honoring `maxSurge` and `maxUnavailable` limits to eliminate downtime.
- **Regional MIG vs. Zonal MIG:** Regional MIGs distribute identical VMs across three separate zones in a region, protecting the application against entire data center outages.

---

### 5.2 App Engine (Standard vs. Flexible)
- **Standard Environment:** Executes code inside Google-managed container sandboxes with specific language runtimes (Python, Node, Java, Go).
  - Rapid scale-to-zero in seconds.
  - No OS access or SSH capability.
  - Billed strictly per instance-hour when active.
- **Flexible Environment:** Executes code inside Docker containers on Compute Engine VMs managed by Google.
  - Supports any custom runtime or native library.
  - **Does NOT scale to zero** (maintains a baseline of at least 1 running VM).
  - Slower scaling (minutes to boot new VMs).
- **Traffic Splitting:** Allows splitting incoming user traffic between application versions by IP address, cookie, or random weight (e.g., 90% to v1, 10% to v2 for canary testing).

---

### 5.3 Cloud Run
- Fully managed serverless container execution platform.
- **True Scale-to-Zero:** Scales down to zero instances when idle; bills only per 100ms of CPU/RAM during request processing.
- **Revisions:** Every deployment creates an immutable new revision. Traffic can be split across revisions for canary releases.
- **Concurrency:** Allows a single container instance to handle multiple simultaneous requests (default: 80, configurable up to 1000), drastically reducing cold starts and container count.

---

### 5.4 Cloud Functions (FaaS)
- Lightweight, event-driven serverless compute.
- Invocation triggers:
  - **HTTP:** Direct REST endpoint call.
  - **Cloud Storage:** Triggers when objects are created, finalized, or deleted in a bucket.
  - **Cloud Pub/Sub:** Triggers when messages arrive in an asynchronous topic.
- Scales automatically per request; scales to zero.

---

## 6. Types / Variants

### 6.1 Compute Service Comparison Matrix
| Dimension | Compute Engine | MIGs | App Engine Standard | App Engine Flexible | Cloud Run | Cloud Functions |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Model** | IaaS | IaaS+ | PaaS | PaaS (Docker) | Serverless CaaS | Serverless FaaS |
| **Unit of Scale** | Single VM | Fleet of VMs | Sandbox App | Container on VM | Container Image | Single Function |
| **Scales to Zero?** | [No] No | [No] No | [Yes] Yes | [No] No | [Yes] Yes | [Yes] Yes |
| **Custom OS/Kernel**| [Yes] Yes | [Yes] Yes | [No] No | [No] No | [No] No | [No] No |
| **Startup Speed** | ~30–60 sec | ~30–60 sec | Seconds | ~3–5 minutes | ~1–3 sec | Milliseconds |
| **Best For** | Legacy, stateful | High-traffic web tier | Simple web apps | Custom runtimes | Microservices, APIs | Event processing |

---

### 6.2 Compute Engine Pricing & Discount Models
| Model | Description | Discount Percentage | Interruption Risk | Best Workloads |
| :--- | :--- | :--- | :--- | :--- |
| **On-Demand** | Standard pay-as-you-go per second | None (Baseline) | None | Unpredictable, short-lived jobs |
| **Spot VMs** | Google's surplus computing capacity | **60% to 91% discount** | High (Can be preempted anytime) | Batch processing, rendering, CI/CD workers |
| **Sustained Use (SUD)**| Applied automatically when VMs run >25% of month | Up to 30% discount | None (Automatic) | Steady-state VMs without commitments |
| **Committed Use (CUD)**| Contractual commitment for 1 or 3 years | Up to 57% (Compute) / 70% (RAM)| None | Predictable, baseline enterprise workloads |

> **Analogy:** Think of Spot VMs like standby airline seating: offered at steep 60%–91% discounts, but subject to immediate forfeiture (preemption) whenever regular full-fare demand surges.

---

## 7. Configuration & Important Settings

### Key Compute Engine VM Parameters
- **Zone:** Geographic data center deployment point (e.g., `asia-south1-a`).
- **Machine Configuration:** Selecting series (E2, N2) and core/memory sizes.
- **Boot Disk Options:** Deletion rule - check *Keep boot disk on instance deletion* for stateful production systems.
- **Network Tags:** Metadata labels (`http-server`, `backend`) used by VPC Firewall rules to target ingress traffic.

### Crucial MIG & Autoscaler Settings
- **Target CPU Utilization:** Target percentage (e.g., 60%) triggering scale-out events.
- **Cool-Down Period:** Grace period (default: 60s) allowing newly booted VMs to finish starting up before the autoscaler evaluates metrics again.
- **Minimum & Maximum Replicas:** Lower and upper capacity bounds.

---

## 8. Pricing & Cost Considerations

- **Per-Second Billing:** Linux VMs are billed per second with a 1-minute minimum.
- **Stopped VM Billing:** When a VM is `STOPPED`, vCPU and memory charges cease, but **Persistent Disks, reserved static IPs, and snapshots continue to be billed**.
- **Serverless Savings:** For bursty or intermittent workloads, Cloud Run and Cloud Functions cost $0 while waiting for requests.

> [!WARNING]
> **ACE Billing Trap:** Reserving a static external IP and forgetting to attach it to a running VM incurs a continuous hourly charge. Always release unattached static IPs (`gcloud compute addresses delete`).

---

## 9. Security Considerations

- **Disable Default Service Account:** Compute Engine's default service account has historically held broad `Editor` rights. Always configure a custom, least-privilege service account.
- **Enable OS Login:** Replaces manual, fragmented public SSH keys stored in project metadata with centralized IAM role evaluation (`roles/compute.osLogin`).
- **Shielded VMs:** Enables Secure Boot, Virtual Trusted Platform Module (vTPM), and integrity monitoring to defend against boot-level rootkits.
- **Use Private Google Access & Bastions:** Keep VMs on private internal IPs; route administrator access via **Identity-Aware Proxy (IAP)** rather than exposing port 22 directly to the public internet.

---

## 10. Advantages

1. **Precision Sizing:** Custom Machine Types allow dialling in exact vCPU and RAM allocations, preventing over-provisioning costs.
2. **Extreme Resilience:** Autohealing MIGs self-detect crashed instances and recover without human intervention.
3. **Instant Scalability:** Cloud Run and App Engine Standard scale up to handle flash surges and scale back down to zero automatically.
4. **Massive Cost Reduction:** Spot VMs deliver up to 91% cost savings for stateless batch processing.

---

## 11. Limitations

1. **Management Burden on IaaS:** Compute Engine requires manual OS security patching, antivirus, and kernel maintenance.
2. **Local SSD Volatility:** Erased completely upon VM shutdown.
3. **Cold Start Latency:** Serverless runtimes (Cloud Run, Cloud Functions) can encounter latency spikes when scaling up from zero instances.

---

## 12. Real-World Use Cases

1. **Flash Sale E-Commerce:** Regional Managed Instance Group behind an External HTTPS Load Balancer autoscaling from 4 to 200 instances during peak shopping hours.
2. **Asynchronous Video Encoding:** Batch processing cluster utilizing Spot VMs in a MIG, reading from Cloud Storage and saving encoded files back.
3. **Event-Driven Image Resizing:** Cloud Function triggered immediately upon image upload to a Cloud Storage bucket.
4. **Containerized REST Microservices:** Enterprise APIs deployed to Cloud Run, scaling to zero overnight to eliminate idle infrastructure costs.
5. **Legacy ERP Database:** High-memory M2 virtual machine running an in-memory SAP HANA database with SSD Persistent Disks.

---

## 13. Hands-on (Console + CLI)

### Console Walkthrough
1. **Create VM:** Navigation Menu -> **Compute Engine** -> **VM Instances** -> Click **Create Instance** -> Set Name, Region, Machine Type -> Check *Allow HTTP traffic* -> Click **Create**.
2. **Create Instance Template:** **Compute Engine** -> **Instance Templates** -> **Create Instance Template** -> Define OS image, startup script, and tags.
3. **Create MIG:** **Compute Engine** -> **Instance Groups** -> **Create Instance Group** -> Select template, set autoscaling CPU target to 60%, configure health check.

### CLI (`gcloud`) Master Commands
```bash
# 1. Create a Compute Engine VM with a specific machine type and boot disk
gcloud compute instances create prod-web-vm \
  --zone=asia-south1-a \
  --machine-type=e2-medium \
  --image-family=debian-12 \
  --image-project=debian-cloud \
  --tags=http-server

# 2. Stop and start a VM
gcloud compute instances stop prod-web-vm --zone=asia-south1-a
gcloud compute instances start prod-web-vm --zone=asia-south1-a

# 3. SSH into a VM using OS Login
gcloud compute ssh prod-web-vm --zone=asia-south1-a

# 4. Create an Instance Template for a MIG
gcloud compute instance-templates create web-template \
  --machine-type=e2-medium \
  --image-family=debian-12 \
  --image-project=debian-cloud \
  --metadata=startup-script="#!/bin/bash
apt-get update && apt-get install -y nginx"

# 5. Create a Managed Instance Group (MIG) with autoscaling
gcloud compute instance-groups managed create web-mig \
  --base-instance-name=web-srv \
  --template=web-template \
  --size=2 \
  --zone=asia-south1-a

gcloud compute instance-groups managed set-autoscaling web-mig \
  --zone=asia-south1-a \
  --min-num-replicas=2 \
  --max-num-replicas=10 \
  --target-cpu-utilization=0.60 \
  --cool-down-period=60

# 6. Deploy a container directly to Cloud Run
gcloud run deploy customer-api \
  --image=gcr.io/[PROJECT_ID]/api:v1 \
  --region=asia-south1 \
  --allow-unauthenticated \
  --min-instances=0 \
  --max-instances=15 \
  --concurrency=80

# 7. Deploy an HTTP-triggered Cloud Function (Gen 2)
gcloud functions deploy process-order \
  --gen2 \
  --runtime=python312 \
  --trigger-http \
  --entry-point=handler \
  --region=asia-south1
```

---

## 14. ACE Exam Focus (Exam Weight: 5/5)

### Top Tested Concepts:
- **Local SSD vs. Persistent Disk:** Local SSD data does **not** survive stopping a VM.
- **Spot VMs:** Up to 91% discount, can be terminated at any time by Google, ideal for stateless fault-tolerant batch workloads.
- **Scale to Zero:** Cloud Run, Cloud Functions, and App Engine Standard scale to zero. App Engine Flexible and Compute Engine do **not**.
- **Autohealing Mechanics:** Requires an **HTTP/TCP Health Check** bound to the Managed Instance Group.

---

## 15. Service Comparisons

### Compute Engine vs. Cloud Run vs. Cloud Functions
| Dimension | Compute Engine | Cloud Run | Cloud Functions |
| :--- | :--- | :--- | :--- |
| **Abstraction** | IaaS (Virtual Machine) | Serverless CaaS (Containers) | Serverless FaaS (Functions) |
| **Idle Cost** | Continuous 24/7 billing | **$0 when idle (Scale to zero)** | **$0 when idle (Scale to zero)** |
| **Scaling Control** | Manual or MIG Autoscaler | Automatic per request | Automatic per event |
| **OS Maintenance** | Customer manages and patches | Fully managed by Google | Fully managed by Google |

### Persistent Disk vs. Local SSD
| Feature | Persistent Disk (PD) | Local SSD |
| :--- | :--- | :--- |
| **Durability** | High (Network-attached, survives VM stops) | **Ephemeral (Wiped clean on VM Stop/Terminate)** |
| **Performance** | Good (Up to 100k IOPS on Extreme/Hyperdisk) | **Sub-millisecond latency, extreme IOPS** |
| **Resizable** | Yes (Can expand live without downtime) | No (Fixed 375 GB increments) |

---

## 16. Best Practices

- Prefer **Cloud Run** or **Cloud Functions** for all new microservices to eliminate OS management and enable scale-to-zero.
- Always use **Managed Instance Groups (MIGs)** with **Autohealing** for production web backends rather than standalone individual VMs.
- Leverage **Spot VMs** for container builds, batch rendering, and fault-tolerant queue workers.
- Attach custom user-managed Service Accounts with least privilege; never deploy VMs with default Editor credentials.
- Enable **OS Login** across the organization to standardize secure SSH access via Cloud IAM.

---

## 17. Common Mistakes

- Placing production database files on a **Local SSD** and losing all data when the VM is stopped.
- Choosing **App Engine Flexible** for an app with long idle periods, unaware that it does not scale to zero.
- Forgetting that stopping a VM does **not** stop storage charges for attached Persistent Disks.
- Leaving reserved static external IP addresses unattached to running instances.
- Forgetting to configure a **Health Check** on a MIG, leaving autohealing unable to detect application crashes.

---

## 18. Quick Revision Sheet

- **Compute Hierarchy:** Compute Engine (IaaS) -> GKE (Containers) -> Cloud Run (Serverless CaaS) -> Cloud Functions (Serverless FaaS).
- **Mnemonic:** **C**ars **M**ove **A**pp **R**oads **F**ast (**C**ompute Engine, **M**IG, **A**pp Engine, Cloud **R**un, Cloud **F**unctions).
- **Local SSD Golden Rule:** Fast, physically attached, but **EPHEMERAL** (data lost on VM stop).
- **Spot VM Rule:** 60–91% discount, can be preempted anytime, perfect for batch/stateless workloads.
- **Scale to Zero:** Cloud Run, Functions, App Engine Standard = YES. App Engine Flexible, Compute Engine = NO.
- **MIG Architecture:** Instance Template (Immutable blueprint) -> MIG (Group) -> Autoscaler (Metric-based) -> Health Check (Autohealing).

---

## 19. Interview Questions

**Q1: What happens to a VM's data when it is stopped versus suspended?**  
*Answer:* When stopped, the VM halts execution; disk data persists, but volatile RAM is cleared. When suspended, the instance's active RAM state is preserved to persistent storage, allowing rapid restoration to the exact previous running state.

**Q2: Why is Local SSD unsuitable for storing transactional database files?**  
*Answer:* Local SSDs are physically tied to the host hypervisor rack. If the instance is stopped or terminated, the data on the Local SSD is permanently erased.

**Q3: How does Cloud Run achieve cost savings compared to Compute Engine for bursty APIs?**  
*Answer:* Cloud Run scales down to zero instances during idle periods, billing $0. Compute Engine instances continue incurring hourly compute charges 24/7 regardless of traffic volume.

**Q4: What is the function of an Instance Template in a Managed Instance Group?**  
*Answer:* An Instance Template is an immutable specification defining machine configuration, boot disk image, network tags, and startup scripts used to ensure all instances within the MIG are completely identical.

**Q5: What is the difference between App Engine Standard and App Engine Flexible?**  
*Answer:* Standard runs within Google-managed sandboxes with fast scaling and scale-to-zero capabilities. Flexible runs custom Docker containers on Compute Engine VMs, supporting custom libraries but without rapid scale-to-zero.

---

## 20. ACE Practice Questions

*(Full practice drill available in [practice-questions/by-phase/phase-03.md](../practice-questions/by-phase/phase-03.md))*

### Question 1 (Compute Selection)
A company runs a stateless REST API with highly variable traffic, including hours of zero activity. They want to minimize costs and operational management. Which service should they choose?
- [ ] A. Compute Engine with a fixed number of VMs
- [ ] B. App Engine Flexible
- [x] C. Cloud Run
- [ ] D. Managed Instance Group with a fixed size
*Explanation: Cloud Run provides true scale-to-zero and pay-per-use billing with zero server management overhead.*

### Question 2 (Storage Ephemerality)
What happens to data stored on a Local SSD when the parent Compute Engine VM is stopped?
- [ ] A. It is backed up to Cloud Storage automatically.
- [ ] B. It persists and is available upon reboot.
- [x] C. It is permanently lost.
- [ ] D. It is converted to a Persistent Disk.
*Explanation: Local SSD storage is physically attached to the server host and is completely wiped when a VM is stopped.*

---

## 21. Related Topics & Navigation

- **Study Before This:** [Phase 02 - Identity & Security (Cloud IAM)](phase-02-identity-security-iam.md).
- **Study After This:** [Phase 04 - Storage Services](phase-04-storage.md) & [Phase 06 - Networking](phase-06-networking.md).
- **Directly Related GCP Services:** Compute Engine, Managed Instance Groups, Cloud Run, Cloud Functions, App Engine, GKE, Cloud Load Balancing.
- **Official ACE Exam Guide Mappings:**
  - *Section 2.1:* Planning and configuring compute resources (sizing, Spot VMs, selecting runtimes).
  - *Section 3.1:* Deploying and implementing Compute Engine resources.
  - *Section 3.3:* Deploying Cloud Run and Cloud Functions resources.

---

[<- Previous: Phase 02 - Identity & Security (IAM)](phase-02-identity-security-iam.md) | [-> Next: Phase 04 - Storage Services](phase-04-storage.md)
