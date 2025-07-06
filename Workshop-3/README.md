# Workshop-3 – Concurrency & Distributed Design  
**Google Drive Clone – Databases II**

> *Concurrency Analysis, Parallel & Distributed Database Design*

## 📘 Sections Overview
1. **Project Scope**  
2. **Key Concurrency Scenarios**  
3. **Problem Identification & Solutions**  
4. **Parallel & Distributed Database Design**  
5. **Performance-Improvement Strategies**  
6. **References**  
7. **Authors**

---

## 1  Project Scope
Workshop 3 extends the Google Drive Clone by analysing critical **concurrency risks** and proposing a **distributed data architecture** capable of supporting ≥ 5 000 concurrent users without compromising integrity or performance.

Deliverables:

* A catalogue of race conditions, deadlocks and isolation anomalies observed in the core services (*File*, *Folder*, *Sharing*, *User*, *Analytics*, etc.).  
* Concrete mitigation techniques (optimistic locking, hierarchical lock ordering, the Saga pattern, queue-based indexing, etc.).  
* A high-level blueprint for partitioned, replicated and parallel data stores aligned with the project’s hexagonal architecture.

---

## 2  Key Concurrency Scenarios

| Domain               | Representative Scenario                     | Typical Hazard                                                     |
|----------------------|---------------------------------------------|--------------------------------------------------------------------|
| **FileService**      | Simultaneous uploads to the same folder     | Lost increments in `used_storage`; duplicate filenames             |
| **Folder**           | Parallel creation of sub-folders            | Duplicate names; incorrect `children_count`                        |
| **Sharing / ACLs**   | Two sessions modify the same permission set | Conflicting access levels                                          |
| **User Management**  | Concurrent quota increment & decrement      | Inaccurate `used_storage` value (< 0 or stale)                     |
| **LogService**       | Analytics query during heavy inserts        | Non-repeatable / phantom reads                                     |
| **Analytics**        | ETL on `Activity_Log` under heavy writes    | Dirty reads; skewed metrics                                        |
| **Cross-Service**    | Metadata write succeeds but audit fails     | Incomplete distributed operation                                   |

*(The full scenario catalogue appears in the accompanying PDF).*

---

## 3  Problem Identification & Proposed Solutions

### 3.1 File & Folder Operations  
* **Atomic Counters** – `SET used_storage = used_storage + ?` executed as a single DB primitive.  
* **Unique Composite Keys** – `(parent_folder_id, name)` eliminates duplicates.  
* **Hierarchical Lock Protocol** – always lock parent → children to avoid deadlocks.

### 3.2 Sharing & Permissions  
* **Versioned Upserts (Optimistic Locking)** – prevents lost updates.  
* **Repeatable-Read Audits** – LogService ensures a consistent snapshot.

### 3.3 Distributed Flows  
* **Saga Pattern** – coordinates metadata, blob storage and logging through events; compensation reverses partial work.  
* **Message Queue for Indexing** – decouples metadata writes from Elasticsearch updates, ensuring eventual consistency.

---

## 4  Parallel & Distributed Database Design

| Layer                              | Suggested Technology      | Distribution Strategy                                |
|------------------------------------|---------------------------|------------------------------------------------------|
| **Transactional SQL** (Users, Subscriptions) | PostgreSQL cluster       | Primary + regional read replicas                     |
| **Metadata NoSQL** (Files, Folders)        | MongoDB / DynamoDB       | Shard by `tenant_id`                                 |
| **Blob Storage**                   | S3-compatible buckets     | Region-aware hot/cold tiers                          |
| **Graph (Permissions)**            | Neo4j                     | Community-based partitioning                         |
| **Time-Series Analytics**          | InfluxDB / TimescaleDB    | Monthly partitions, compression enabled              |

---

## 5  Performance-Improvement Strategies
1. **Horizontal Scaling & Load Balancing** – multiple API Gateway and service pods distribute > 5 000 concurrent sessions.  
2. **Sharding** – isolates high-traffic tenants, reducing single-node hotspots.  
3. **Replication** – routes read-heavy dashboards to replicas; improves availability.  
4. **Parallel Query Execution** – analytics jobs split by date range, processed concurrently on partitions, then merged.

> **Trade-offs:** greater operational complexity, potential replica lag, careful sharding-key selection.

---

## 6  References
* Amazon Web Services. *What Is Data Architecture?* (2025).  
* Corporate Finance Institute. *Business Model Canvas – Examples* (2025).  
* IBM. *Data Architecture Fundamentals* (2025).  
* UNIR Revista. *Entity–Relationship Model* (2025).  
* Google Workspace. *Google Drive Product Page* (2025).

---

## 7  Authors
* **Cristian David Casas Díaz** – 20192020091  
* **Dylan Alejandro Solarte Rincón** – 20201020088  
* **Professor:** Carlos Andrés Sierra Virguez

---

*README generated for Workshop 3 – Concurrency & Distributed Design (July 2025).*
