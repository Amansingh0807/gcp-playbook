[<- Previous: Phase 03 - Compute Services](phase-03-compute.md) | [-> Next: Phase 05 - Databases & Analytics](phase-05-databases.md)

---

# Phase 4 - Storage Services (Exam Weight: 5/5)

> **Prerequisite:** [Phase 03 - Compute Services](phase-03-compute.md)  
> **Estimated Study Time:** 3–4 Hours  
> **Exam Weight:** (Exam Weight: 5/5) (One of the Most Heavily Tested Domains)  
> **Covers:** Cloud Storage (Buckets, Objects, Naming, Locations, Storage Classes, Versioning, Lifecycle Rules, Retention Policies, Uniform Bucket Access, Signed URLs, Encryption/CMEK), Persistent Disk (Standard/Balanced/SSD/Local SSD), Filestore (awareness).

**Storage is one of the most heavily tested domains in ACE - especially Storage Classes and Lifecycle Rules.**

---

## 1. Definition

**Cloud Storage** is Google's fully managed **object storage** service - a place to store any type of file (images, videos, backups, logs, documents) as "objects" inside containers called "buckets," accessible from anywhere via the internet, at massive scale, without managing any servers.

> **Core Concept:** Cloud Storage acts as an infinitely scalable online object locker. You store unstructured data (media files, backups, logs, archives), and Google guarantees durability, replication, and global HTTP/HTTPS accessibility with zero infrastructure management.

---

## 2. Why does this service exist?

### Problem it solves:
- On-premises storage is **expensive**, requires hardware procurement, capacity planning, and physical maintenance.
- Traditional file servers have **limited capacity** - when they fill up, you must buy more hardware.
- Backup, replication, and disaster recovery require **complex manual configuration**.
- Sharing large files across global teams is **slow and unreliable**.

### Without Cloud Storage:
- Businesses would need to maintain their own storage arrays, RAID configurations, tape backups, and off-site disaster recovery facilities.
- Scaling storage would take weeks/months of procurement cycles.

### How it solves the problem:
- **Unlimited capacity** - store petabytes without worrying about running out of space.
- **99.999999999% (11 nines) durability** - Google replicates your data across multiple facilities.
- **Global accessibility** - any authorized user/application can access files from anywhere via HTTP/HTTPS.
- **Zero maintenance** - no servers to manage, no disks to replace, no firmware to update.

---

## 3. Core Concepts & Terminology

### 3.1 Buckets

A **bucket** is the top-level container in Cloud Storage. Every object (file) must live inside a bucket.

> **Analogy:** Think of a bucket like a root folder, but with one critical distinction: its name must be unique across the entire global Google Cloud namespace, not just within your project.

#### Bucket Naming Rules (Note: Exam Favorite):
| Rule | Detail |
|------|--------|
| **Globally Unique** | No two buckets in the world can have the same name |
| **3–63 characters** | Must be between 3 and 63 characters long |
| **Starts with letter/number** | Cannot start with `goog` prefix |
| **Lowercase only** | No uppercase letters allowed |
| **No spaces or special chars** | Only letters, numbers, hyphens (-), underscores (_), and dots (.) |
| **Cannot be IP format** | Cannot look like an IP address (e.g., 192.168.1.1) |
| **Immutable name** | Once created, a bucket name CANNOT be changed - you must delete and recreate |

> **Exam Trap:** "Can you rename a bucket?" → **NO.** You must create a new bucket with the desired name, copy all objects, then delete the old bucket.

#### Bucket Locations:

| Location Type | What It Means | Example | Durability | Use Case |
|--------------|---------------|---------|------------|----------|
| **Region** | Single data center region | `us-central1` | High (within region) | Low-latency access for localized workloads |
| **Dual-Region** | Two specific regions paired | `us-central1 + us-east1` | Higher (cross-region) | High availability + low latency in two specific areas |
| **Multi-Region** | Automatic geo-redundancy across a continent | `US`, `EU`, `ASIA` | Highest | Global content delivery, maximum availability |

> **Exam Trap:** Bucket location is set at creation and **CANNOT be changed** afterward. To change location, you must create a new bucket in the desired location, copy objects, and delete the old one.

### 3.2 Objects

An **object** is the individual file stored inside a bucket. Each object consists of:

