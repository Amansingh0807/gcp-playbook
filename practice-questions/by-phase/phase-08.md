# Phase 08 Practice Drill - Monitoring & Operations

> **Total Questions:** 15 (10 MCQs + 5 MSQs)  
> **Topic Tag:** `#phase-08-monitoring-operations`  
> **Target Score:** 13/15 (86%+)  
> **Covers:** Cloud Monitoring (Metrics, Dashboards, Alerts, Uptime Checks), Cloud Logging (Log Explorer, Router, Sinks, Buckets, Audit Logs), Diagnostics (Trace, Profiler, Error Reporting, Ops Agent).

---

## MCQ Section (Single Correct Answer)

---

### Q1. Which Google Cloud Audit Log type is ALWAYS enabled, 100% free of charge, and cannot be disabled by any administrator?

- A) Data Access Audit Logs
- B) Admin Activity Audit Logs
- C) Cloud Trace Audit Logs
- D) Custom Ops Agent Logs

<details>
<summary>[Yes] Answer</summary>

**B) Admin Activity Audit Logs**

**Explanation:** Admin Activity audit logs document administrative configuration and metadata modifications (such as creating VMs or deleting storage buckets). They are permanently enabled across all Google Cloud services, free of charge, retained for 400 days in the `_Required` log bucket, and cannot be disabled by any user. Data Access audit logs (A) are disabled by default and incur storage costs.
</details>

---

### Q2. A team needs to monitor an external web application and receive immediate notifications whenever the homepage becomes unreachable from public clients worldwide. What should be configured?

- A) Cloud Trace latency span
- B) Cloud Monitoring Uptime Check paired with an Alert Policy
- C) A custom Cloud Profiler agent
- D) Cloud Logging sink routing to Pub/Sub

<details>
<summary>[Yes] Answer</summary>

**B) Cloud Monitoring Uptime Check paired with an Alert Policy**

**Explanation:** An Uptime Check regularly sends synthetic HTTP/HTTPS/TCP probes from multiple geographical edge locations worldwide to evaluate endpoint availability. Attaching an Alert Policy ensures that on-call engineers are immediately notified via email, Slack, or PagerDuty if the endpoint fails health checks.
</details>

---

### Q3. An e-commerce platform observes that user checkout requests are taking over 4 seconds to complete across a chain of 6 microservices. Which tool should the team use to pinpoint the exact service causing the bottleneck?

- A) Cloud Logging
- B) Cloud Trace
- C) Cloud Profiler
- D) Error Reporting

<details>
<summary>[Yes] Answer</summary>

**B) Cloud Trace**

**Explanation:** Cloud Trace is Google Cloud's distributed tracing system designed specifically to track incoming requests as they propagate across multiple microservices. It visualizes the latency of every remote procedure call (RPC) span, pinpointing the exact microservice causing request delays. Cloud Profiler (C) analyzes in-code function execution, not distributed network hops.
</details>

---

### Q4. Which log destination should an organization select if it requires long-term compliance log archival for 7 years at the absolute lowest cost per gigabyte?

- A) Cloud Logging `_Default` bucket
- B) Cloud Storage bucket configured with Coldline or Archive storage
- C) Cloud Pub/Sub topic
- D) BigQuery partitioned dataset

<details>
<summary>[Yes] Answer</summary>

**B) Cloud Storage bucket configured with Coldline or Archive storage**

**Explanation:** Cloud Storage Archive class delivers the lowest cost per gigabyte for multi-year compliance archival and supports bucket retention policy locks. Cloud Logging buckets (A) and BigQuery (D) are significantly more expensive per gigabyte for long-term cold storage. Pub/Sub (C) is a real-time message bus, not a durable storage archive.
</details>

---

### Q5. Why does enabling Data Access audit logs broadly across an entire Google Cloud organization pose a financial risk?

- A) Data Access logs incur high licensing fees from third-party vendors.
- B) Data Access logs record every read and write of user data, generating massive log volumes and substantial ingestion charges.
- C) Data Access logs require upgrading all VMs to custom machine tiers.
- D) Data Access logs disable Google Cloud committed use discounts.

<details>
<summary>[Yes] Answer</summary>

