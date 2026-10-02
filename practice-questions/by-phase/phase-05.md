# Phase 05 Practice Drill — Databases & Analytics

> **Total Questions:** 15 (10 MCQs + 5 MSQs)  
> **Topic Tag:** `#phase-05-databases`  
> **Target Score:** 13/15 (86%+)  
> **Covers:** Cloud SQL (MySQL, PostgreSQL, SQL Server), Firestore (Native vs. Datastore), Cloud Bigtable, Cloud Spanner, Memorystore, BigQuery (Storage, Slots, Partitioning, Clustering).

---

## MCQ Section (Single Correct Answer)

---

### Q1. A multinational financial enterprise requires a database to process real-time credit card transactions across three continents. The architecture requires full SQL support, multi-row ACID transactions, and strong consistency globally. Which database service should you recommend?

- A) Cloud SQL with cross-region read replicas
- B) Firestore in Datastore mode
- C) Cloud Bigtable
- D) Cloud Spanner

<details>
<summary>✅ Answer</summary>

**D) Cloud Spanner**

**Explanation:** Cloud Spanner is Google Cloud's globally distributed relational database designed specifically for workloads requiring horizontal scale, strong external consistency, and full multi-row ACID transaction support across continents. Cloud SQL (A) is regional and cannot perform cross-region synchronous writes. Firestore (B) is a NoSQL document database. Bigtable (C) is limited to single-row atomic transactions.
</details>

---

### Q2. A junior analyst executes an on-demand BigQuery query: `SELECT * FROM sales_archive.transactions LIMIT 5`. The underlying table contains 3 TB of data across 20 columns. The analyst expects to be billed for only 5 rows. What is the actual billing behavior?

- A) The analyst is billed for 0 bytes because the query was cached.
- B) The analyst is billed for scanning the full 3 TB of data.
- C) The analyst is billed for only the 5 rows returned.
- D) The query fails because LIMIT is unsupported in on-demand mode.

<details>
<summary>✅ Answer</summary>

**B) The analyst is billed for scanning the full 3 TB of data.**

**Explanation:** BigQuery on-demand pricing bills strictly for the volume of data scanned by the referenced columns across the entire table. Because BigQuery uses columnar storage (Capacitor), selecting `*` reads all 20 columns. The `LIMIT` clause restricts only the final output records displayed to the user, not the underlying disk scan. To reduce scan costs, the query must specify partition filters and specific columns.
</details>

---

### Q3. A mobile gaming startup is developing an iOS and Android multiplayer game. The backend requires a flexible document schema, offline data caching on client devices, and automatic real-time push updates when player statistics change. Which service should they choose?

- A) Cloud SQL (PostgreSQL)
- B) Firestore in Native Mode
- C) Cloud Bigtable
- D) Memorystore for Memcached

<details>
<summary>✅ Answer</summary>

**B) Firestore in Native Mode**

**Explanation:** Firestore in Native Mode provides native mobile SDKs supporting real-time listeners and offline local persistence. When player statistics change on the backend, listeners push updates to client apps automatically without polling. Cloud SQL (A) requires manual schema migrations and polling. Bigtable (C) is a wide-column engine lacking mobile client listeners. Memcached (D) is an ephemeral in-memory cache.
</details>

---

### Q4. Which GCP database service supports high-throughput time-series writes at millions of events per second, but is architecturally restricted to single-row atomic transactions only?

- A) Cloud SQL
- B) Cloud Spanner
- C) Cloud Bigtable
- D) BigQuery

<details>
<summary>✅ Answer</summary>

**C) Cloud Bigtable**

**Explanation:** Cloud Bigtable delivers petabyte-scale wide-column storage optimized for IoT telemetry and time-series data at single-digit millisecond latency. However, it enforces a single-row atomicity constraint—it does not support multi-row ACID transactions. Cloud SQL and Spanner both support multi-row ACID transactions.
</details>

---

### Q5. What is the operational distinction between Cloud SQL High Availability (HA) and Cloud SQL Read Replicas?

- A) HA provides read scaling; Read Replicas provide disaster recovery failover.
- B) HA maintains a standby instance in a secondary zone for automatic failover; Read Replicas offload read traffic.
- C) HA replicates data across regions; Read Replicas only operate within a single zone.
- D) HA and Read Replicas are identical configurations with differing administrative names.

<details>
<summary>✅ Answer</summary>

**B) HA maintains a standby instance in a secondary zone for automatic failover; Read Replicas offload read traffic.**