| Component | Description |
|-----------|-------------|
| **Object Data** | The actual file content (bytes) |
| **Object Metadata** | Key-value pairs describing the object (Content-Type, custom metadata, etc.) |
| **Object Name (Key)** | The unique identifier within the bucket (acts like a file path) |
| **Generation Number** | A unique version identifier (changes every time the object is overwritten) |

**Maximum Object Size:** 5 TiB (5 terabytes per single object).

> **Core Concept:** An object represents your raw file bytes paired with descriptor metadata and a unique key that functions as its address within the bucket.

### 3.3 Storage Classes (Note: Most Tested Topic)

Storage classes define the **cost**, **availability**, and **minimum storage duration** for objects.

| Storage Class | Use Case | Min Storage Duration | Availability SLA | Relative Cost (Storage) | Relative Cost (Access) |
|--------------|----------|---------------------|-------------------|------------------------|----------------------|
| **Standard** | Frequently accessed ("hot") data | None | 99.99% (Region), 99.95% (Multi) | Cost: Cost: Cost: Cost: Highest | Cost: Lowest |
| **Nearline** | Accessed less than once per 30 days | **30 days** | 99.9% (Region), 99.0% (Multi) | Cost: Cost: Cost: | Cost: Cost: |
| **Coldline** | Accessed less than once per 90 days | **90 days** | 99.9% (Region), 99.0% (Multi) | Cost: Cost: | Cost: Cost: Cost: |
| **Archive** | Accessed less than once per 365 days | **365 days** | 99.9% (Region), 99.0% (Multi) | Cost: Lowest | Cost: Cost: Cost: Cost: Highest |

#### The Golden Rule of Storage Classes:
> **As storage cost goes DOWN ↓, access/retrieval cost goes UP ↑**

#### Critical Exam Points:
- **Minimum storage duration** means you pay for at least that many days, even if you delete the object earlier.
- **There is NO retrieval delay** - unlike AWS Glacier, ALL Cloud Storage classes provide **millisecond access**. Objects are immediately available.
- You **CAN** change the storage class of an existing object (via rewriting) or set it automatically via **Lifecycle Rules**.
- **Autoclass** - A feature that automatically transitions objects between storage classes based on access patterns.

> **Exam Trap:** "Archive class objects take hours to retrieve" → **FALSE.** Google Cloud Archive storage has NO retrieval delay. Data is available immediately.

### 3.4 Versioning

**Object Versioning** keeps a historical copy of an object every time it is overwritten or deleted.

| Feature | Without Versioning | With Versioning |
|---------|-------------------|-----------------|
| Overwrite | Old data is permanently lost | Old version is preserved with a unique generation number |
| Delete | Object is permanently removed | A "delete marker" is placed; old versions remain accessible |
| Cost | Lower (single copy) | Higher (multiple copies stored) |
| Recovery | Not possible | Retrieve any previous version by generation number |

> **Key Takeaway:** With versioning enabled, every overwrite or deletion retains the previous file copy under a unique generation number, shielding you against accidental loss.

#### Key Commands:
```bash
# Enable versioning on a bucket
gsutil versioning set on gs://my-bucket

# List all versions of an object
gsutil ls -a gs://my-bucket/my-file.txt

# Restore a specific version
gsutil cp gs://my-bucket/my-file.txt#1234567890 gs://my-bucket/my-file.txt
```

### 3.5 Lifecycle Rules (Note: Heavily Tested)

**Lifecycle rules** automatically perform actions on objects based on conditions you define - most commonly, transitioning storage classes or deleting old objects.

#### Common Lifecycle Actions:

| Action | What It Does |
|--------|-------------|
| **SetStorageClass** | Changes the storage class (e.g., Standard → Nearline after 30 days) |
| **Delete** | Permanently removes the object |
| **AbortIncompleteMultipartUpload** | Cleans up failed multipart uploads |

#### Common Lifecycle Conditions:

| Condition | Description |
|-----------|-------------|
| `age` | Number of days since object creation |
| `createdBefore` | Object was created before a specific date |
| `isLive` | Whether the object is the live (current) version |
| `matchesStorageClass` | Current storage class of the object |
| `numNewerVersions` | Number of newer versions that exist |

