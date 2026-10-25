# Phase 04 Practice Drill - Storage Services

> **Total Questions:** 15 (10 MCQs + 5 MSQs)  
> **Topic Tag:** `#phase-04-storage`  
> **Target Score:** 13/15 (86%+)  
> **Covers:** Cloud Storage (Storage Classes, Lifecycle Rules, Versioning, Retention, Signed URLs, Encryption), Persistent Disk, Local SSD, Filestore.

---

## MCQ Section (Single Correct Answer)

---

### Q1. Your company stores log files in a Cloud Storage bucket. Logs are frequently accessed for the first 30 days, rarely accessed for the next 60 days, and almost never accessed after 90 days. Which lifecycle rule configuration minimizes costs?

- A) Set a single rule to move all objects to Archive after 30 days
- B) Standard → Nearline at 30 days → Coldline at 90 days → Archive at 365 days
- C) Standard → Coldline at 30 days → Archive at 90 days
- D) Store everything in Nearline from the start

<details>
<summary>[Yes] Answer</summary>

**B) Standard → Nearline at 30 days → Coldline at 90 days → Archive at 365 days**

**Explanation:** This follows the natural access pattern - hot data stays in Standard, then transitions progressively to colder classes. Moving directly to Archive too early incurs high retrieval costs if data is accessed. The progressive transition balances storage savings with retrieval costs.

**Trap:** Option C skips Nearline, which means paying Coldline retrieval costs for the 30–90 day period when data is still occasionally accessed.
</details>

---

### Q2. A developer needs to share a private Cloud Storage object with an external contractor who does NOT have a Google account. The contractor needs access for only 24 hours. What is the BEST approach?

- A) Make the object public using `allUsers` IAM binding
- B) Create a service account key and share it with the contractor
- C) Generate a Signed URL with a 24-hour expiry
- D) Add the contractor's email to the bucket's ACL

<details>
<summary>[Yes] Answer</summary>

**C) Generate a Signed URL with a 24-hour expiry**

**Explanation:** Signed URLs provide temporary, time-limited access to a specific object without requiring a Google account. The URL itself contains embedded authentication and automatically expires. Making the object public (A) is a security risk. Sharing a service account key (B) is a severe security anti-pattern. The contractor doesn't have a Google account, so ACL (D) won't work.
</details>

---

### Q3. You need to ensure that financial records stored in Cloud Storage cannot be deleted by anyone - including project owners - for 7 years. What should you do?

- A) Enable Object Versioning
- B) Set a Retention Policy of 7 years and lock it
- C) Use CMEK encryption so only key holders can delete
- D) Remove the `storage.objectAdmin` role from all users

<details>
<summary>[Yes] Answer</summary>

**B) Set a Retention Policy of 7 years and lock it**

**Explanation:** A locked retention policy is permanent and irreversible - even project owners and organization admins cannot delete objects before the retention period expires. Versioning (A) protects against accidental deletes but doesn't prevent intentional deletion. CMEK (C) controls encryption, not deletion. Removing roles (D) can be circumvented by granting the role again.
</details>

---

### Q4. What happens to data stored on a Local SSD when the VM instance is stopped?

- A) Data is preserved until the VM is deleted
- B) Data is automatically backed up to a Persistent Disk
- C) Data is permanently lost
- D) Data is saved to a snapshot

<details>
<summary>[Yes] Answer</summary>

**C) Data is permanently lost**

**Explanation:** Local SSDs are ephemeral storage physically attached to the host machine. Data is permanently lost when the VM is stopped, deleted, reset, or experiences a host maintenance event. For persistent data, use Persistent Disks. Local SSDs are designed for caching, scratch space, and temporary high-IOPS workloads.
</details>

---

### Q5. Which statement about Cloud Storage bucket names is TRUE?

- A) Bucket names are unique within a project
- B) Bucket names can be changed after creation
- C) Bucket names are globally unique across all of Google Cloud
- D) Bucket names can contain uppercase letters

<details>
<summary>[Yes] Answer</summary>

**C) Bucket names are globally unique across all of Google Cloud**

