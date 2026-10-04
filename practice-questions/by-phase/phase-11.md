# Phase 11 Practice Drill — High Availability & Reliability

> **Total Questions:** 15 (10 MCQs + 5 MSQs)  
> **Topic Tag:** `#phase-11-high-availability-reliability`  
> **Target Score:** 13/15 (86%+)  
> **Covers:** Availability Zones, Regional Resources, Zonal Resources, Backups, Disaster Recovery (RTO & RPO), High Availability (HA) vs. Fault Tolerance (FT), Synchronous vs. Asynchronous Replication, Regional MIGs, Regional Persistent Disks, Cloud SQL HA vs. Cross-Region Read Replicas.

---

## MCQ Section (Single Correct Answer)

---

### Q1. A production transactional database requires automatic failover to a standby instance in a different zone with zero data loss if its primary zone fails. The application does not require protection against an entire regional grid outage. What should you configure?

- A) A single-zone Cloud SQL instance with daily automated snapshots
- B) Cloud SQL High Availability (Regional configuration)
- C) A Multi-Region Cloud Storage bucket with object versioning
- D) A Zonal Persistent Disk attached to a Compute Engine VM

<details>
<summary>✅ Answer</summary>

**B) Cloud SQL High Availability (Regional configuration)**

**Explanation:** Cloud SQL High Availability provides synchronous block-level storage replication across two availability zones in the same region. It features automated health detection and failover (~60 seconds) with zero data loss (RPO = 0), matching the requirement for zone-level resilience without regional multi-site overhead.
</details>

---

### Q2. What is the fundamental operational difference between a Backup and Replication?

- A) Backups are stored on Compute Engine VMs, whereas replication is stored in BigQuery.
- B) A Backup is a static point-in-time copy used for restoration, whereas Replication is a continuous copy designed for rapid failover.
- C) Backups are always synchronous, whereas replication is always manual.
- D) Replication is free of charge, whereas backups incur network egress fees.

<details>
<summary>✅ Answer</summary>

**B) A Backup is a static point-in-time copy used for restoration, whereas Replication is a continuous copy designed for rapid failover.**

**Explanation:** Backups represent static, point-in-time data copies taken on a scheduled basis to restore systems after data loss or corruption. Replication is a continuous, near-real-time synchronization mechanism designed to enable rapid service failover with minimal downtime.
</details>

---

### Q3. Which architectural term describes a system designed to continue operating without any perceptible interruption or degradation when a hardware component fails?

- A) Disaster Recovery
- B) High Availability
- C) Fault Tolerance
- D) Cold Standby

<details>
<summary>✅ Answer</summary>

**C) Fault Tolerance**

**Explanation:** Fault Tolerance guarantees continuous operation with zero perceptible downtime during a hardware or software failure. High Availability (B) aims to minimize downtime but allows for brief, transient failover interruptions.
</details>

---

### Q4. A company needs to protect a stateful application on Compute Engine against a single zone failure. Data written by the application must be synchronously replicated across two availability zones within the same region. What storage resource should be selected?

- A) Zonal Persistent Disk
- B) Regional Persistent Disk
- C) Local SSD
- D) Cloud Storage Archive bucket

<details>
<summary>✅ Answer</summary>

**B) Regional Persistent Disk**

**Explanation:** Regional Persistent Disks synchronously replicate data blocks across two availability zones within a single region. If the primary zone fails, an identical standby VM in the second zone can force-attach the regional disk and immediately resume application operations with zero data loss.
</details>

---

### Q5. What does the operational metric RPO (Recovery Point Objective) measure?

- A) The financial budget allocated to disaster recovery infrastructure.
- B) The duration of time an application is allowed to remain offline during a disaster.
- C) The maximum acceptable amount of data loss measured backward in time from an incident.
- D) The network throughput speed between two regional data centers.

<details>
<summary>✅ Answer</summary>

**C) The maximum acceptable amount of data loss measured backward in time from an incident.**

**Explanation:** RPO defines the maximum tolerable data loss measured in time (e.g., "up to 15 minutes of transactional data"). RTO (Recovery Time Objective) defines permissible downtime (e.g., "system must be back online within 1 hour").
</details>