#### Example Lifecycle Policy:
```json
{
  "lifecycle": {
    "rule": [
      {
        "action": {"type": "SetStorageClass", "storageClass": "NEARLINE"},
        "condition": {"age": 30, "matchesStorageClass": ["STANDARD"]}
      },
      {
        "action": {"type": "SetStorageClass", "storageClass": "COLDLINE"},
        "condition": {"age": 90, "matchesStorageClass": ["NEARLINE"]}
      },
      {
        "action": {"type": "SetStorageClass", "storageClass": "ARCHIVE"},
        "condition": {"age": 365, "matchesStorageClass": ["COLDLINE"]}
      },
      {
        "action": {"type": "Delete"},
        "condition": {"age": 730}
      }
    ]
  }
}
```

> **Exam Trap:** Lifecycle rules can only transition objects to **colder** storage classes (Standard → Nearline → Coldline → Archive). You CANNOT transition backwards (e.g., Archive → Standard) via lifecycle rules - you must manually rewrite the object.

### 3.6 Retention Policies

A **retention policy** specifies a minimum period of time that objects must be stored before they can be deleted or replaced.

| Concept | Description |
|---------|-------------|
| **Retention Period** | Minimum time (in seconds) objects must be retained (e.g., 1 year = 31536000 seconds) |
| **Retention Policy Lock** | Once locked, the policy CANNOT be removed or shortened - only lengthened |
| **Bucket Lock** | Makes the retention policy permanent and irreversible (for compliance) |

> **Compliance Rule:** A retention policy enforces an immutable lock window during which no user - including project owners - can delete or overwrite the data.

> **Exam Trap:** A **locked** retention policy is **PERMANENT** and **IRREVERSIBLE**. Even a project owner or organization admin cannot remove it. The only way to get rid of the data is to wait until the retention period expires.

### 3.7 Uniform Bucket-Level Access

| Access Control Model | How It Works | Recommended? |
|---------------------|--------------|-------------|
| **Fine-Grained (ACLs)** | Per-object access control lists - each object can have different permissions | [No] Legacy - being deprecated |
| **Uniform Bucket-Level Access** | ALL objects in the bucket inherit the bucket's IAM policy - no per-object ACLs | [Yes] **Recommended** (Google best practice) |

> **Exam Trap:** Once Uniform Bucket-Level Access is enabled and the 90-day lock period passes, it becomes **permanent** and cannot be reverted to fine-grained ACLs.

### 3.8 Signed URLs

A **Signed URL** gives time-limited read or write access to a specific object, without requiring the accessor to have a Google account or IAM permissions.

| Feature | Detail |
|---------|--------|
| **Purpose** | Share private objects with external users temporarily |
| **Expiry** | You set a maximum duration (up to 7 days for V4 signing) |
| **Authentication** | The URL itself contains the credentials - anyone with the URL can access |
| **Use Cases** | Shareable download links, temporary upload endpoints, CDN-backed delivery |

```bash
# Generate a signed URL valid for 1 hour
gsutil signurl -d 1h /path/to/service-account-key.json gs://my-bucket/my-file.pdf
```

> **Analogy:** A Signed URL acts like a temporary, time-bound visitor pass. Anyone who holds the URL can read or upload the specified file without needing Google credentials.

### 3.9 Encryption

| Encryption Type | Who Manages Keys | Description |
|----------------|-------------------|-------------|
| **Google-Managed (Default)** | Google | Automatic AES-256 encryption - no configuration needed |
| **CMEK (Customer-Managed Encryption Keys)** | You (via Cloud KMS) | You create and manage the encryption key in Cloud KMS - Google uses YOUR key to encrypt |
| **CSEK (Customer-Supplied Encryption Keys)** | You (completely) | You supply the raw encryption key with every API call - Google never stores it |

> **Exam Trap:** By default, ALL data in Cloud Storage is encrypted at rest using Google-managed keys. You do NOT need to do anything to enable encryption - it's automatic.

---

## 4. Architecture / How It Works

