## Why Neo4j Can Provide ACID

Neo4j is designed as a **transactional graph database** with a write-ahead log and strict concurrency control.  
That’s why it can support full **ACID** properties:

---

### 🔵 Atomicity
- Every transaction is **all-or-nothing**.
- Changes are first written to a transaction log and only committed if the entire transaction succeeds.

---

### 🟢 Consistency
- Constraints (e.g. relationship existence) are checked on commit.
- Transactions that violate graph schema or constraints are **rolled back**.

---

### 🟠 Isolation
- Uses **locking** and **MVCC** (Multi-Version Concurrency Control).
- One transaction never sees partial updates from another.

---

### 🟣 Durability
- Once committed, data is **persisted to disk**.
- Transaction log is flushed before acknowledging the commit.

---

### 💡 Summary
Unlike some other NoSQL graph databases that focus on eventual consistency, **Neo4j mimics relational DBMS** features under the hood to guarantee ACID.  
This makes it suitable for **OLTP-style workloads** where **strict data integrity** matters.

## Cypher Overview: Edges and Relationship Types

In Cypher, relationships between nodes can be specified in different ways:

- `-->` and `<--` are used for **directed edges**.
- `--` is used for **undirected relationships**.  
  *(The database still stores a direction, but `--` means we "don't care" which way for query purposes.)*

---

### Variables and Types

- `[...]` denotes a **relationship pattern**.
- `[rel]` is a **relationship variable** you can use to refer to that edge later.
- `[:likes]` is a **relationship type**, i.e. all edges labeled `:likes`.
- Relationship variables and types can be **combined** inside the brackets:
  - `-[rel:likes]->` matches an outgoing `:likes` relationship and names it `rel`.
  - `-[ :likes ]->` matches an outgoing `:likes` relationship without naming it.
  - `-[rel]->` matches any outgoing relationship and names it `rel`.

---

### Example

```cypher
// Match any directed :likes edge between person and food
MATCH (person)-[rel:likes]->(food)
RETURN person, food, rel
```
## Difference Between `MATCH` and `SELECT`

| Feature          | `MATCH` (Cypher, Neo4j)                            | `SELECT` (SQL, Relational Databases)           |
|------------------|----------------------------------------------------|-----------------------------------------------|
| **Query Language** | Cypher                                            | SQL                                           |
| **Data Model**    | Graphs (nodes and relationships)                  | Tables (rows and columns)                    |
| **Purpose**       | Find **patterns** in a graph                      | Retrieve **rows and columns** from tables     |
| **Syntax**        | `MATCH (n)-[r]->(m) RETURN n, r, m`               | `SELECT column1, column2 FROM table_name ...` |
| **Joins**         | Implicit via graph relationships                  | Explicit `JOIN` clauses required             |
| **Traversal**     | Traversing is a **core concept**                 | Requires `JOIN` operations for traversal     |
| **Primary Focus** | Discovering connections between entities          | Filtering and projecting table data           |

---

### Example Comparison:

#### Cypher (Neo4j)
```cypher
MATCH (person)-[:LIKES]->(food)
RETURN person.name, food.name;
```

## Resource Description Framework (RDF)

**RDF (Resource Description Framework)** is a W3C standard designed to represent knowledge about any kind of **resource**. Its goal is to enable data to be self-descriptive and easily processed by machines.

---

### 🔍 Key Characteristics:
- **Simple language for capturing knowledge**  
  RDF provides a straightforward model to describe data "about" arbitrary things (e.g., people, places, websites).

- **Self-descriptive data**  
  The meaning of data is explicit and not dependent on the original application.

---

### 🎯 Motivations:
1. **Machine-processable information**  
   Data can be consumed and integrated across different environments without relying on proprietary formats.

2. **Linked Open Data**  
   Enables linking information across the Web — for example, describing relationships between different web resources and datasets.

---

> 💡 **In short:** RDF is a **standard for describing and interlinking data** on the web so machines can easily read, share, and understand it.

### Elasticity: Dynamically Requesting Additional Resources for Peak Loads

**Elasticity** refers to a system's ability to automatically scale its computing resources up or down based on current workload demand.

- **Scaling Up:** During periods of high demand (peak loads), the system dynamically provisions additional servers, CPU, memory, or storage to maintain optimal performance and prevent bottlenecks.
- **Scaling Down:** Once the workload decreases, these extra resources are released automatically, avoiding unnecessary costs.
- **Automation:** This process is usually handled by cloud platforms (e.g. AWS EC2 Auto Scaling, Azure VM Scale Sets, Google Cloud Managed Instance Groups) that continuously monitor resource utilization and apply scaling policies in real-time.
- **Benefits:**  
  - Reduces manual intervention  
  - Improves application responsiveness and user experience  
  - Optimizes cost-efficiency by paying only for the resources actually used

In short, **elasticity** enables systems to handle unpredictable workloads seamlessly and cost-effectively.

### CAP Theorem: Examples of (N, W, R) Configurations

Usually, **high availability** in distributed storage systems means choosing `N > 2`.  
In most cases, `N = 3` (triplication) is the standard setup.

#### ⚡ Systems with an exclusive focus on fault tolerance
- **Configuration:** `N = 3`, `W = 2`, `R = 2`
- **Goal:** Ensure that the system can tolerate one node failure.
- **Effect:** Strong consistency (`W + R > N`) is achieved — at least one up-to-date node is read.

#### 📖 Systems that aim at supporting high read loads
- **Configuration:** `N = 3` or even higher (e.g. up to hundreds of nodes), `R = 1`
- **Goal:** Maximize read throughput and minimize read latency.
- **Effect:** Reads hit only one replica — very fast, but can return stale data.

