# Phase 07 Practice Drill - Kubernetes Engine (GKE)

> **Total Questions:** 15 (10 MCQs + 5 MSQs)  
> **Topic Tag:** `#phase-07-kubernetes-gke`  
> **Target Score:** 13/15 (86%+)  
> **Covers:** GKE Standard vs. Autopilot, K8s Core Objects (Pods, Deployments, ReplicaSets, Services, Namespaces), Scaling (HPA, Cluster Autoscaler), Upgrades & Repair, Workload Identity.

---

## MCQ Section (Single Correct Answer)

---

### Q1. A startup is migrating its containerized microservices to Google Cloud. The development team wants to leverage Kubernetes APIs but has no dedicated infrastructure operations engineers and wants to avoid all node pool maintenance and VM sizing. Which GKE operating mode should they select?

- A) GKE Standard Mode with zonal node pools
- B) GKE Autopilot Mode
- C) GKE Standard Mode with Cluster Autoscaler
- D) Compute Engine Managed Instance Groups

<details>
<summary>[Yes] Answer</summary>

**B) GKE Autopilot Mode**

**Explanation:** GKE Autopilot provides a fully managed, hands-off Kubernetes experience where Google manages node provisioning, scaling, auto-repair, and security hardening. In GKE Standard (A and C), teams are still responsible for configuring and managing node pools and virtual machines. Compute Engine MIGs (D) are not Kubernetes.
</details>

---

### Q2. What is the correct hierarchical order of Kubernetes workload management abstractions from highest level to lowest execution unit?

- A) Pod -> ReplicaSet -> Deployment
- B) Deployment -> ReplicaSet -> Pod
- C) Service -> Deployment -> Pod
- D) ReplicaSet -> Deployment -> Pod

<details>
<summary>[Yes] Answer</summary>

**B) Deployment -> ReplicaSet -> Pod**

**Explanation:** In Kubernetes workload management, you declare desired application state in a **Deployment**. The Deployment automatically creates and manages a **ReplicaSet**, which in turn enforces the desired quantity and health of **Pod** instances. A Service is a network abstraction that routes to Pods, not a controller in this lifecycle hierarchy.
</details>

---

### Q3. How does billing differ between GKE Standard and GKE Autopilot?

- A) GKE Standard bills per pod request; GKE Autopilot bills per provisioned VM.
- B) GKE Standard bills for provisioned Compute Engine VM nodes; GKE Autopilot bills for resources requested by active Pods.
- C) Both modes bill identical flat rates per cluster.
- D) GKE Autopilot is free; customers pay only for network egress.

<details>
<summary>[Yes] Answer</summary>

**B) GKE Standard bills for provisioned Compute Engine VM nodes; GKE Autopilot bills for resources requested by active Pods.**

**Explanation:** In GKE Standard, you pay for the underlying Compute Engine VM instances in your node pools, regardless of whether your containers fully utilize that capacity. In GKE Autopilot, Google manages the nodes and bills you strictly for the cumulative vCPU, memory, and storage requested by running Pods.
</details>

---

### Q4. Why is a Kubernetes Service required when deploying application pods behind a frontend?

- A) Pods cannot execute without an attached Service.
- B) Pods are ephemeral with dynamic IP addresses; a Service provides a stable IP and load balances across them.
- C) Services compile container source code before execution.
- D) Services are required to enforce IAM access policies on Compute Engine.

<details>
<summary>[Yes] Answer</summary>

**B) Pods are ephemeral with dynamic IP addresses; a Service provides a stable IP and load balances across them.**

**Explanation:** Pods are frequently created, destroyed, or rescheduled across nodes, resulting in dynamic, changing internal IP addresses. A Kubernetes Service provides an immutable, stable virtual IP and internal DNS name that load balances requests across all pods matching the Service's label selector.
</details>

---

### Q5. Which GKE feature automatically detects unhealthy worker nodes and provisions replacement virtual machines?

- A) Node Auto-Upgrade
- B) Node Auto-Repair
- C) Horizontal Pod Autoscaler
- D) Workload Identity

<details>
<summary>[Yes] Answer</summary>

**B) Node Auto-Repair**