```
┌──────────────────────────────────────────────────────────────────────┐
│                    CLOUD STORAGE ARCHITECTURE                        │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  User/App ──HTTP/HTTPS──► Cloud Storage API                         │
│                                │                                     │
│                       ┌────────┴────────┐                           │
│                       │   BUCKET LAYER  │                           │
│                       │  (Namespace)    │                           │
│                       └────────┬────────┘                           │
│                                │                                     │
│              ┌─────────────────┼─────────────────┐                  │
│              │                 │                   │                  │
│    ┌─────────▼──────┐ ┌───────▼────────┐ ┌───────▼────────┐       │
│    │  Region Store  │ │ Dual-Region    │ │ Multi-Region   │       │
│    │  (us-central1) │ │ (us-c1+us-e1)  │ │ (US/EU/ASIA)   │       │
│    └────────────────┘ └────────────────┘ └────────────────┘       │
│                                                                      │
│    STORAGE CLASSES (per object):                                     │
│    ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐            │
│    │ Standard │→│ Nearline │→│ Coldline │→│ Archive  │            │
│    │ (Hot)    │ │ (30-day) │ │ (90-day) │ │ (365-day)│            │
│    └──────────┘ └──────────┘ └──────────┘ └──────────┘            │
│         ↑                                                            │
│     Lifecycle Rules auto-transition ───────────────────►            │
│                                                                      │
│    SECURITY LAYER:                                                   │
│    ┌──────────────────────────────────────────────┐                 │
│    │ IAM Policies │ Uniform Access │ Signed URLs  │                 │
│    │ Encryption   │ CMEK/CSEK     │ Retention     │                 │
│    └──────────────────────────────────────────────┘                 │
└──────────────────────────────────────────────────────────────────────┘
```

---

## 5. Key Features Summary Table

| Feature | Description | Exam Relevance |
|---------|-------------|----------------|
| **Global Namespace** | Bucket names are globally unique across all of Google Cloud | (Exam Weight: 5/5) |
| **Storage Classes** | Standard, Nearline, Coldline, Archive - cost vs access tradeoff | (Exam Weight: 5/5) |
| **Lifecycle Rules** | Auto-transition or delete objects based on age/conditions | (Exam Weight: 5/5) |
| **Object Versioning** | Keep historical copies of every object version | (Exam Weight: 4/5) |
| **Retention Policies** | Prevent deletion for compliance (lockable) | (Exam Weight: 4/5) |
| **Uniform Bucket Access** | Bucket-level IAM only (no per-object ACLs) | (Exam Weight: 4/5) |
| **Signed URLs** | Temporary, time-limited access for external users | (Exam Weight: 4/5) |
| **CMEK/CSEK** | Customer-managed or customer-supplied encryption keys | (Exam Weight: 3/5) |
| **Object Notifications** | Pub/Sub notifications when objects change | (Exam Weight: 3/5) |
| **Transfer Service** | Bulk transfer from AWS S3, Azure, or on-prem | (Exam Weight: 3/5) |
| **Autoclass** | Automatic storage class management based on access patterns | (Exam Weight: 3/5) |
| **Requester Pays** | Requester pays for data access instead of bucket owner | (Exam Weight: 2/5) |

---

## 6. CLI / Commands Cheat Sheet

### gsutil Commands (Cloud Storage):

```bash
# Create a bucket
gsutil mb -l us-central1 -c standard gs://my-unique-bucket-name

# Upload a file
gsutil cp local-file.txt gs://my-bucket/

# Upload a folder (recursive)
gsutil cp -r ./my-folder gs://my-bucket/

# Download a file
gsutil cp gs://my-bucket/file.txt ./local-directory/

# List bucket contents
gsutil ls gs://my-bucket/

# List all buckets in project
gsutil ls

# Move/Rename an object
gsutil mv gs://my-bucket/old-name.txt gs://my-bucket/new-name.txt

# Delete an object
gsutil rm gs://my-bucket/file.txt

# Delete a bucket (must be empty)
gsutil rb gs://my-bucket

# Delete a bucket and ALL contents (force)
gsutil rm -r gs://my-bucket

# Set storage class for a bucket
gsutil defstorageclass set nearline gs://my-bucket

# Enable versioning
gsutil versioning set on gs://my-bucket

# Set lifecycle rules from JSON file
gsutil lifecycle set lifecycle.json gs://my-bucket

# Get lifecycle rules
gsutil lifecycle get gs://my-bucket

# Set uniform bucket-level access
gsutil uniformbucketlevelaccess set on gs://my-bucket

# Make an object public
gsutil acl ch -u AllUsers:R gs://my-bucket/file.txt

# Generate a signed URL
gsutil signurl -d 1h key.json gs://my-bucket/file.txt

# Get bucket/object metadata
gsutil stat gs://my-bucket/file.txt

# Parallel upload (for large files)
gsutil -m cp -r ./large-folder gs://my-bucket/

# Rsync (mirror a directory)
gsutil rsync -r ./local-dir gs://my-bucket/remote-dir
```

