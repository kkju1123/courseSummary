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

## Distributed Control

Distributed control focuses on how the **application layer** is organized and deployed across multiple nodes.

---

### 🔀 Partitioned Application Services
- The application is split into **smaller, focused services** that handle different concerns.
- Services can be **local or regional**, allowing for **local adaptations**.
- **Synchronization** happens at the **application layer** to keep services coordinated.
- Each partitioned service may maintain its own local state or data.
- To ensure consistency and proper operation, these services communicate and synchronize through well-defined protocols or messaging.
- This coordination can involve:
  - Sharing state updates,
  - Coordinating transactions,
  - Managing dependencies between services.

By handling synchronization at the application layer, distributed systems can maintain flexibility, support local adaptations, and isolate faults within individual services.
- Benefits:
  - Improved **scalability** by distributing workload.
  - Better **fault isolation** — failure in one service doesn't bring down others.

---

### 🧬 Replicated Application Services
- The **entire application** is deployed **in multiple locations**.
- This increases **availability** and/or **scalability** of the service.
- **Synchronization** happens mainly at the **database layer** to keep data consistent.
- Enables **load balancing** — user requests are distributed among replicas.

---

### 🔄 Combination of Partitioning and Replication
- Many distributed systems combine both approaches:
  - **Partition services** by functionality or region.
  - **Replicate critical services or data** to increase availability and balance load.

---

> 💡 **Summary:**  
> Distributed control designs how applications are divided and deployed, balancing scalability, availability, and fault tolerance through partitioning and replication.

## Examples of Service Partitioning, Replication, and Their Combination

---

### 🔀 Service Partitioning (Partitioned Application Services)
- Example: **Microservices architecture**  
  - An e-commerce app splits functionality into services like:  
    - User Service (handles user accounts)  
    - Product Service (manages product catalog)  
    - Order Service (handles orders and payments)  
  - Each service runs independently, can be deployed to different servers or regions.  
  - Allows local adaptations and fault isolation per service.

---

### 🧬 Service Replication (Replicated Application Services)
- Example: **Content Delivery Networks (CDNs)**  
  - The entire web application or static assets are replicated across multiple geographic data centers.  
  - User requests are routed to the nearest or fastest replica.  
  - Improves availability and load balancing.

---

### 🔄 Combination of Partitioning and Replication
- Example: **Global-scale social media platform**  
  - Core services (e.g., user profiles, posts, notifications) are partitioned by functionality (microservices).  
  - Each microservice is replicated across multiple data centers globally.  
  - This enables:  
    - Scalability by distributing services and data.  
    - High availability and low latency through replication.  
    - Fault tolerance via isolated failures and backups.

---

> 💡 **Summary:**  
> - **Partitioning** breaks the app into focused components deployed independently.  
> - **Replication** copies services/data for availability and load balancing.  
> - Together, they build scalable, resilient distributed applications.

## What are Distributed Information Systems?

Distributed Information Systems are **three- or multi-tier systems** where at least the **data management layer** or the **application layer** is distributed—either **physically**, **logically**, or both—across different nodes of a network.

- **Physically distributed:** Components (data or application) run on different machines or servers.
- **Logically distributed:** Data or services appear as a single, unified system to end-users.

### Two main types of Distributed Information Systems:
- **Distributed Databases:** Focus on distributing data storage and management.
- **Distributed Control:** Focus on distributing application logic and services.

## Scaling Systems and Applications

Scaling means **increasing a system's capacity** to handle more traffic, data, or computations by **upgrading hardware or adding more resources.**

### Types of Scaling

- **Vertical Scaling (Scaling Up)**  
  - Increasing the power of a **single machine** (e.g., more CPU, RAM, faster disks).  
  - Simpler but limited by the capacity of one machine.

- **Horizontal Scaling (Scaling Out)**  
  - Adding more machines or nodes to distribute the load.  
  - More complex but can handle much larger scale and provides fault tolerance.

### Vertical Scaling (Scaling Up)

**Definition:**  
Increasing a system’s capacity by upgrading existing hardware (e.g., adding more CPU, RAM, or storage) or replacing the system with a more powerful one.

**Example:**  
A company upgrades its database server from **64GB RAM** to **256GB RAM** to handle more workload.

**Advantages:**  
- Easier to implement  
- No need to change the implementation logic