**Explanation:** High Availability (HA) provisions a synchronous standby replica in an alternate zone within the same region to provide automatic failover during zonal failures; the standby does not serve application traffic. Read Replicas asynchronously mirror data to serve read-only queries and scale read throughput, but do not provide automated disaster recovery failover.
</details>

---

### Q6. A data team frequently queries an enterprise sales table in BigQuery. Queries consistently filter on an `order_date` column spanning a 5-year timeline. How can the engineering team reduce query scan costs?

- A) Scale up the BigQuery VM machine tier.
- B) Convert the BigQuery dataset to Firestore Datastore mode.
- C) Partition the BigQuery table by the `order_date` column.
- D) Enable High Availability on the BigQuery dataset.

<details>
<summary>✅ Answer</summary>

**C) Partition the BigQuery table by the `order_date` column.**

**Explanation:** Partitioning a table by a date/timestamp column physically divides the underlying storage blocks. When a query filters on `order_date`, BigQuery prunes irrelevant partitions, scanning only the matching date blocks and dramatically slashing on-demand scan costs. BigQuery is serverless (there are no VM machine tiers to scale up).
</details>

---

### Q7. A fleet management organization ingests real-time GPS coordinates and diagnostic sensor metrics from 500,000 delivery vehicles continuously. The operational database must support massive low-latency write throughput. Which service is optimal?

- A) Cloud SQL (MySQL)
- B) BigQuery
- C) Cloud Bigtable
- D) Cloud Spanner

<details>
<summary>✅ Answer</summary>

**C) Cloud Bigtable**

**Explanation:** Cloud Bigtable is specifically optimized for massive operational time-series data ingest with single-digit millisecond latency and horizontal linear node scaling. BigQuery (B) is an OLAP data warehouse suited for batch or streaming analytics rather than high-throughput real-time operational lookups. Cloud SQL (A) cannot scale to sustain this volume of continuous concurrent write IOPS.
</details>

---

### Q8. What primary architectural function does Memorystore for Redis serve when deployed alongside a relational database?

- A) Serving as the primary persistent data store for transaction logs.
- B) Serving as an in-memory caching tier to reduce read latency and primary database CPU utilization.
- C) Running complex analytical joins across historical datasets.
- D) Providing cold disaster recovery storage for disk snapshots.

<details>
<summary>✅ Answer</summary>

**B) Serving as an in-memory caching tier to reduce read latency and primary database CPU utilization.**

**Explanation:** Memorystore provides sub-millisecond in-memory caching in RAM to absorb frequent read traffic (session tokens, product catalogs) before requests hit persistent databases like Cloud SQL or Spanner, mitigating database load by up to 90%. It is not designed to be the durable, primary system of record.
</details>

---

### Q9. A regional retail web platform requires a traditional relational database supporting SQL queries, ACID compliance, and automatic storage expansion, with total storage expected to remain under 5 TB. Which service is most cost-appropriate?

- A) Cloud Spanner
- B) Cloud Bigtable
- C) Cloud SQL
- D) BigQuery

<details>
<summary>✅ Answer</summary>

**C) Cloud SQL**

**Explanation:** Cloud SQL provides a fully managed, cost-effective relational engine (MySQL, PostgreSQL, SQL Server) with automatic storage increase and high availability for regional applications. Cloud Spanner (A) would be substantial architectural and financial overkill for a regional 5 TB database. Bigtable (B) is NoSQL. BigQuery (D) is an analytical data warehouse.
</details>

---

### Q10. Which BigQuery pricing model should an enterprise select if it runs continuous, predictable, 24/7 business intelligence analytical queries and requires fixed, capped monthly expenditure?

- A) On-demand pricing
- B) Capacity-based commitment reservations (Slots)
- C) Sustained use discounts on Compute Engine
- D) Spot analytical pricing

<details>
<summary>✅ Answer</summary>

**B) Capacity-based commitment reservations (Slots)**

**Explanation:** Capacity-based slot reservations allow organizations to purchase dedicated query processing compute units ("slots") for a fixed monthly commitment. This provides predictable budgeting for continuous heavy query workloads, in contrast to on-demand pricing which fluctuates directly with scanned terabytes.
</details>

---

## MSQ Section (Multiple Correct Answers — Select ALL that apply)

---

### Q11. Which of the following database services in Google Cloud are categorized as NoSQL databases? (Select TWO)

- A) Firestore
- B) Cloud SQL
- C) Cloud Bigtable
- D) Cloud Spanner
- E) Cloud Storage