### gcloud Commands (for Persistent Disks):

```bash
# Create a persistent disk
gcloud compute disks create my-disk \
    --size=100GB \
    --type=pd-ssd \
    --zone=us-central1-a

# Attach a disk to a VM
gcloud compute instances attach-disk my-vm \
    --disk=my-disk \
    --zone=us-central1-a

# Detach a disk from a VM
gcloud compute instances detach-disk my-vm \
    --disk=my-disk \
    --zone=us-central1-a

# Resize a disk (can only INCREASE, never decrease)
gcloud compute disks resize my-disk \
    --size=200GB \
    --zone=us-central1-a

# Create a snapshot of a disk
gcloud compute disks snapshot my-disk \
    --snapshot-names=my-snapshot \
    --zone=us-central1-a

# List all disks
gcloud compute disks list

# Delete a disk
gcloud compute disks delete my-disk --zone=us-central1-a
```

---

## 7. Persistent Disks & Local SSDs

### 7.1 Persistent Disk Types

| Disk Type | API Name | Performance | Use Case |
|-----------|----------|-------------|----------|
| **Standard Persistent Disk** | `pd-standard` | Low IOPS, sequential reads | Boot disks, bulk storage, large datasets |
| **Balanced Persistent Disk** | `pd-balanced` | Moderate IOPS, good balance | General-purpose workloads (best default choice) |
| **SSD Persistent Disk** | `pd-ssd` | High IOPS, low latency | Databases, high-performance applications |
| **Extreme Persistent Disk** | `pd-extreme` | Highest IOPS (configurable) | SAP HANA, large databases, enterprise workloads |

### 7.2 Key Persistent Disk Features

| Feature | Detail |
|---------|--------|
| **Zonal Resource** | PDs exist in a specific zone |
| **Regional Persistent Disk** | Replicated across 2 zones in the same region (for HA) |
| **Resize** | Can be increased while attached and running - CANNOT be decreased |
| **Multi-Attach** | PDs can be attached to multiple VMs in READ-ONLY mode |
| **Snapshots** | Incremental backups that can be used to create new disks |
| **Encryption** | Automatically encrypted at rest (Google-managed, CMEK, or CSEK) |
| **Independent Lifecycle** | PDs persist even after the VM is deleted (unless auto-delete is set) |

> **Exam Trap:** Persistent disks can only be **increased** in size, NEVER decreased. If you need a smaller disk, create a new smaller disk and copy data.

### 7.3 Local SSDs

| Feature | Persistent Disk | Local SSD |
|---------|----------------|-----------|
| **Data Persistence** | Survives VM stop/delete | **Data LOST on VM stop, terminate, or maintenance** |
| **Performance** | Good to Excellent | **Extreme** (highest IOPS) |
| **Size** | 10 GB to 64 TB | Fixed 375 GB per device (up to 24 devices = 9 TB) |
| **Snapshots** | [Yes] Supported | [No] NOT supported |
| **Encryption** | [Yes] All types | [Yes] Google-managed only |
| **Use Case** | Permanent storage | Caching, scratch space, temp data, high-performance computing |

> **Exam Trap:** Local SSD data is **EPHEMERAL** - it is permanently lost when the VM stops, is deleted, or undergoes maintenance migration. Never use Local SSD for data you need to keep.

---

## 8. Filestore (NFS)

**Filestore** is a managed **file storage** (NFS) service for applications that need a traditional filesystem interface.

| Feature | Detail |
|---------|--------|
| **Protocol** | NFS (Network File System) |
| **Use Case** | Shared file systems for VMs, GKE, legacy applications |
| **Tiers** | Basic HDD, Basic SSD, High Scale SSD, Enterprise |
| **Managed** | Fully managed - no server OS to maintain |
| **Key Difference** | File storage (hierarchical) vs. Object storage (flat) vs. Block storage (disk) |

> **Analogy:** Filestore functions like a traditional shared network drive (NAS/NFS), allowing multiple VMs and GKE pods to read and write to the same hierarchical file system simultaneously.

---

## 9. Storage Type Comparison (Note: Exam Favorite)