**B) Data Access logs record every read and write of user data, generating massive log volumes and substantial ingestion charges.**

**Explanation:** Data Access audit logs record operations that read or write user-provided resource data. In high-traffic services (such as Cloud Storage, BigQuery, or Cloud SQL), this generates billions of log events, resulting in massive, unexpected ingestion and storage charges if enabled broadly without scoping.
</details>

---

### Q6. What is the fundamental operational difference between Cloud Monitoring and Cloud Logging?

- A) Cloud Monitoring manages security; Cloud Logging manages billing.
- B) Cloud Monitoring captures numeric time-series metrics; Cloud Logging captures discrete event records.
- C) Cloud Logging is serverless; Cloud Monitoring requires managing dedicated VMs.
- D) Cloud Monitoring only functions on Google Kubernetes Engine.

<details>
<summary>[Yes] Answer</summary>

**B) Cloud Monitoring captures numeric time-series metrics; Cloud Logging captures discrete event records.**

**Explanation:** Cloud Monitoring deals with time-series numeric data (answering "how much", "how fast", and tracking utilization thresholds). Cloud Logging deals with discrete, timestamped text and JSON event records (answering "what happened" and "who initiated the action").
</details>

---

### Q7. A security compliance officer requires a permanent, unmodifiable record of every administrative configuration change made within a production project. Where are these records stored by default?

- A) Cloud Storage Nearline bucket
- B) BigQuery audit dataset
- C) The immutable `_Required` log bucket in Cloud Logging
- D) Compute Engine metadata server

<details>
<summary>[Yes] Answer</summary>

**C) The immutable `_Required` log bucket in Cloud Logging**

**Explanation:** Admin Activity, System Event, and Policy Denied audit logs are automatically ingested into the `_Required` log bucket in Cloud Logging, where they are retained for 400 days. This bucket is immutable and cannot be deleted, modified, or disabled by any user.
</details>

---

### Q8. A software engineer wants to identify which exact function inside a Go backend binary is consuming 80% of CPU during request processing. Which tool is purpose-built for this?

- A) Cloud Trace
- B) Cloud Profiler
- C) Error Reporting
- D) Cloud Logging

<details>
<summary>[Yes] Answer</summary>

**B) Cloud Profiler**

**Explanation:** Cloud Profiler continuously collects statistical CPU and memory allocation samples directly from running application code, displaying interactive flame graphs that pinpoint the exact functions and lines of code consuming resources. Cloud Trace (A) inspects distributed latency across services, not in-code CPU consumption.
</details>

---

### Q9. Which component in Cloud Logging evaluates filters to decide whether log entries are retained in log buckets or forwarded to external destinations?

- A) Metric Descriptor
- B) Log Router
- C) Uptime Check
- D) Ops Agent

<details>
<summary>[Yes] Answer</summary>

**B) Log Router**

**Explanation:** The Log Router checks every log entry against inclusion and exclusion filter rules to determine where to deliver the record - routing entries to the `_Default` or `_Required` buckets, or exporting them via sinks to BigQuery, Cloud Storage, or Pub/Sub.
</details>

---

### Q10. What is the primary benefit of deploying Log Exclusion Filters in Cloud Logging?

- A) They automatically fix application runtime exceptions.
- B) They discard high-volume, low-value logs at ingestion, preventing unwanted storage and ingestion fees.
- C) They convert text logs into Cloud Trace spans.
- D) They force Data Access logs to become free of charge.

<details>
<summary>[Yes] Answer</summary>

**B) They discard high-volume, low-value logs at ingestion, preventing unwanted storage and ingestion fees.**

**Explanation:** Exclusion filters instruct the Log Router to drop matching logs before they are written to storage buckets, completely eliminating storage and ingestion charges for noisy, repetitive logs (such as load balancer health check pings).
</details>

---

## MSQ Section (Multiple Correct Answers - Select ALL that apply)

---

### Q11. Which of the following Audit Log types in Google Cloud are ALWAYS enabled by default and free of charge? (Select THREE)

- A) Admin Activity
- B) Data Access
- C) System Event
- D) Policy Denied
- E) Cloud Profiler Execution