<details>
<summary>✅ Answer</summary>

**A) Firestore, C) Cloud Bigtable**

**Explanation:** Firestore (document-based NoSQL) and Cloud Bigtable (wide-column NoSQL) are Google Cloud's NoSQL database systems. Cloud SQL and Cloud Spanner are relational (SQL) databases. Cloud Storage is an object store, not a database.
</details>

---

### Q12. Which statements regarding Cloud Spanner architecture and capabilities are TRUE? (Select THREE)

- A) It delivers external consistency globally using TrueTime technology.
- B) It supports multi-row, multi-table ACID transactions.
- C) It is substantially cheaper than Cloud SQL for small single-instance regional setups.
- D) It can scale compute and storage horizontally without maintenance downtime.
- E) It enforces a strict single-row transaction constraint.

<details>
<summary>✅ Answer</summary>

**A) It delivers external consistency globally using TrueTime technology, B) It supports multi-row, multi-table ACID transactions, D) It can scale compute and storage horizontally without maintenance downtime.**

**Explanation:** Cloud Spanner uses atomic clocks and GPS receivers via the TrueTime API to provide global external consistency. It supports full multi-row, multi-table ACID transactions and scales compute (nodes/PUs) and storage horizontally. Spanner is significantly more expensive than Cloud SQL for small regional setups (C is false), and it is Bigtable—not Spanner—that is limited to single-row transactions (E is false).
</details>

---

### Q13. Which architectural factors justify selecting BigQuery over Cloud SQL? (Select TWO)

- A) The primary workload consists of complex analytical aggregations across terabytes of historical records.
- B) The application requires sub-10ms single-row transactional lookups for live end-users.
- C) The team requires a serverless architecture where storage and compute scale and bill independently.
- D) The workload requires frequent low-latency single-row UPDATE and DELETE operations.

<details>
<summary>✅ Answer</summary>

**A) The primary workload consists of complex analytical aggregations across terabytes of historical records, C) The team requires a serverless architecture where storage and compute scale and bill independently.**

**Explanation:** BigQuery is an enterprise analytical data warehouse built for SQL aggregations over petabytes of data with a serverless decoupled pricing model. Options B and D describe online transaction processing (OLTP) access patterns, which are properly handled by Cloud SQL, Firestore, or Bigtable.
</details>

---

### Q14. Which of the following are valid table partitioning types supported natively by BigQuery? (Select THREE)

- A) Time-unit column partitioning
- B) Ingestion-time partitioning
- C) Integer-range partitioning
- D) Alphabetical string partitioning
- E) Hash-based key partitioning

<details>
<summary>✅ Answer</summary>

**A) Time-unit column partitioning, B) Ingestion-time partitioning, C) Integer-range partitioning**

**Explanation:** BigQuery natively supports three partitioning schemes: Time-unit column (partitioned by a DATE or TIMESTAMP column in the data), Ingestion-time (partitioned by the timestamp when rows are loaded), and Integer-range (partitioned by ranges of an integer column). Alphabetical string and hash-based key partitioning are not native BigQuery partitioning options.
</details>

---

### Q15. Which of the following statements are TRUE regarding Cloud Bigtable? (Select TWO)

- A) It provides native multi-row ACID transactions.
- B) It scales linearly by adding worker nodes to the cluster.
- C) It is compatible with the Apache HBase open-source API standard.
- D) It utilizes a hierarchical document-collection data model identical to Firestore.

<details>
<summary>✅ Answer</summary>

**B) It scales linearly by adding worker nodes to the cluster, C) It is compatible with the Apache HBase open-source API standard.**

**Explanation:** Cloud Bigtable throughput scales linearly as worker nodes are added, and it natively supports the Apache HBase API. Bigtable does not support multi-row transactions (A is false; mutations are atomic only per single row), and it utilizes a sparse, wide-column multidimensional map structure rather than a hierarchical document-collection model (D is false).
</details>

---

## 📊 Score Tracker

| Score | Verdict |
| :---: | :--- |
| **15/15** | 🏆 **Database Architect Master:** Completely exam-ready for this domain. |
| **13–14** | ✅ **Solid Readiness:** Minor review of BigQuery cost model or Spanner TrueTime needed. |
| **10–12** | ⚠️ **Needs Review:** Re-read the Master Database Comparison Matrix and Bigtable vs. BigQuery traps. |
| **< 10** | 🔄 **Re-study Phase 05:** Carefully review Section 3 (Decision Tree) and Section 15 (Master Comparison Table). |