**Explanation:** Cloud Storage bucket names exist in a single global namespace - no two buckets in the world can have the same name. Names cannot be changed after creation (must delete and recreate). Names must be lowercase only (3–63 characters, no spaces or special characters).
</details>

---

### Q6. Your application requires the highest possible disk IOPS for a real-time database running on a Compute Engine VM. You also need the data to survive VM restarts. Which disk type should you use?

- A) Local SSD
- B) Standard Persistent Disk (`pd-standard`)
- C) SSD Persistent Disk (`pd-ssd`)
- D) Balanced Persistent Disk (`pd-balanced`)

<details>
<summary>[Yes] Answer</summary>

**C) SSD Persistent Disk (`pd-ssd`)**

**Explanation:** The requirement specifies both high IOPS AND data persistence (survive restarts). Local SSD (A) has the highest IOPS but data is lost on restart - eliminated. SSD Persistent Disk provides high IOPS with full persistence. Standard (B) has low IOPS, and Balanced (D) has moderate IOPS - neither optimal for a high-performance database.
</details>

---

### Q7. You enable Object Versioning on a bucket and then delete a file called `report.pdf`. What happens?

- A) The file is permanently deleted
- B) A delete marker is placed; the previous version is preserved and retrievable
- C) The file is moved to a "Trash" folder within the bucket
- D) The file is archived to Coldline storage automatically

<details>
<summary>[Yes] Answer</summary>

**B) A delete marker is placed; the previous version is preserved and retrievable**

**Explanation:** With versioning enabled, deleting an object places a "delete marker" as the current version. All previous versions remain stored and accessible by their generation numbers. You can restore any previous version by copying it back. There is no "Trash" concept in Cloud Storage.
</details>

---

### Q8. Which Cloud Storage operation is NOT possible using lifecycle rules?

- A) Transition objects from Standard to Nearline
- B) Transition objects from Coldline to Archive
- C) Transition objects from Archive to Standard
- D) Delete objects older than 365 days

<details>
<summary>[Yes] Answer</summary>

**C) Transition objects from Archive to Standard**

**Explanation:** Lifecycle rules can ONLY transition objects to colder (cheaper storage) classes: Standard → Nearline → Coldline → Archive. You cannot transition backwards (warmer) via lifecycle rules. To change an Archive object back to Standard, you must manually rewrite/copy the object with the new storage class.
</details>

---

### Q9. A Persistent Disk attached to a running VM is 100 GB, but you realize you only need 50 GB. What should you do?

- A) Use `gcloud compute disks resize` to decrease to 50 GB
- B) Create a new 50 GB disk, copy data, detach the old disk, attach the new one
- C) Modify the disk size in the VM instance settings
- D) Delete the disk and recreate with 50 GB

<details>
<summary>[Yes] Answer</summary>

**B) Create a new 50 GB disk, copy data, detach the old disk, attach the new one**

**Explanation:** Persistent disks can only be INCREASED in size, never decreased. The resize command will reject any attempt to shrink a disk. The only way to reduce disk size is to create a new smaller disk, copy the data, and swap them. Option D would cause data loss without copying first.
</details>

---

### Q10. By default, how is data encrypted in Cloud Storage?

- A) Data is NOT encrypted by default - you must enable it manually
- B) Data is encrypted using the customer's own key (CSEK)
- C) Data is encrypted at rest using Google-managed AES-256 encryption keys
- D) Data is encrypted only if stored in Archive class

<details>
<summary>[Yes] Answer</summary>

**C) Data is encrypted at rest using Google-managed AES-256 encryption keys**

**Explanation:** ALL data in Cloud Storage is automatically encrypted at rest using Google-managed encryption keys. No configuration or action is required. CMEK and CSEK are optional upgrades for organizations that want more control over their encryption keys.
</details>

---

## MSQ Section (Multiple Correct Answers - Select ALL that apply)

---

### Q11. Which of the following are valid Cloud Storage location types? (Select THREE)

- A) Region
- B) Zone
- C) Dual-Region
- D) Multi-Region
- E) Global

<details>
<summary>[Yes] Answer</summary>

**A) Region, C) Dual-Region, D) Multi-Region**