<details>
<summary>[Yes] Answer</summary>

**A) Admin Activity, C) System Event, D) Policy Denied**

**Explanation:** Admin Activity, System Event, and Policy Denied audit logs are always enabled by default and 100% free of charge. Data Access audit logs (B) are disabled by default for most services and incur standard logging storage costs when enabled.
</details>

---

### Q12. Which of the following are valid destination targets for Cloud Logging sinks? (Select THREE)

- A) BigQuery dataset
- B) Cloud Storage bucket
- C) Cloud Pub/Sub topic
- D) Cloud Trace span collector
- E) Compute Engine persistent disk

<details>
<summary>[Yes] Answer</summary>

**A) BigQuery dataset, B) Cloud Storage bucket, C) Cloud Pub/Sub topic**

**Explanation:** Supported export destinations for Log Router sinks are BigQuery (for analytics), Cloud Storage (for cost-effective archival), Cloud Pub/Sub (for streaming to external SIEMs), and other Cloud Logging Log Buckets.
</details>

---

### Q13. Which metrics can be viewed natively by Cloud Monitoring from a Compute Engine VM WITHOUT installing the Ops Agent? (Select TWO)

- A) Hypervisor CPU Utilization
- B) Guest Operating System RAM Memory Utilization
- C) Network Egress Packet Count
- D) Linux Guest Disk Free Space Percentage
- E) Running Process Table Memory Footprint

<details>
<summary>[Yes] Answer</summary>

**A) Hypervisor CPU Utilization, C) Network Egress Packet Count**

**Explanation:** The Google Cloud hypervisor tracks physical CPU time, network traffic, and raw disk read/write throughput from outside the VM. It cannot inspect the internal guest OS memory space (RAM) or filesystem free space percentages without installing the Google Cloud Ops Agent.
</details>

---

### Q14. Which statements regarding Cloud Trace and Cloud Profiler are TRUE? (Select TWO)

- A) Cloud Trace inspects distributed latency across microservices in a request path.
- B) Cloud Profiler inspects CPU and memory allocation within application source code.
- C) Cloud Trace and Cloud Profiler are identical tools renamed for different regions.
- D) Cloud Profiler requires installing physical network cables in Google data centers.
- E) Cloud Trace only functions on on-premises bare metal servers.

<details>
<summary>[Yes] Answer</summary>

**A) Cloud Trace inspects distributed latency across microservices in a request path, B) Cloud Profiler inspects CPU and memory allocation within application source code.**

**Explanation:** Cloud Trace evaluates distributed request latency across multiple services in an invocation chain, while Cloud Profiler evaluates in-code function execution and heap memory consumption.
</details>

---

### Q15. Which practices effectively optimize Google Cloud Logging expenditures? (Select TWO)

- A) Implementing log exclusion filters to drop repetitive load balancer health-check entries.
- B) Enabling Data Access audit logs across all services organization-wide.
- C) Routing long-term compliance logs to Cloud Storage Nearline/Coldline buckets.
- D) Increasing the retention duration of the `_Required` bucket to 10 years.
- E) Converting all monitoring metrics into text logs.

<details>
<summary>[Yes] Answer</summary>

**A) Implementing log exclusion filters to drop repetitive load balancer health-check entries, C) Routing long-term compliance logs to Cloud Storage Nearline/Coldline buckets.**

**Explanation:** Exclusion filters discard noisy logs before storage billing occurs, and exporting historical logs to Cloud Storage Nearline/Coldline significantly cuts storage expenses compared to retaining logs in Cloud Logging buckets. The `_Required` bucket's retention is fixed at 400 days and cannot be modified (D is false).
</details>

---

## Score Tracker

| Score | Verdict |
| :---: | :--- |
| **15/15** | **Observability Master:** Completely prepared for monitoring and logging exam questions. |
| **13–14** | [Yes] **Solid Readiness:** Minor review of audit log billing or Ops Agent capabilities needed. |
| **10–12** | Note: **Needs Review:** Re-read Section 6 (Audit Logs Matrix) and Section 4.2 (Log Router & Sinks). |
| **< 10** | **Re-study Phase 08:** Review the distinction between Monitoring (metrics) and Logging (events). |