**Explanation:** Node Auto-Repair continuously monitors worker node health status. If a node fails health checks repeatedly for several minutes, GKE drains and reprovisions a fresh replacement node. Node Auto-Upgrade (A) automatically patches Kubernetes versions. HPA (C) scales pods, not nodes.
</details>

---

### Q6. A Node Pool in GKE is best defined as:

- A) A single virtual machine running one container.
- B) A group of worker nodes within a cluster that share identical hardware and machine configurations.
- C) A Kubernetes namespace designated for database storage.
- D) An external load balancer forwarding traffic into the cluster.

<details>
<summary>[Yes] Answer</summary>

**B) A group of worker nodes within a cluster that share identical hardware and machine configurations.**

**Explanation:** A Node Pool is a group of nodes within a GKE cluster that all share the same configuration (such as machine type, disk size, labels, and Spot VM settings), allowing you to run distinct workload types on specialized machine pools within a single cluster.
</details>

---

### Q7. A software team is deploying an internal batch processing service on GKE Standard that can tolerate unexpected interruptions. Which strategy minimizes infrastructure costs?

- A) Deploy a multi-region Autopilot cluster.
- B) Configure a node pool using Spot VMs.
- C) Disable the Cluster Autoscaler.
- D) Allocate persistent SSD disks to every pod.

<details>
<summary>[Yes] Answer</summary>

**B) Configure a node pool using Spot VMs.**

**Explanation:** Spot VMs offer 60%–91% discounts compared to standard Compute Engine pricing. Because batch processing workloads can tolerate sudden preemption and reschedule gracefully, Spot VM node pools provide substantial cost savings.
</details>

---

### Q8. What security advantage does Workload Identity provide when running pods in GKE?

- A) Allows pods to authenticate to Google Cloud APIs securely without storing static service account JSON key files.
- B) Automatically encrypts node boot disks using CMEK.
- C) Disables public IP addresses across all cluster load balancers.
- D) Prevents developers from executing kubectl commands.

<details>
<summary>[Yes] Answer</summary>

**A) Allows pods to authenticate to Google Cloud APIs securely without storing static service account JSON key files.**

**Explanation:** Workload Identity binds Kubernetes Service Accounts (KSAs) to Google Cloud IAM Service Accounts (GSAs). Pods automatically receive short-lived, ephemeral Google credentials, completely eliminating the need to download, mount, and manage static, long-lived JSON service account keys in pods.
</details>

---

### Q9. Which component in GKE is responsible for adding Compute Engine worker nodes when pods cannot be scheduled due to insufficient CPU or memory?

- A) Horizontal Pod Autoscaler (HPA)
- B) Cluster Autoscaler (CA)
- C) Kubelet
- D) ReplicaSet

<details>
<summary>[Yes] Answer</summary>

**B) Cluster Autoscaler (CA)**

**Explanation:** Cluster Autoscaler monitors for unschedulable (Pending) pods due to insufficient compute resources across worker nodes, and automatically provisions additional Compute Engine VMs in the appropriate node pool. Horizontal Pod Autoscaler (A) scales the number of pods based on observed application metric load.
</details>

---

### Q10. What is the primary operational role of a Kubernetes Namespace?

- A) Allocating external public IP addresses to pods.
- B) Logically partitioning cluster resources and access control across teams or environments.
- C) Managing physical network cabling between Google data centers.
- D) Automatically encrypting pod storage volumes.

<details>
<summary>[Yes] Answer</summary>

**B) Logically partitioning cluster resources and access control across teams or environments.**

**Explanation:** Namespaces provide virtual administrative boundaries within a single physical Kubernetes cluster. They allow organizations to separate distinct projects, teams, or environments (e.g., dev, staging, prod), applying scoped RBAC policies and resource quotas to prevent noisy-neighbor interference.
</details>

---

## MSQ Section (Multiple Correct Answers - Select ALL that apply)

---

### Q11. Which of the following statements regarding GKE Autopilot are TRUE? (Select TWO)

- A) Google completely manages worker node provisioning, maintenance, and security hardening.
- B) The customer is billed for entire underlying VM node capacity regardless of usage.
- C) Billing is calculated based on the CPU, memory, and storage requested by running Pods.
- D) Customers must manually configure Cluster Autoscaler thresholds on node pools.
- E) Customers can SSH directly into underlying worker node virtual machines.