#### 🏅 Systems that aim at providing a high degree of consistency
- **Configuration:** `W = N` (e.g. `N = 3`, `W = 3`)
- **Goal:** Every write must be acknowledged by all replicas.
- **Effect:** Highest level of consistency, but lower success rate for writes if one node is slow or unreachable.

#### 🧭 Systems that aim at providing high fault tolerance but not consistency
- **Configuration:** `N = 3`, `W = 1` (e.g. a single master node)
- **Goal:** Maximize availability and fault tolerance with lazy replication.
- **Effect:** Writes succeed quickly, but replicas may lag behind — eventual consistency.

#### ✨ …and many more different combinations
You can fine-tune `N`, `W`, and `R` to strike a balance between:
- Read latency vs. consistency
- Write durability vs. availability
- Fault tolerance vs. performance

Each combination reflects a different trade-off as described by the **CAP theorem**.

### CAP Theorem: Examples of (N, W, R) Choices

Concrete values of `N`, `W`, and `R` depend on which property you want to optimize — **read latency**, **write latency**, **consistency**, or **availability**.

#### 🔍 Optimizing for **read performance**:
- **Configuration:** `R = 1`, `W = N`
- **Effect:**  
  ✅ Reads are very fast — only one node is contacted.  
  ✅ Writes wait for all replicas (`W = N`), ensuring all nodes are up to date before returning.
  ⚠️ But writes will be **slower** and less available.

#### 🚀 Optimizing for **write performance**:
- **Configuration:** `W = 1`, `R = N`
- **Effect:**  
  ✅ Writes return immediately after one node acknowledges.  
  ⚠️ Durability is **not guaranteed** — if that one node fails before other replicas receive the data, the write is lost.  
  ⚠️ High read latency — must contact all nodes to read the most up-to-date value.

#### ⚠️ Potential consistency issue:
- If `W ≤ N/2`, then **conflicting writes can happen** because there’s no **quorum overlap**:
  - Write sets might not overlap.
  - Readers can see stale data or different versions at different nodes.

---

### ✅ Rule of thumb for strong consistency:
> Choose `W + R > N`.  
> This ensures at least one node is always common between read and write sets — so every read will see the most recently acknowledged write.

### 🔄 Sequential Consistency
- Corresponds to the **serializability** criterion in databases (specifically **conflict-preserving serializability (CPSR)**).
- Intuitively:
  - The result of all operations is as if they were executed in some **serial order**.
  - Every process observes **the same global order of all operations**.
- ✅ This is considered the **highest form of consistency**.

---

### 🧭 Causal Consistency
- Only writes that are **causally related** must be observed in the **same order** on all nodes.
  - Example of causal relation: one write depends on the value produced by a previous write.
- Writes that are **not causally related**:
  - ✅ May be observed in different orders on different nodes.
  - ✅ This allows more concurrency and better availability.
- ⚠️ Causal consistency is **weaker** than sequential consistency, but still prevents violations of cause-and-effect.

---

### 🎯 Summary
- **Sequential consistency** = all replicas see the same order for all writes.
- **Causal consistency** = all replicas see the same order for writes that are causally linked, but unrelated writes can be reordered.

### 🔄 Monotonic Read Consistency
- Once a client has read a particular version of a data record,  
- Any subsequent read by the **same client** will return a version that is **at least as fresh as the previously read version**.
- ✅ Prevents going backwards in time — no client will ever see older data than they’ve already seen.

---

### 📝 Monotonic Write Consistency
- If a particular client updates the **same data item multiple times**,  
- The system must ensure those writes are **applied in exactly the order they were issued**.
- ✅ This preserves the client’s order of operations.

---

### 🔍 Read-Your-Write Consistency
- After a client writes (or updates) a record,  
- Any **subsequent read** of that record by the **same client** will return the updated version.
- ✅ Guarantees that a client will never read its own stale writes.

### 📝 Writes-Follow-Reads Consistency
- If a **transaction** performs a write on object `x` after reading `x`,  
- The write is guaranteed to take place on the **same version or a more recent version** of `x` that was read.
- ✅ Prevents overwriting newer updates accidentally with stale data.

---

### 🔄 Session Consistency
- Provides **Read-Your-Write consistency** but **scoped to the lifetime of a client session**.
- ✅ Within a single session, a client will always see its own writes reflected in subsequent reads.
- ⚠️ Across different sessions or clients, this guarantee does not necessarily hold.
### GFS achieves consistency
GFS achieves consistency through lazy replication and an append-only design: the master designates a primary **chunkserver to serialize all writes**, especially appends, while clients send data in parallel to all replicas. **The primary dictates the order of updates and forwards them to secondaries**, ensuring all replicas apply changes identically. Locks at the master prevent concurrent conflicting writes, and the decoupled data transfer and ordering allow high throughput and fault tolerance across commodity hardware.

The client must ask the master for the chunk server addresses because the master holds the metadata mapping files to chunks and chunks to their current replica locations. By querying the master first, the client learns which chunk servers store the data it needs, allowing the client to read or write data directly to those servers. This avoids bottlenecking the master with file content and lets the master focus solely on lightweight metadata management, making the system much more scalable.

- **Append targets a specific chunk:**  
  Files in GFS are divided into fixed-size chunks (typically 64 MB). An append operation adds data only to the current active chunk that has space.

- **Not all chunks are modified:**  
  Other chunks of the same file remain unchanged during an append; only the chunk receiving the new data is updated.

- **Data sent to all replicas of that chunk:**  
  To maintain consistency and fault tolerance, the client sends the appended data to all replicas of the target chunk simultaneously.

- **Chunk rollover:**  
  When the current chunk is full, new data is appended to the next chunk in the file sequence.
