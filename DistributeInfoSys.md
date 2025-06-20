# 1. Introduction
## Distributed Information Systems (DIS)

**Definition:**  
Distributed Information Systems are systems where **data storage, processing, and access are spread across multiple computers** that communicate over a network.  
They are the predominant systems for data management and access today.

---

### Key Characteristics
- ✅ **Transparency:** Users don’t need to know where data or processes physically reside.
- ✅ **Scalability:** Can grow to handle more data and more users.
- ✅ **Fault tolerance:** Continues functioning despite some node failures.

---

### Based on Two Core Concepts
1. **Distributed data management**  
   - Focuses on **how data is distributed, replicated, and kept consistent** across multiple nodes.  
   - *Examples:* Distributed databases like **Google Spanner**, **Cassandra**, or **MongoDB sharding**.

2. **Distributed control (application layer)**  
   - Focuses on **how application logic is distributed** across machines or services.  
   - *Examples:* Microservice architectures, cloud-native apps, distributed middleware.

---

> **In practice, most distributed information systems use both distributed data management and distributed control jointly.**

---

### Why these approaches matter
✅ Enables handling of **massive datasets** and **millions of requests**.  
✅ Improves **availability** and **fault-tolerance**.  
✅ Enhances **scalability** and **performance**.

---

### Examples of Modern Distributed Information Systems
- 🌐 **Google Search Backend** (distributed data + distributed control)  
- 🗄️ **Amazon DynamoDB** (distributed data management)  
- 🎥 **Netflix Microservices** (distributed control + some distributed data management)  
- ☁️ **Apache Spark** (distributed data processing engine)

---

Let me know if you'd also like a **diagram**, or a more **technical deep dive** into one of these concepts!

## What are Distributed Information Systems?

**Distributed Information Systems** are systems built on a **three- or multi-tier architecture** where one or more of the following tiers is distributed across different nodes in a network:

- 🔄 The **data management layer** (e.g. distributed databases, file systems)  
- ⚙️ The **application layer** (e.g. distributed business logic, service tiers)

---

### Key Characteristics
✅ **Physically distributed:**  
Data and/or application components run on different machines or servers.

✅ **Logically distributed:**  
The data or services appear as a single, unified system to end-users.
### Difference between Physically and Logically Distributed

| Aspect                 | Physically Distributed                                        | Logically Distributed                                              |
|------------------------|--------------------------------------------------------------|---------------------------------------------------------------------|
| **Definition**          | Components (data, apps, services) actually run on **different machines or servers** across a network. | Multiple distributed components **appear as one unified system** to the end-user or client. |
| **User Perspective**    | End-user might need to know **which server or node** is being contacted. | End-user **sees a single interface or endpoint**, unaware of underlying complexity. |
| **System Complexity**   | Emphasis on physical placement — different machines, networks, or data centers. | Emphasis on **transparency** — hides the complexity of where or how data is distributed. |
| **Example**             | A web app with databases on different servers in different cities. | Dropbox/Google Drive — files are on many servers, but you just see one “drive.” |

---

> 💡 **Summary:**  
> - **Physically distributed** is about **actual separation across hardware or locations.**  
> - **Logically distributed** is about **presenting a unified, seamless interface to the user** regardless of where the pieces actually live.

---

### Typical Architecture
In a distributed information system:
1. **Presentation tier** — user interface or client-side application
2. **Application tier** — distributed business logic and services
3. **Data management tier** — distributed data storage and access

---

> **In short:**  
Distributed Information Systems leverage multiple machines and network connections so that data management and/or application logic is split up across different nodes — improving scalability, fault tolerance, and resource usage.

### Layers in a Distributed Information System

A Distributed Information System typically consists of **at least three tiers**:

1. **Presentation Tier** — user interface (e.g. web front-end, mobile app)
2. **Application Tier** — business logic and services (often distributed across multiple servers)
3. **Data Tier** — databases or file systems (distributed for scalability and fault tolerance)

> 💡 In distributed systems, **at least one of these tiers is physically or logically distributed** across multiple machines on a network.

## Distributed Databases

In distributed databases, data is **split** and/or **replicated** across multiple machines to improve scalability, availability, and fault tolerance.

---

### 🔀 Data Partitioning
- **Why?**  
  The volume of data is too big for a single node or single database.
- **How?**  
  The data is **split into smaller chunks** and **distributed across multiple databases**.
- **Also known as:**  
  Multi-databases or federated databases.
- **Consequence:**  
  Requires **distributed transactions** to maintain consistency across partitions.

> 💡 **Requires distributed transactions to maintain consistency across partitions.**  
> 
> When data is **partitioned** across multiple databases or nodes, operations that involve **data spanning multiple partitions** must use **distributed transactions**.  
> 
> - This ensures all involved partitions either **commit** or **abort together**,  
> - Preserving **atomicity, consistency, isolation, and durability (ACID)** across the distributed setup.  
> - Otherwise, you could end up with **inconsistent** data if one partition is updated and another is not.

---

### 🧬 Data Replication
- **Why?**  
  To **increase availability** and/or improve **scalability of data access**.
- **How?**  
  The data is **stored several times** in different databases (replicas).
- **Consequence:**  
  Requires **replica management and coordination** to keep copies consistent.

---

### 🔄 Combination of Partitioning and Replication
- Many distributed database systems use **both** techniques:
  - Partitioning to **scale out** across multiple machines.
  - Replication to **increase fault tolerance and read performance**.

---

> 💡 **In summary:**  
> - **Data partitioning** splits big data into smaller chunks distributed across different databases.
> - **Data replication** keeps multiple copies of the same data on different nodes for better availability.

## How Distributed Transactions Work

When a transaction involves multiple databases or nodes (partitions), a **distributed transaction** ensures the operation is completed **atomically and consistently** across all participants.

---

### Challenges
- Each database/node may be on a different machine.
- Network failures or crashes can interrupt parts of the transaction.
- Need to guarantee **all-or-nothing** outcome despite distribution.

---

### The Two-Phase Commit Protocol (2PC)

A common protocol to coordinate distributed transactions is the **Two-Phase Commit (2PC)**:

1. **Phase 1: Prepare Phase**
   - The **transaction coordinator** sends a "prepare" request to all participating nodes.
   - Each node performs the transaction steps locally but does **not commit yet**.
   - Each node replies with either:
     - **Vote Commit** (ready to commit), or
     - **Vote Abort** (unable to commit).

2. **Phase 2: Commit/Abort Phase**
   - If **all nodes vote Commit**, the coordinator sends a "commit" message to all nodes, instructing them to finalize the transaction.
   - If **any node votes Abort**, the coordinator sends an "abort" message to all nodes, instructing them to roll back.
   - Each node responds with acknowledgment after committing or aborting.

---

### Guarantees
- **Atomicity:** All nodes commit or none do.
- **Consistency:** The system moves from one consistent state to another.
- **Isolation:** Transactions appear isolated, even across nodes.
- **Durability:** Once committed, changes survive failures.

---

### Limitations
- 2PC can be slow due to waiting for all nodes.
- Can block if the coordinator crashes during commit.
- More advanced protocols (e.g., Three-Phase Commit, Paxos-based consensus) improve fault tolerance.

---

### Summary
Distributed transactions use protocols like **Two-Phase Commit** to ensure that operations involving multiple databases/nodes happen reliably and consistently, preserving the ACID properties in a distributed environment.