**Limitations:**  
- A single machine can only be upgraded to a certain extent  
- Costs often increase **exponentially** as upgrades get more powerful

### Horizontal Scaling (Scaling Out)

**Definition:**  
Expanding capacity by adding more machines (servers or nodes) to the system.

**Example:**  
Instead of upgrading a single powerful database server, an organization distributes data across multiple servers.

**Advantages:**  
- Not capped by the physical limits of a single machine  
- Often more **cost-effective** than continually upgrading one server

**Considerations:**  
- Requires distributing the system across multiple nodes  
- Involves changes to the implementation and coordination (e.g., load balancing, data partitioning, consistency management)

### Distribution is Not Only About Scaling

Besides **scalability**, there are several other reasons to distribute an Information System:

- **Latency Reduction**  
  Distributed systems place resources **closer to users**, reducing network delays and improving response times.

- **High Availability**  
  If one server fails, another can take over. This increases fault tolerance and keeps the system running even under failures.

- **Maintainability**  
  Distributed systems make it easier to update, monitor, and troubleshoot individual components without affecting the entire system.

## Evolution of Information Systems

### Traditional View
Information Systems (IS) were traditionally developed based on:
- **Database Management Systems (DBMS)**  
- **Programming environments** for data handling and business logic.

---

### Modern Information Systems
Today's IS are distributed and require concepts and technologies from multiple areas of computer science:
- **Communication Systems**  
- **Software Engineering / Software Architectures**  
- **Database Systems**

As a result, the **infrastructure of today's distributed IS** is more complex.  
There is a greater need for **middleware** and other components that "glue" together the different building blocks.

---

### Evolution of Information Systems
This evolution can be thought of along three key axes:
- **Communication axis** — advances in networking and distributed protocols
- **Software axis** — advances in software engineering practices and architectures
- **Data axis** — advances in data management, distributed databases, and replication

These axes reflect the increasing complexity and richness of modern distributed information systems.

## The “Communication Axis”

### Sockets
- **Low-level communication**  
- Requires manual handling of connections and data transfer  
- Not appropriate for large or complex distributed systems

### Remote Procedure Call (RPC)
- **Type-safe distributed communication**  
- Provides a higher level of abstraction than sockets  
- Server address and procedures must usually be known **a priori**

### Distributed Object Management
- **Object-orientation** and support for heterogeneity  
- Server object address need **not** be known in advance  
- Enables communication between distributed objects

### Dynamically Deployable Code
- Adds **flexibility** and **platform independence**  
- Enables **Write Once, Run Anywhere (WORA)** principles  
- Simplifies updates and maintenance across distributed systems

### Peer-to-Peer Interactions
- No centralized control — all nodes are **equal peers**  
- Support for dynamic node discovery and data exchange  
- Scales easily as nodes join or leave the network

## The “Communication Axis” — Examples

### Sockets
- **Examples**:
  - TCP and UDP socket programming in C, Java (`java.net.Socket`)
  - Raw network programming for custom client-server apps (e.g. basic chat apps)

### Remote Procedure Call (RPC)
- **Examples**:
  - gRPC — cross-language RPC with protocol buffers
  - Java RMI — calling Java methods on a remote JVM
  - XML-RPC — RPC using XML over HTTP

### Distributed Object Management
- **Examples**:
  - CORBA — Common Object Request Broker Architecture
  - Java RMI with dynamic service lookup
  - Microsoft DCOM — Distributed Component Object Model
  - Enterprise JavaBeans (EJB)

### Dynamically Deployable Code
- **Examples**:
  - Java applets — downloadable code that runs in the browser
  - OSGi — dynamic module system for Java
  - Mobile code in distributed agent systems (e.g. IBM Aglets)

### Peer-to-Peer Interactions
- **Examples**:
  - BitTorrent — file sharing with decentralized control
  - Bitcoin — decentralized ledger for cryptocurrency
  - IPFS — peer-to-peer file storage
  - Gnutella — early peer-to-peer file sharing network

## The “Software Axis”

### Software Systems / Programs
- Originally **not interactive** — single-shot programs
- **Monolithic**, not easily extendable
- Earmarked for a specific purpose or domain
- **Examples**:
  - Early desktop applications like Microsoft Word 97
  - Standalone calculation tools or custom scripts