---

### Q6. Which Compute Engine configuration automatically distributes virtual machine instances across multiple availability zones within a region to provide high availability?

- A) Zonal Managed Instance Group
- B) Regional Managed Instance Group
- C) Standalone Compute Engine VM
- D) Unmanaged Instance Group

<details>
<summary>✅ Answer</summary>

**B) Regional Managed Instance Group**

**Explanation:** Regional Managed Instance Groups (Regional MIGs) automatically distribute virtual machine instances evenly across multiple availability zones in a single region, providing automated self-healing, load balancing, and zone-outage survivability.
</details>

---

### Q7. A non-critical internal reporting application can tolerate 12 hours of downtime (RTO) and up to 24 hours of data loss (RPO). What is the most cost-effective disaster recovery strategy?

- A) Backup & Restore using daily automated snapshots
- B) Multi-region Active-Active Hot Standby
- C) Cloud Spanner Multi-Region instance
- D) Warm Standby with auto-scaling compute pools

<details>
<summary>✅ Answer</summary>

**A) Backup & Restore using daily automated snapshots**

**Explanation:** Given the relaxed RTO (12 hours) and RPO (24 hours), Backup & Restore is the most economical strategy. It incurs costs solely for storing daily disk snapshots in Cloud Storage, eliminating the ongoing compute costs of maintaining running standby infrastructure.
</details>

---

### Q8. Why does synchronous replication across different geographical regions introduce performance bottlenecks for high-throughput write applications?

- A) Google Cloud prohibits cross-region network connections.
- B) Physical distance between distant regions introduces speed-of-light network latency, delaying transaction confirmation.
- C) Synchronous replication requires manual human approval for every database write.
- D) Regional Persistent Disks only support magnetic hard drives.

<details>
<summary>✅ Answer</summary>

**B) Physical distance between distant regions introduces speed-of-light network latency, delaying transaction confirmation.**

**Explanation:** Synchronous replication requires the primary storage system to wait for confirmation from the remote replica before acknowledging a write. Across long geographic distances, network latency delays transaction confirmation, creating severe performance bottlenecks.
</details>

---

### Q9. A GKE cluster has its Kubernetes control plane replicas distributed across three availability zones within the same region. What type of cluster is this?

- A) Zonal Cluster
- B) Regional Cluster
- C) Single-tenant Cluster
- D) Private Cluster

<details>
<summary>✅ Answer</summary>

**B) Regional Cluster**

**Explanation:** A GKE Regional Cluster distributes both its Kubernetes control plane components and worker node pools across multiple availability zones within a single region, providing higher control plane availability (99.95% SLA) compared to a single-zone control plane.
</details>

---

### Q10. A cloud architect wants to ensure that automated disk snapshots survive even if an entire Google Cloud region experiences a catastrophic outage. Where are Compute Engine Persistent Disk snapshots stored by default?

- A) On local SSD drives in the same zone
- B) On the VM's boot persistent disk
- C) In Google Cloud Storage across a Multi-Region location
- D) In BigQuery analytical datasets

<details>
<summary>✅ Answer</summary>

**C) In Google Cloud Storage across a Multi-Region location**

**Explanation:** By default, Compute Engine snapshots are stored redundantly in Google Cloud Storage across multi-region geographic locations, ensuring durability against localized regional catastrophes and allowing restoration into any other region.
</details>

---

## MSQ Section (Multiple Select Questions)

---

### Q11. Which of the following statements regarding Google Cloud Zonal resources are TRUE? (Select TWO)

- A) A Zonal resource exists exclusively within a single physical availability zone.
- B) Zonal resources are generally less expensive than their Regional equivalents.
- C) A Zonal resource automatically survives an availability zone power outage.
- D) Zonal resources automatically replicate their data across multiple continents.
- E) Compute Engine virtual machine instances can be configured as native regional resources directly without instance groups.

<details>
<summary>✅ Answer</summary>

**A) A Zonal resource exists exclusively within a single physical availability zone, B) Zonal resources are generally less expensive than their Regional equivalents.**