| Feature | Cloud Storage | Persistent Disk | Local SSD | Filestore |
|---------|--------------|-----------------|-----------|-----------|
| **Type** | Object Storage | Block Storage | Block Storage | File Storage |
| **Protocol** | HTTP/HTTPS (REST API) | Attached to VM | Attached to VM | NFS |
| **Persistence** | [Yes] Durable | [Yes] Durable | [No] Ephemeral | [Yes] Durable |
| **Shared Access** | [Yes] Global | Read-only multi-attach | [No] Single VM | [Yes] Multi-VM NFS |
| **Max Size** | Unlimited | 64 TB | 9 TB (24 × 375GB) | Up to 100 TB |
| **Use Case** | Unstructured data, backups, media | VM boot/data disks | Cache, temp, scratch | Shared filesystems |
| **Exam Focus** | (Exam Weight: 5/5) | (Exam Weight: 4/5) | (Exam Weight: 3/5) | (Exam Weight: 2/5) |

---

## 10. IAM & Access Control for Cloud Storage

| Permission Level | Description |
|-----------------|-------------|
| **Storage Admin** (`roles/storage.admin`) | Full control over buckets and objects |
| **Storage Object Admin** (`roles/storage.objectAdmin`) | Full control over objects (but not buckets) |
| **Storage Object Creator** (`roles/storage.objectCreator`) | Can create objects, cannot view/delete existing |
| **Storage Object Viewer** (`roles/storage.objectViewer`) | Read-only access to objects |
| **Storage HMAC Key Admin** (`roles/storage.hmacKeyAdmin`) | Manage HMAC keys for S3-compatible access |

> **Exam Trap:** `Storage Object Creator` can ONLY create/upload objects. It CANNOT list, read, or delete existing objects. This is frequently tested.

---

## 11. Common Exam Scenarios & Solutions

### Scenario 1: Reduce storage costs for infrequently accessed data
**Solution:** Implement Lifecycle Rules to automatically transition objects from Standard → Nearline (30 days) → Coldline (90 days) → Archive (365 days).

### Scenario 2: Protect against accidental deletion
**Solution:** Enable **Object Versioning** on the bucket. Deleted objects will have a delete marker, and previous versions can be restored.

### Scenario 3: Compliance requirement - data must be retained for 7 years
**Solution:** Set a **Retention Policy** with a 7-year period and **lock** the policy. Once locked, no one (not even admins) can delete data before the retention period expires.

### Scenario 4: Share a private file with an external vendor temporarily
**Solution:** Generate a **Signed URL** with a limited expiry (e.g., 24 hours). The vendor can access the file without needing a Google account.

### Scenario 5: VM needs highest possible disk I/O for a database
**Solution:** Use **SSD Persistent Disk** (`pd-ssd`) for persistent high-IOPS storage. If temporary scratch performance is needed, add **Local SSDs** for caching.

### Scenario 6: Multiple VMs need to access the same file system
**Solution:** Use **Filestore** (managed NFS) for shared file access across multiple VM instances.

### Scenario 7: Need to migrate data from AWS S3 to Google Cloud
**Solution:** Use **Storage Transfer Service** to schedule and automate data transfers from S3 to Cloud Storage.

### Scenario 8: Customer requires full control over encryption keys
**Solution:** Use **CMEK** (Customer-Managed Encryption Keys) with Cloud KMS if you want Google to use your key. Use **CSEK** (Customer-Supplied Encryption Keys) if you want to provide the key yourself with every request.

---

## 12. Comparison with Other Cloud Providers

| Feature | Google Cloud Storage | AWS S3 | Azure Blob Storage |
|---------|---------------------|--------|-------------------|
| **Storage Classes** | Standard, Nearline, Coldline, Archive | Standard, IA, One-Zone IA, Glacier, Deep Archive | Hot, Cool, Archive |
| **Archive Retrieval** | **Instant** (milliseconds) | Hours (Glacier) | Hours |
| **Lifecycle Rules** | [Yes] | [Yes] | [Yes] |
| **Object Versioning** | [Yes] | [Yes] | [Yes] (Soft Delete) |
| **Signed URLs** | [Yes] | [Yes] (Pre-signed URLs) | [Yes] (SAS Tokens) |
| **Key Advantage** | Instant archive retrieval, simple pricing | Mature ecosystem | Azure integration |