### Services
- Decouple application functionality into **modular services**
- Support multiple users and sessions
- Re-use of existing services in different contexts
- **Examples**:
  - Web services (REST/SOAP), such as a payment processing service
  - Microservices architecture for e-commerce (e.g. product catalog, shipping, payment as separate services)

### Process-aware Systems
- Application-independent, **standardized services** that hide heterogeneity
- Support transactions and orchestrations across multiple components
- Allow **service composition** into business processes or workflows
- **Examples**:
  - BPMN-based workflows (e.g. invoice approval process in Camunda)
  - SAP ERP systems integrating finance, HR, and logistics via workflows
  - Enterprise Service Bus (ESB) systems

### Virtualization
- Sharing of CPU, storage, and application services across tenants
- Elastic and dynamic resource provisioning — scale up or down on demand
- **Examples**:
  - Virtual Machines (e.g. VMware, VirtualBox)
  - Cloud services like AWS EC2, Azure Virtual Machines
  - Containers and orchestrators (e.g. Docker, Kubernetes) for microservice deployments

## The “Data Axis”

### Relational Database Systems
- Store data in **relational tables** with a fixed schema
- Emphasis on ACID transactions and SQL
- **Examples**:
  - MySQL, PostgreSQL, Oracle Database
  - Traditional banking or inventory systems

### Object-Relational Database Systems
- Combine relational model with **object-oriented concepts** (types, inheritance)
- Support for **methods and attributes** inside the database
- **Examples**:
  - PostgreSQL with custom types
  - Oracle’s object-relational features
  - IBM Db2 Object-Relational Extensions

### Distributed Database Systems
- Multiple databases across different nodes — can be **geographically distributed**
- Applications access data from **several databases** transparently
- **Examples**:
  - Google Spanner, CockroachDB
  - Federated databases or data virtualization solutions

### Big Data
- Focus on **scalability and flexibility** over strict schema
- Use of **NoSQL** and **data streams** for real-time data
- **Examples**:
  - MongoDB, Cassandra (NoSQL document/column databases)
  - Apache Kafka, Apache Flink (streaming platforms)
  - Hadoop ecosystem (HDFS, Spark)

### Higher-order Database Systems
- Database systems that **manage other databases or services**
- Provide **meta-level services** (e.g. monitoring, orchestrating other databases and applications)
- **Examples**:
  - Kubernetes Operators for databases
  - Service meshes managing distributed microservices
  - AWS RDS or Azure SQL managed database services

## One-Tier Architecture

### Definition
- **One-tier architecture** is the simplest software architecture.
- All components (user interface, business logic, and data management) run **on a single machine**.
- Typically a **monolithic application** where the user interacts directly with the system.

### Characteristics
- No network distribution — all data and logic are local.
- Easier to design and deploy but **harder to scale**.
- Changes require updating the entire application.
- Poor fault tolerance (one machine is a single point of failure).

### Examples
- A standalone desktop application like **Microsoft Excel** or **Photoshop**.
- Personal databases like **MS Access** that contain the UI and data together.
- Early video games that read data and present graphics on the same computer.

---

✅ **Use cases**: Suitable for personal use, small tools, or prototype software where scalability and distributed features are not a concern.

# Two-Tier Client/Server Architectures

## Definition
- In two-tier architecture, the system is split into two parts:
  1. **Client** (front-end): User interface and some business logic
  2. **Server** (back-end): Database management and sometimes additional business logic
- The client communicates directly with the server over a network.

## Characteristics
- The client handles the presentation layer (UI) and possibly some processing.
- The server manages data storage and query processing.
- Communication is usually done via database queries or API calls.
- Common in desktop applications connected to a centralized database.

## Advantages
- Simpler than multi-tier architectures.
- Clear separation between user interface and data storage.
- Easier to develop and maintain than monolithic (one-tier) systems.

## Disadvantages
- Limited scalability: The server can become a bottleneck with many clients.
- Business logic may be duplicated or split between client and server, complicating maintenance.
- Tight coupling between client and server.

## Examples
- A desktop application (e.g., a bank teller app) connecting directly to a database server (e.g., Oracle DB).
- Classic client-server database applications using **ODBC/JDBC**.
- Early email clients communicating directly with an email server (e.g., Microsoft Outlook with Exchange Server).

---

✅ **Summary**: Two-tier architecture moves the data management to a separate server but keeps the client relatively “thick” with UI and some logic.