**Explanation:** Zonal resources reside in one zone and do not survive zonal failure (C is false). They are less expensive than regional equivalents (A and B are true). A single VM is always zonal; regional compute requires a Regional MIG (E is false).
</details>

---

### Q12. Which Google Cloud services natively offer a "Regional" configuration to protect workloads against a single availability zone failure? (Select THREE)

- A) Persistent Disk (Regional Persistent Disk)
- B) Cloud SQL (High Availability Regional configuration)
- C) Google Kubernetes Engine (Regional Cluster)
- D) A single Compute Engine virtual machine instance
- E) Cloud Shell development environment

<details>
<summary>✅ Answer</summary>

**A) Persistent Disk (Regional Persistent Disk), B) Cloud SQL (High Availability Regional configuration), C) Google Kubernetes Engine (Regional Cluster).**

**Explanation:** Persistent Disks, Cloud SQL, and GKE offer native Regional configurations that span multiple zones. A single VM (D) is inherently zonal.
</details>

---

### Q13. Which statements accurately distinguish High Availability (HA) from Fault Tolerance (FT)? (Select TWO)

- A) High Availability minimizes downtime but may experience a brief, transient failover interruption.
- B) Fault Tolerance guarantees continuous operation with zero noticeable interruption to end users.
- C) High Availability is always more expensive to implement than Fault Tolerance.
- D) Fault Tolerance only applies to cold storage backups.
- E) High Availability and Fault Tolerance are identical terms with no technical distinction.

<details>
<summary>✅ Answer</summary>

**A) High Availability minimizes downtime but may experience a brief, transient failover interruption, B) Fault Tolerance guarantees continuous operation with zero noticeable interruption to end users.**

**Explanation:** HA minimizes downtime with potential brief interruptions during automated failover. FT guarantees completely seamless, uninterrupted operation. FT is substantially more complex and costly than HA (C is false).
</details>

---

### Q14. Which of the following represent recognized Disaster Recovery strategies ordered by decreasing recovery time (RTO)? (Select THREE)

- A) Backup & Restore (Cold)
- B) Warm Standby
- C) Hot Standby (Active-Active)
- D) Permanent Snapshot Deletion
- E) Unmanaged Instance Group Cycling

<details>
<summary>✅ Answer</summary>

**A) Backup & Restore (Cold), B) Warm Standby, C) Hot Standby (Active-Active).**

**Explanation:** Backup & Restore, Warm Standby, and Hot Standby are standard industry disaster recovery tiers with progressively lower RTO and RPO.
</details>

---

### Q15. Which of the following statements regarding synchronous versus asynchronous replication are TRUE? (Select TWO)

- A) Synchronous replication guarantees zero data loss (RPO = 0) between primary and replica.
- B) Asynchronous replication is generally preferred across long geographical distances to minimize write latency.
- C) Synchronous replication has zero impact on network latency over thousands of miles.
- D) Asynchronous replication guarantees that no data can ever be lost during an unexpected primary crash.
- E) Synchronous replication is only supported on Cloud Storage Archive buckets.

<details>
<summary>✅ Answer</summary>

**A) Synchronous replication guarantees zero data loss (RPO = 0) between primary and replica, B) Asynchronous replication is generally preferred across long geographical distances to minimize write latency.**

**Explanation:** Synchronous replication guarantees RPO = 0 by waiting for confirmation, while asynchronous replication decouples writes to avoid latency penalties over long distances (at the cost of potential replication lag).
</details>

---

## 📊 Score Tracker

| Score | Verdict |
| :---: | :--- |
| **15/15** | 🏆 **Reliability Architect:** 100% prepared for HA, DR, and cross-zone replication exam questions. |
| **13–14** | ✅ **Solid Readiness:** Excellent grasp of resilience principles; minor review of RTO/RPO or Cloud SQL HA needed. |
| **10–12** | ⚠️ **Needs Review:** Re-read Section 6 (DR Strategies) and Section 14 (Exam Focus & Traps). |
| **< 10** | 🔄 **Re-study Phase 11:** Review the difference between Zonal, Regional, and Multi-Regional resource scopes. |