**Explanation:** Cloud Storage supports three location types: Region (single region), Dual-Region (two specific regions), and Multi-Region (continent-wide geo-redundancy like US, EU, ASIA). "Zone" (B) is a Compute Engine concept - Cloud Storage doesn't have zone-level granularity. "Global" (E) is not a valid location type.
</details>

---

### Q12. Which statements about Signed URLs are TRUE? (Select THREE)

- A) They provide temporary access to private objects
- B) The accessor does NOT need a Google account
- C) They can be set to never expire
- D) Maximum validity for V4 signing is 7 days
- E) They can grant both read and write access

<details>
<summary>[Yes] Answer</summary>

**A) They provide temporary access to private objects, B) The accessor does NOT need a Google account, D) Maximum validity for V4 signing is 7 days, E) They can grant both read and write access**

**Explanation:** Signed URLs are time-limited URLs that embed credentials for accessing private objects. They work without requiring a Google account. V4 signed URLs have a maximum of 7 days. They can be configured for GET (read) or PUT (write) operations. They CANNOT be set to never expire (C is false).

**Note:** Both D and E are correct - this question has 4 correct answers, making it tricky.
</details>

---

### Q13. Which features differentiate Persistent Disks from Local SSDs? (Select THREE)

- A) Persistent Disks support snapshots; Local SSDs do not
- B) Persistent Disks survive VM stops; Local SSD data is lost
- C) Local SSDs provide higher IOPS than Persistent Disks
- D) Local SSDs can be shared across multiple VMs
- E) Persistent Disks can be resized; Local SSDs have fixed 375 GB per device

<details>
<summary>[Yes] Answer</summary>

**A) Persistent Disks support snapshots; Local SSDs do not, B) Persistent Disks survive VM stops; Local SSD data is lost, C) Local SSDs provide higher IOPS than Persistent Disks, E) Persistent Disks can be resized; Local SSDs have fixed 375 GB per device**

**Explanation:** PDs support snapshots, persist across stops, and can be resized. Local SSDs have no snapshot support, data is ephemeral, provide the highest IOPS, and come in fixed 375 GB increments. Local SSDs CANNOT be shared across VMs (D is false) - they are physically attached to a single host.
</details>

---

### Q14. A bucket has Uniform Bucket-Level Access enabled. Which of the following are TRUE? (Select TWO)

- A) All objects inherit the bucket's IAM policy
- B) Individual objects can still have their own ACLs
- C) After the 90-day lock period, it becomes permanent
- D) It can be freely enabled and disabled at any time
- E) It replaces the need for encryption

<details>
<summary>[Yes] Answer</summary>

**A) All objects inherit the bucket's IAM policy, C) After the 90-day lock period, it becomes permanent**

**Explanation:** Uniform Bucket-Level Access means ALL objects use the bucket-level IAM policy - no individual ACLs (B is false). Once enabled, there's a 90-day grace period to revert; after that, it's permanent and irreversible (D is false). It controls access, not encryption (E is false).
</details>

---

### Q15. Which of the following are valid use cases for Filestore? (Select TWO)

- A) Serving as a boot disk for Compute Engine VMs
- B) Providing shared file storage accessible by multiple VMs via NFS
- C) Storing unstructured data like images and videos at scale
- D) Shared filesystem for GKE pods that need ReadWriteMany access
- E) Archiving data for long-term compliance

<details>
<summary>[Yes] Answer</summary>

**B) Providing shared file storage accessible by multiple VMs via NFS, D) Shared filesystem for GKE pods that need ReadWriteMany access**

**Explanation:** Filestore is a managed NFS service designed for shared file access across multiple clients. It's perfect for multi-VM file sharing and GKE pods needing ReadWriteMany volume access. Boot disks (A) require block storage (Persistent Disks). Unstructured data at scale (C) is better served by Cloud Storage. Long-term archiving (E) should use Cloud Storage Archive class.
</details>

---

## Score Tracker

| Score | Verdict |
|-------|---------|
| 15/15 | Storage Master - You're exam-ready for this domain |
| 13–14 | [Yes] Solid - Minor review needed |
| 10–12 | Note: Re-read Storage Classes, Lifecycle Rules, and PD vs Local SSD |
| Below 10 | Re-study Phase 04 completely, focus on the Gotchas & Traps section |