<details>
<summary>[Yes] Answer</summary>

**A) Google completely manages worker node provisioning, maintenance, and security hardening, C) Billing is calculated based on the CPU, memory, and storage requested by running Pods.**

**Explanation:** In GKE Autopilot, Google manages the entire infrastructure (node provisioning, security, scaling), and you are billed strictly for the resources requested by active pods. Paying for provisioned VM capacity (B) and managing node pools (D) apply to GKE Standard. Direct SSH into Autopilot nodes (E) is disallowed for security hardening.
</details>

---

### Q12. Which of the following are core Kubernetes objects natively managed in GKE? (Select THREE)

- A) Pod
- B) Deployment
- C) Service
- D) Cloud Function
- E) VPC Subnet

<details>
<summary>[Yes] Answer</summary>

**A) Pod, B) Deployment, C) Service**

**Explanation:** Pods, Deployments, and Services are fundamental declarative Kubernetes API objects. Cloud Functions is a Google serverless compute service, and VPC Subnets are Google Cloud networking primitives.
</details>

---

### Q13. Which automated management features are provided natively by GKE for worker node pools? (Select TWO)

- A) Node Auto-Upgrade
- B) Node Auto-Repair
- C) Automated code refactoring
- D) Automated SQL schema migrations
- E) Automated client DNS registration

<details>
<summary>[Yes] Answer</summary>

**A) Node Auto-Upgrade, B) Node Auto-Repair**

**Explanation:** GKE provides Node Auto-Upgrade (keeping nodes patched to the control plane version) and Node Auto-Repair (detecting unhealthy nodes and replacing them). GKE does not refactor application code or perform database schema migrations.
</details>

---

### Q14. Which statements regarding a Kubernetes Pod are TRUE? (Select TWO)

- A) It is the smallest deployable computing unit in Kubernetes.
- B) It can encapsulate one or more tightly coupled containers sharing network and storage.
- C) Pods maintain permanent, immutable IP addresses throughout their entire lifecycle.
- D) A Pod replaces the need for a container image.
- E) Pods can only run directly on bare metal physical servers.

<details>
<summary>[Yes] Answer</summary>

**A) It is the smallest deployable computing unit in Kubernetes, B) It can encapsulate one or more tightly coupled containers sharing network and storage.**

**Explanation:** A Pod is the fundamental unit of deployment in Kubernetes and can house one or more co-located containers that share localhost networking and mounted storage volumes. Pod IPs are ephemeral and change upon rescheduling (C is false).
</details>

---

### Q15. In which scenarios is Cloud Run generally preferred over GKE? (Select TWO)

- A) A simple stateless web microservice that benefits from automatic scale-to-zero.
- B) A complex multi-service application requiring custom network mesh and DaemonSets.
- C) An application team wanting minimal operational complexity with zero cluster configuration.
- D) Workloads requiring custom GPU drivers and kernel-level modifications.
- E) High-performance computing workloads requiring bare-metal host access.

<details>
<summary>[Yes] Answer</summary>

**A) A simple stateless web microservice that benefits from automatic scale-to-zero, C) An application team wanting minimal operational complexity with zero cluster configuration.**

**Explanation:** Cloud Run is the preferred choice for simple stateless HTTP applications that need native scale-to-zero, fast request-based autoscaling, and zero cluster or infrastructure overhead. Complex multi-service orchestration with DaemonSets (B) or custom kernel drivers (D) require GKE.
</details>

---

## Score Tracker

| Score | Verdict |
| :---: | :--- |
| **15/15** | **Kubernetes Master:** Fully prepared for GKE exam scenarios. |
| **13–14** | [Yes] **Solid Readiness:** Minor review of Workload Identity or scaling mechanics needed. |
| **10–12** | Note: **Needs Review:** Re-read Section 6 (Standard vs. Autopilot) and Section 4 (Object Hierarchy). |
| **< 10** | **Re-study Phase 07:** Thoroughly study the Deployment -> ReplicaSet -> Pod -> Service chain. |