> **Key GCP Advantage:** Google Cloud Storage Archive class has **NO retrieval delay** - data is available in milliseconds. AWS Glacier takes minutes to hours. This is a frequent exam differentiator.

---

## 13. Best Practices & Production Tips

1. **Always use Uniform Bucket-Level Access** - simpler, more secure, Google-recommended.
2. **Enable Object Versioning** on critical buckets to protect against accidental deletes.
3. **Implement Lifecycle Rules** from day one - don't let Standard-class data sit forever burning money.
4. **Use CMEK** for sensitive data where your organization needs key rotation control.
5. **Set Retention Policies** for compliance-sensitive data (healthcare, finance, government).
6. **Use `gsutil -m`** (parallel mode) for large uploads - dramatically faster than sequential.
7. **Use `gsutil rsync`** instead of `gsutil cp` for ongoing synchronization - only transfers changes.
8. **Prefer Regional storage** for latency-sensitive workloads co-located with Compute/GKE resources.
9. **Use Multi-Region** for globally accessed content like static websites, CDN origins, and public datasets.
10. **Monitor with Cloud Monitoring** - set alerts on bucket size, request rates, and error rates.

---

## 14. Gotchas & Traps (Exam-Critical)

| Trap | Truth |
|------|-------|
| [No] "You can rename a bucket" | [Yes] **NO** - must create new, copy, delete old |
| [No] "You can change bucket location" | [Yes] **NO** - location is permanent at creation |
| [No] "Archive class has retrieval delay like AWS Glacier" | [Yes] **NO** - GCS Archive has instant (millisecond) retrieval |
| [No] "Lifecycle rules can transition from Coldline to Standard" | [Yes] **NO** - lifecycle only goes colder, never warmer |
| [No] "Persistent disks can be decreased in size" | [Yes] **NO** - only increased, never decreased |
| [No] "Local SSD data persists after VM stop" | [Yes] **NO** - data is lost on stop/delete/maintenance |
| [No] "Locked retention policy can be removed by admin" | [Yes] **NO** - locked is permanent and irreversible |
| [No] "Storage Object Creator can read existing objects" | [Yes] **NO** - can only create/upload, not read/list/delete |
| [No] "Object names must be globally unique" | [Yes] **NO** - only bucket names are globally unique; object names are unique within a bucket |

---

## 15. Quick Revision Card

```
┌─────────────────────────────────────────────────────┐
│         PHASE 04 STORAGE QUICK REVISION          │
├─────────────────────────────────────────────────────┤
│                                                     │
│  CLOUD STORAGE = Object Storage (HTTP/HTTPS)        │
│  PERSISTENT DISK = Block Storage (attach to VM)     │
│  LOCAL SSD = Ephemeral Block (highest IOPS)         │
│  FILESTORE = File Storage (NFS, shared access)      │
│                                                     │
│  STORAGE CLASSES (hot → cold):                      │
│  Standard → Nearline (30d) → Coldline (90d) →       │
│  Archive (365d)                                     │
│                                                     │
│  GOLDEN RULE: Storage cost ↓ = Access cost ↑        │
│  ALL CLASSES: Instant retrieval (no delay!)          │
│                                                     │
│  LIFECYCLE: Only transitions COLDER, never warmer   │
│  VERSIONING: Keeps all previous versions            │
│  RETENTION: Lock = permanent, irreversible          │
│  SIGNED URL: Temporary access, max 7 days (V4)      │
│                                                     │
│  BUCKET: Globally unique, immutable name & location │
│  PD: Resize UP only, never DOWN                     │
│  LOCAL SSD: Data LOST on stop/delete/maintenance    │
│                                                     │
│  ENCRYPTION: Default = Google-managed AES-256       │
│  CMEK = Your key in KMS  |  CSEK = Your raw key    │
│                                                     │
│  KEY ROLES:                                         │
│  storage.admin → Full bucket + object control       │
│  storage.objectAdmin → Full object control          │
│  storage.objectCreator → Upload ONLY (can't read)   │
│  storage.objectViewer → Read ONLY                   │
└─────────────────────────────────────────────────────┘
```

---

## 16. Navigation & What's Next

[<- Previous: Phase 03 - Compute Services](phase-03-compute.md) | [-> Next: Phase 05 - Databases & Analytics](phase-05-databases.md)

---

*Last Updated: October 2026*  
*Phase Status: [Yes] Complete*
