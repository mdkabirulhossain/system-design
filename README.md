# 🏗️ Complete Guide to System Design

Welcome to the **System Design Master Guide**. This repository provides an easy-to-understand, comprehensive breakdown of **System Design**, its **Types (High-Level Design vs. Low-Level Design)**, core architectural patterns, **Functional vs. Non-Functional Requirements**, and **multiple practical real-world examples**.

---

## 📌 Table of Contents
1. [What is System Design?](#-what-is-system-design)
2. [Why is System Design Important?](#-why-is-system-design-important)
3. [The Core Building Blocks of System Architecture](#-the-core-building-blocks-of-system-architecture)
4. [Types of System Design](#-types-of-system-design)
   - [1. High-Level Design (HLD)](#1-high-level-design-hld---the-architectural-blueprint)
   - [2. Low-Level Design (LLD)](#2-low-level-design-lld---the-detailed-code-blueprint)
5. [HLD vs LLD Comparison](#-hld-vs-lld-comparison)
6. [Step 1: Fundamentals (HLD Roadmap)](#-step-1-fundamentals-hld-roadmap)
   - [1.1 Serverless vs Serverful Architecture](#11-serverless-vs-serverful-architecture)
   - [1.2 Horizontal vs Vertical Scaling](#12-horizontal-vs-vertical-scaling)
   - [1.3 What are Threads?](#13-what-are-threads)
   - [1.4 What are Pages? (OS Paging & DB Pagination)](#14-what-are-pages-os-paging--db-pagination)
   - [1.5 How Does the Internet Work?](#15-how-does-the-internet-work)
7. [Step 2: Databases (HLD Roadmap)](#-step-2-databases-hld-roadmap)
   - [2.1 SQL vs NoSQL Databases](#21-sql-vs-nosql-databases)
   - [2.2 In-Memory Databases & Caching](#22-in-memory-databases--caching)
   - [2.3 Data Replication & Migration](#23-data-replication--migration)
   - [2.4 Data Partitioning](#24-data-partitioning)
   - [2.5 Database Sharding](#25-database-sharding)
8. [Functional vs Non-Functional Requirements](#-functional-vs-non-functional-requirements)
9. [Multiple Real-World Examples](#-multiple-real-world-examples)
10. [Key System Design Trade-offs & Concepts](#-key-system-design-trade-offs--concepts)
11. [How to Approach a System Design Interview / Problem](#-how-to-approach-a-system-design-problem)

---

## 💡 What is System Design?

**System Design** is the process of defining the **architecture, modules, interfaces, and data flow** for a software application to satisfy specific business and technical requirements.

### 🏠 The Building Analogy
Imagine you want to construct a 100-story skyscraper:
* **Without Planning**: You start laying bricks. At floor 5, the foundation cracks because it cannot support the weight. The building collapses.
* **With System Design**: Before building, architects calculate load capacity, design structural columns, plan elevator shafts, plumbing, and electrical grids. 

In software, system design ensures that when your application grows from **100 users** to **10,000,000 users**, it stays fast, secure, and never crashes.

---

## 🚀 Why is System Design Important?

When building small apps, a single server with a basic database is enough. But enterprise software faces massive challenges:

| Challenge | Without System Design | With System Design |
| :--- | :--- | :--- |
| **Traffic Spikes** | Server crashes during flash sales | **Load Balancers** distribute traffic across hundreds of servers |
| **Database Slowness** | Database freezes under millions of queries | **Caching (Redis)** serves 90% of requests in memory in <5ms |
| **Data Failures** | Hard drive failure loses all customer data | **Database Replication & Sharding** ensures zero data loss |
| **System Outages** | One bug takes down the entire application | **Microservices Architecture** isolates faults so app stays online |

---

## 🏛️ The Core Building Blocks of System Architecture

Before diving into HLD and LLD, here are the primary components used in system design:

```
[ Client (Mobile/Web) ]
           │
           ▼
     [ DNS / CDN ]
           │
           ▼
   [ Load Balancer ]
           │
     ┌─────┴─────┐
     ▼           ▼
[ API Server 1 ] [ API Server 2 ]
     │           │
     ├───────────┼──────────────┐
     ▼           ▼              ▼
 [ Cache ]  [ Database ]  [ Message Queue ]
 (Redis)     (PostgreSQL)    (Kafka)
```

1. **Client**: The web browser, mobile app, or IoT device interacting with the user.
2. **DNS (Domain Name System)**: Translates human-readable names (e.g., `google.com`) into IP addresses.
3. **CDN (Content Delivery Network)**: Caches static content (images, videos, CSS) on servers globally near users.
4. **Load Balancer**: Distributes incoming HTTP requests across multiple web servers (e.g., NGINX, HAProxy, AWS ALB).
5. **API Gateway / Web Servers**: Executes business logic and security checks.
6. **In-Memory Cache**: Ultra-fast RAM storage (e.g., Redis, Memcached) to reduce database load.
7. **Database**: Persistent storage (SQL like PostgreSQL/MySQL or NoSQL like MongoDB/Cassandra).
8. **Message Queue**: Enables asynchronous processing (e.g., RabbitMQ, Apache Kafka) so heavy tasks run in the background.

---

## 📐 Types of System Design

System Design is divided into **two core categories**:

### 1. High-Level Design (HLD) - *The Architectural Blueprint*

High-Level Design focuses on the **macro-architecture** ("the big picture"). It describes the system components, how they interact, and how data moves across networks.

#### Key Aspects of HLD:
* System architecture & component placement.
* Choice of technology stack (SQL vs NoSQL, REST vs WebSockets vs gRPC).
* Data flow diagrams between clients, servers, databases, and queues.
* Scalability, reliability, and security strategies.
* Hardware and cloud network topology.

---

### 2. Low-Level Design (LLD) - *The Detailed Code Blueprint*

Low-Level Design focuses on the **micro-architecture** ("the detailed implementation"). It defines how individual components will be implemented in code.

#### Key Aspects of LLD:
* Class diagrams and Object-Oriented Design (OOD).
* Database schemas (table structures, indexes, foreign key relationships).
* API interface contracts (REST endpoints, JSON request/response formats, status codes).
* Data structures and algorithmic choices within modules.
* Application of software **Design Patterns** (e.g., Singleton, Factory, Strategy, Observer).

---

## ⚖️ HLD vs LLD Comparison

| Dimension | High-Level Design (HLD) | Low-Level Design (LLD) |
| :--- | :--- | :--- |
| **Focus** | System Architecture & Infrastructure | Object Design & Code Structure |
| **Question Answered** | *WHAT components are needed?* | *HOW to write the code for each component?* |
| **Target Audience** | System Architects, Tech Leads, DevOps | Software Developers, Code Reviewers |
| **Primary Diagrams** | Flowcharts, Component Diagrams, Network Maps | Class Diagrams, Sequence Diagrams, ER Diagrams |
| **Key Deliverable** | Architecture Document, Infrastructure Setup | API Specifications, DB Schemas, Source Code |
| **Example Topic** | Choosing Kafka vs RabbitMQ for async jobs | Implementing the Observer Pattern for email alerts |

---

## 🚀 Step 1: Fundamentals (HLD Roadmap)

High-Level Design begins with core infrastructure concepts that determine how application servers scale, process background work, allocate memory, and route network packets.

---

### 1.1 Serverless vs Serverful Architecture

Engineers must choose how application code will be executed: using **Serverful (Traditional Dedicated Servers)** or **Serverless (Function-as-a-Service / Managed Cloud)**.

#### 🚗 The Apartment vs. Taxi Analogy
* **Serverful (Renting an Apartment)**: You pay monthly rent regardless of whether you are home, sleeping, or away on vacation. You handle maintenance and utilities.
* **Serverless (Booking an Uber/Taxi)**: You don't own or maintain the vehicle. You request a ride, get driven to your destination, and pay strictly for the distance/time traveled. When idle, you pay **$0**.

```
[ Serverful ]  : Client ──► Load Balancer ──► EC2 Server (Runs 24/7) ──► DB
[ Serverless ] : Client ──► API Gateway ──► AWS Lambda (Runs on demand) ──► DB
```

| Feature | Serverful Architecture | Serverless Architecture |
| :--- | :--- | :--- |
| **Management** | You manage OS, security patches, and VMs | 100% Cloud Provider Managed |
| **Cost Model** | Fixed hourly / monthly instance rate | Pay per millisecond of execution time |
| **Idle Cost** | Pay 100% full server price | Pay **$0** (Zero cost when idle) |
| **Scaling** | Rule-based Auto Scaling Groups (takes minutes) | Instant auto-scaling (0 to 10,000+ in seconds) |
| **Cold Starts** | None (Server process runs 24/7) | Potential latency on idle function wakeup |
| **Max Run Time**| Unlimited (runs 24/7) | Time-bounded (e.g., 15-min AWS Lambda limit) |

---

### 1.2 Horizontal vs Vertical Scaling

When application traffic grows from 100 to 1,000,000 users, systems must scale up or out.

#### 🚚 The Bigger Truck vs. Van Fleet Analogy
* **Vertical Scaling (Scale Up)**: Buying a bigger truck with a stronger engine to carry heavier cargo.
* **Horizontal Scaling (Scale Out)**: Buying 10 delivery vans to split cargo across 10 drivers.

```
[ Vertical Scaling ]  : [ Server 8GB RAM ] ──► [ Server 128GB RAM (Upgraded) ]
[ Horizontal Scaling]: [ Load Balancer ] ──► [ Server 1 ] + [ Server 2 ] + [ Server 3 ]
```

#### Comparison Table:
| Dimension | Vertical Scaling (Scale Up) | Horizontal Scaling (Scale Out) |
| :--- | :--- | :--- |
| **Approach** | Add more CPU/RAM to 1 existing machine | Add MORE machine instances to pool |
| **Hardware Capacity**| Physical hardware ceiling limit | Near-infinite scale |
| **Fault Tolerance** | Low (Single Point of Failure - SPOF) | High (Traffic routes away from dead nodes) |
| **Downtime** | Required during hardware upgrades | Zero-downtime rolling deployments |
| **Application Type**| Monolithic applications | Stateless Microservices |

---

### 1.3 What are Threads?

Understanding OS processes and threads is essential for designing concurrent servers.

#### 👩‍🍳 The Restaurant Kitchen Analogy
* **Process**: The entire kitchen facility with its own isolated building space and tools.
* **Thread**: Individual chefs working inside the kitchen, sharing the countertop space, refrigerators, and ovens to prepare dishes simultaneously.

```
┌─────────────────────────── PROCESS ───────────────────────────┐
│ Memory Address Space (Heap, Code, Global Variables)           │
│                                                               │
│  ┌──────────────┐      ┌──────────────┐      ┌──────────────┐ │
│  │   Thread 1   │      │   Thread 2   │      │   Thread 3   │ │
│  │ (Stack/Regs) │      │ (Stack/Regs) │      │ (Stack/Regs) │ │
│  └──────────────┘      └──────────────┘      └──────────────┘ │
└───────────────────────────────────────────────────────────────┘
```

#### Core Concepts:
1. **Concurrency vs Parallelism**:
   * *Concurrency*: Juggling multiple tasks on a single CPU core via rapid context switching.
   * *Parallelism*: Executing multiple tasks at the exact same instant on separate CPU cores.
2. **Context Switching**: The OS saving state of a thread and restoring state of another. High context switching overhead slows down servers.
3. **Thread Safety & Race Conditions**: Unsynchronized concurrent writes to shared memory cause data corruption. Solved via Locks, Semaphores, or Mutexes.
4. **Concurrency Models**:
   * *Thread-Per-Request* (Java Spring/Tomcat): 1 dedicated thread per HTTP connection. High RAM consumption at scale.
   * *Single-Threaded Event Loop* (Node.js/Redis/NGINX): Non-blocking I/O event loop handles 10,000+ connections on 1 thread.

---

### 1.4 What are Pages? (OS Paging & DB Pagination)

In System Design, "Pages" refers to **OS Virtual Memory Paging** and **Database Query Pagination**.

#### A. OS Virtual Memory Paging
* **Concept**: Physical RAM is divided into fixed blocks called **Frames**, and Virtual Memory is divided into matching **Pages** (typically 4KB).
* **Page Table**: Managed by OS/MMU to map Virtual addresses to Physical RAM.
* **Page Fault**: When a requested memory page is missing from physical RAM and must be fetched from disk (Swap space) $\rightarrow$ causes disk I/O latency.

#### B. Database Result Pagination
When querying millions of records, fetching all rows crashes client browsers. Data must be paginated:

1. **Offset-Based Pagination**:
   ```sql
   SELECT * FROM orders ORDER BY created_at DESC LIMIT 20 OFFSET 10000;
   ```
   * *Problem*: Database reads first 10,020 rows and discards 10,000. $O(N)$ slowdown on deep pages!
2. **Cursor-Based / Keyset Pagination (High-Performance Choice)**:
   ```sql
   SELECT * FROM orders WHERE id > 10000 ORDER BY id ASC LIMIT 20;
   ```
   * *Advantage*: Uses database index directly. Performs at $O(1)$ constant speed regardless of page depth.

---

### 1.5 How Does the Internet Work? (Step-by-Step Request Flow)

When a user opens a browser and types `https://example.com`, the request flows through 5 main steps:

```
[ User Browser ] ──1. DNS Lookup──► [ DNS Resolver ] (IP: 93.184.216.34)
       │
       ├──2. TCP 3-Way Handshake──► (SYN ➔ SYN-ACK ➔ ACK)
       ├──3. TLS Encryption Handshake (HTTPS Security Key)
       ├──4. HTTP Request GET / ──► [ CDN / Load Balancer ] ──► [ API Server ]
       └──5. Render HTML/DOM ◄──── [ 200 OK Response ]
```

1. **DNS Resolution**: Browser checks Browser Cache $\rightarrow$ OS Cache $\rightarrow$ Router $\rightarrow$ ISP DNS Resolver $\rightarrow$ Root/TLD DNS Servers $\rightarrow$ Returns Server IP Address (`93.184.216.34`).
2. **TCP 3-Way Handshake**: Client sends `SYN` $\rightarrow$ Server replies `SYN-ACK` $\rightarrow$ Client sends `ACK`. Connection established!
3. **TLS/SSL Handshake**: Negotiates asymmetric cipher keys (RSA/Diffie-Hellman) to secure HTTPS payload via symmetric AES-256 encryption.
4. **HTTP Request/Response Cycle**: Browser sends `GET /index.html`. Request passes through CDN $\rightarrow$ Load Balancer $\rightarrow$ Web Server $\rightarrow$ Database. Server returns `200 OK` + HTML.
5. **Browser Rendering Pipeline**: Browser parses HTML to build DOM Tree $\rightarrow$ CSS to build CSSOM Tree $\rightarrow$ Combines into Render Tree $\rightarrow$ V8 executes JS $\rightarrow$ Paints pixels on screen.

---

## 🗄️ Step 2: Databases (HLD Roadmap)

Choosing the correct database architecture determines data consistency, availability, and storage throughput.

---

### 2.1 SQL vs NoSQL Databases

#### 📂 Spreadsheets vs Document Folders Analogy
* **SQL (Relational)**: Structured Excel spreadsheets with strict rows, columns, and foreign key relationships.
* **NoSQL (Non-Relational)**: Unstructured folders containing dynamic JSON documents or Key-Value pairs.

```
[ SQL ]   : Tables with Rigid Schema (Row x Column) + Foreign Keys + ACID
[ NoSQL ] : JSON Documents / Key-Value Stores + Dynamic Schema + BASE
```

| Feature | SQL Databases (RDBMS) | NoSQL Databases (Non-Relational) |
| :--- | :--- | :--- |
| **Examples** | PostgreSQL, MySQL, Oracle | MongoDB, Cassandra, Redis, DynamoDB |
| **Data Schema** | Rigid, pre-defined tabular schema | Flexible, schema-less (JSON, Key-Value) |
| **Consistency** | Strict **ACID** (Atomicity, Consistency, Isolation, Durability) | **BASE** (Basically Available, Soft-state, Eventual) |
| **Scaling** | Primary Vertical Scaling; Read-Replicas | Native Horizontal Scaling (Auto-sharding) |
| **Best For** | Financial systems, ERP, E-commerce Checkout | Real-time social feeds, Analytics, Big Data |

---

### 2.2 In-Memory Databases & Caching

#### 📝 Desk Notepad vs Basement File Cabinet Analogy
* Storing active work on your desk (RAM) provides instant access compared to walking down to a basement filing cabinet (Disk Database) every time.

```
[ Client ] ──► [ App Server ] ──1. Check Cache──► [ In-Memory Cache (Redis) ]
                     │                            (< 1ms Response)
                     └──2. Cache Miss ─────────► [ Disk Database (PostgreSQL) ]
```

#### Caching Strategies:
1. **Cache-Aside (Lazy Loading)**: App reads Cache. If Miss $\rightarrow$ reads DB $\rightarrow$ writes to Cache $\rightarrow$ returns data. (Most popular).
2. **Write-Through**: App writes to Cache $\rightarrow$ Cache synchronously writes to DB. (Guarantees data consistency).
3. **Write-Back (Write-Behind)**: App writes to Cache $\rightarrow$ Cache asynchronously writes to DB in background batches. (Ultra-fast writes, but risk of data loss on power crash).

#### Cache Eviction Policies:
* **LRU (Least Recently Used)**: Evicts keys that haven't been requested for the longest time.
* **LFU (Least Frequently Used)**: Evicts keys with the lowest total access frequency.
* **TTL (Time To Live)**: Key automatically expires after X seconds.

---

### 2.3 Data Replication & Migration

Replication photocopies data across multiple database nodes to prevent data loss and increase read throughput.

```
[ Single-Leader ] : [ Master (Writes) ] ──► [ Replica 1 (Reads) ] + [ Replica 2 (Reads) ]
```

#### Replication Architecture Models:
1. **Single-Leader (Master-Replica)**: 1 Master Node processes all Writes; multiple Replicas process Reads. (Ideal for read-heavy apps like Twitter/Reddit).
2. **Multi-Leader (Master-Master)**: Multiple masters across different data centers process Writes. (Requires conflict resolution).
3. **Leaderless (Cassandra/Dynamo)**: Any node accepts Writes and Reads. Uses Quorum Voting ($R + W > N$) to ensure consistency.

#### Zero-Downtime Migration Strategy:
1. **Dual-Writing**: Application writes all new incoming data to BOTH Old DB & New DB simultaneously.
2. **Backfill**: Copy past historical data from Old DB to New DB via Change Data Capture (CDC).
3. **Verify**: Run checksum validation to ensure data identity.
4. **Switch Reads**: Route client read queries to New DB.
5. **Deprecate**: Stop dual-writing and decommission Old DB.

---

### 2.4 Data Partitioning

Partitioning splits a massive single dataset into smaller, isolated subsets to accelerate queries.

```
[ Vertical Partitioning ]   : Table A (id, email) | Table B (id, heavy_bio_text, avatar_blob)
[ Horizontal Partitioning ] : Partition 1 (IDs 1-1M) | Partition 2 (IDs 1M-2M)
```

1. **Vertical Partitioning**: Splitting a table by **COLUMNS**. Keeps frequent lightweight columns together and moves heavy BLOB columns to a separate table.
2. **Horizontal Partitioning**: Splitting a table by **ROWS** using a partitioning strategy:
   * *Range-Based*: Partition by date ranges (e.g. `orders_2024`, `orders_2025`).
   * *Hash-Based*: `Partition = Hash(user_id) % Total_Partitions` (Prevents traffic hotspots).
   * *List-Based*: Partition by explicit categories (e.g. Region: `US`, `EU`, `ASIA`).

---

### 2.5 Database Sharding

Sharding is horizontal partitioning **across separate physical database servers (nodes)**.

```
[ Client Request ] ──► [ Shard Router ] ──► Hash(User_ID)
                                               ├── Shard Node 1 (Users 1-100k)
                                               ├── Shard Node 2 (Users 100k-200k)
                                               └── Shard Node 3 (Users 200k-300k)
```

#### Key Concepts:
1. **Shard Key Selection**:
   * The primary column used to route queries to specific shard nodes (e.g. `user_id`, `tenant_id`).
   * *Good Key*: High cardinality, even data distribution across nodes.
   * *Bad Key*: Low cardinality (e.g. `gender`), creates massive single-node hotspots.
2. **Consistent Hashing**:
   * Uses a 360-degree virtual Hash Ring to assign data to server nodes. Adding or removing a database node only requires remapping $1/N$ of keys instead of rebuilding 99% of the cluster.
3. **Challenges of Sharding**:
   * *Cross-Shard Joins*: SQL `JOIN` across different physical servers is impossible natively; must be handled in application layer.
   * *Distributed Transactions*: Updating Shard A and Shard B requires 2-Phase Commit (2PC) or Saga Pattern.

---

---

## 🎯 Functional vs Non-Functional Requirements

Before designing system architecture, engineers define two fundamental types of requirements:

### 🚗 The Simple Car Analogy
* **Functional Requirement (WHAT it does)**: The car has a steering wheel to turn, a gas pedal to accelerate, and brakes to stop.
* **Non-Functional Requirement (HOW WELL it performs)**: The car accelerates 0-60 mph in 3.5 seconds, gets a 5-star crash safety rating, and maintains 99.9% reliability.

---

### ⚙️ 1. Functional Requirements (FR) - *What the system MUST DO*
Functional Requirements define specific features, user capabilities, and business logic.

#### Key Aspects:
* Specifies input data, processing rules, and output results.
* Easily tested with Pass/Fail test cases.

#### 📌 Examples of Functional Requirements:
1. **E-Commerce (Amazon)**: *"Users can search items, add them to a shopping cart, and pay via credit card."*
2. **Digital Banking**: *"Users can transfer money between accounts and receive a transaction receipt."*
3. **Social Media (Instagram)**: *"Users can upload photos, apply filters, and write comments."*
4. **Ride-Sharing (Uber)**: *"Riders can select a pickup location and request a nearby driver."*
5. **Video Streaming (Netflix)**: *"Users can stream video content and save movies to a watch list."*

---

### ⚡ 2. Non-Functional Requirements (NFR) - *How WELL the system MUST PERFORM*
Non-Functional Requirements define quality attributes, operational limits, security levels, and performance constraints.

#### Key Categories:
* **Performance & Speed**: Response times and page load latencies.
* **Scalability**: Ability to handle traffic growth (e.g., 100,000 requests per second).
* **Availability & Reliability**: System uptime (e.g., 99.99% availability = < 52 mins downtime per year).
* **Security**: Data encryption (TLS/AES-256), authentication, and authorization.

#### 📌 Examples of Non-Functional Requirements:
1. **E-Commerce (Amazon)**: *"Checkout page must render in < 1.5 seconds under peak traffic."* *(Performance)*
2. **Digital Banking**: *"All financial transactions must be encrypted using AES-256 encryption."* *(Security)*
3. **Social Media (Instagram)**: *"The feed service must support 100,000 active concurrent users."* *(Scalability)*
4. **Ride-Sharing (Uber)**: *"GPS location pings must reach rider screens within 2 seconds."* *(Latency)*
5. **Video Streaming (Netflix)**: *"Video playback buffering must start within 2 seconds globally."* *(Performance)*

---

### 📊 FR vs NFR Comparison Table

| Feature | Functional Requirements (FR) | Non-Functional Requirements (NFR) |
| :--- | :--- | :--- |
| **Main Question** | What must the system **DO**? | How well must it **PERFORM**? |
| **Focus** | User features & business rules | Performance, security, uptime & scalability |
| **Primary Audience** | Product Owners & End Users | System Architects & Security Engineers |
| **Testing Type** | Functional, Unit & Integration testing | Performance, Stress, Penetration & Load testing |
| **Failure Result** | Feature fails or breaks | System slows down, crashes, or leaks data |

---

## 🔍 Multiple Real-World Examples

To master System Design, let's explore real-world cases covering both **HLD** and **LLD**.

---

### Example 1: Real-Time Chat System (WhatsApp) - *HLD & LLD*

#### 🌐 High-Level Design (HLD)
```
[ User A ] ──(WebSocket)──► [ API Gateway / Load Balancer ]
                                     │
                                     ▼
                            [ Web Socket Server ] ──► [ Presence Server ]
                                     │
                   ┌─────────────────┴─────────────────┐
                   ▼                                   ▼
        [ User B (Online) ]                     [ Receiver Offline? ]
        Message Delivered                              │
                                                       ▼
                                            [ Message Queue (Kafka) ]
                                                       │
                                                       ▼
                                            [ Push Notification Service ]
                                                       │
                                                       ▼
                                            [ Cassandra Database ]
```

* **Connection**: Long-lived bidirectional WebSockets connection between client and WebSocket Servers.
* **Presence Service**: Tracks online/offline status in Redis.
* **Storage**: Apache Cassandra (NoSQL) for storing billions of messages efficiently due to high write performance.
* **Offline Handling**: If Receiver is offline, message goes to Kafka message queue, triggering Apple APNs / Google FCM Push Notifications.

#### 💻 Low-Level Design (LLD) - Message Schema & API
**Database Schema (`messages` table):**
```sql
CREATE TABLE messages (
    message_id UUID PRIMARY KEY,
    chat_room_id UUID,
    sender_id UUID,
    receiver_id UUID,
    content TEXT,
    media_url VARCHAR(255),
    status VARCHAR(20), -- 'SENT', 'DELIVERED', 'READ'
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);
CREATE INDEX idx_chat_created ON messages(chat_room_id, created_at DESC);
```

---

### Example 2: Video Streaming Platform (YouTube/Netflix) - *HLD*

#### 🌐 High-Level Design (HLD)
When a creator uploads a 4K video, the system must transcode it into multiple resolutions (1080p, 720p, 480p, 360p) and stream it globally.

```
[ Creator ] ──► [ API Server ] ──► [ Blob Storage (AWS S3) ]
                                            │
                                            ▼
                                   [ Upload Event (Kafka) ]
                                            │
                                            ▼
                                  [ Video Transcoder Pool ]
                                   (Splits video into chunks
                                   & generates HLS/DASH formats)
                                            │
                                            ▼
                                  [ CDN Storage (Cloudflare) ]
                                            │
                                            ▼
                                  [ Global Viewers Stream ]
```

1. **Blob Storage**: Raw video uploaded to Amazon S3.
2. **Asynchronous Transcoding**: Video broken into 10-second chunks and encoded in parallel into multiple formats (HLS/DASH).
3. **CDN Distribution**: Transcoded chunks pushed to Edge CDN servers globally so users stream from servers nearest to them.

---

### Example 3: Ride-Sharing Service (Uber) - *HLD*

#### 🌐 High-Level Design (HLD)
Uber handles real-time location tracking for millions of drivers every 4 seconds.

* **Geospatial Indexing**: Earth divided into hexagonal cells using **Google S2** or **Uber H3** spatial index.
* **Location Ingestion Service**: Drivers send GPS ping `(driver_id, lat, long)` to Location Service via UDP/WebSockets.
* **Redis Spatial Cache**: Stores current location of active drivers indexed by GeoHash/H3 cell.
* **Match Service**: Finds the nearest available driver using geospatial queries (e.g., `GEORADIUS` in Redis).

---

### Example 4: Parking Lot System - *LLD*

#### 💻 Low-Level Design (LLD) - Object-Oriented Class Design

Let's design a multi-floor parking lot supporting different vehicle types (Car, Bike, Truck).

```python
from enum import Enum
from abc import ABC, abstractmethod

class VehicleType(Enum):
    BIKE = 1
    CAR = 2
    TRUCK = 3

class Vehicle(ABC):
    def __init__(self, license_plate: str, vehicle_type: VehicleType):
        self.license_plate = license_plate
        self.vehicle_type = vehicle_type

class Car(Vehicle):
    def __init__(self, license_plate: str):
        super().__init__(license_plate, VehicleType.CAR)

class ParkingSpot:
    def __init__(self, spot_id: int, spot_type: VehicleType):
        self.spot_id = spot_id
        self.spot_type = spot_type
        self.is_occupied = False
        self.current_vehicle = None

    def park_vehicle(self, vehicle: Vehicle) -> bool:
        if not self.is_occupied and vehicle.vehicle_type == self.spot_type:
            self.current_vehicle = vehicle
            self.is_occupied = True
            return True
        return False

    def remove_vehicle(self):
        self.current_vehicle = None
        self.is_occupied = False

# Strategy Pattern for Fee Calculation
class FeeStrategy(ABC):
    @abstractmethod
    def calculate_fee(self, hours: float) -> float:
        pass

class CarFeeStrategy(FeeStrategy):
    def calculate_fee(self, hours: float) -> float:
        return hours * 10.0  # $10/hour
```

---

## ⚡ Key System Design Trade-offs & Concepts

### 1. CAP Theorem
In a distributed database, you can choose at most **TWO** of the following three guarantees:
* **Consistency (C)**: Every read receives the most recent write or an error.
* **Availability (A)**: Every request receives a non-error response without guarantee that it contains the latest write.
* **Partition Tolerance (P)**: The system continues to operate despite network message losses or delays.

> 💡 *Real World:* Distributed systems must handle network failures, so they choose **CP** (e.g., Banking apps prioritizing exact data balance) or **AP** (e.g., Social media feeds prioritizing uptime over instant updates).

### 2. Vertical vs. Horizontal Scaling
* **Vertical Scaling (Scale Up)**: Adding more RAM/CPU to an existing machine. (Easy, but has a hardware ceiling).
* **Horizontal Scaling (Scale Out)**: Adding more machines to a pool. (Infinite scale, requires Load Balancer & stateless architecture).

---

## 📝 How to Approach a System Design Problem

Follow this 5-step framework in interviews or software architecture planning:

1. **Clarify Requirements (Functional & Non-Functional)**
   - *Functional*: What features must the app have? (e.g., "Users can upload video and watch videos").
   - *Non-Functional*: Scalability (10M daily active users), Low Latency (<200ms), High Availability (99.99%).
2. **Back-of-the-Envelope Estimation**
   - Estimate Storage, Read/Write QPS (Queries Per Second), and Bandwidth.
3. **Define API Contracts & Data Schemas**
   - Write key REST endpoints and DB tables.
4. **Draw High-Level Architecture (HLD)**
   - Sketch Clients -> Load Balancer -> Servers -> Caching -> DB -> Queues.
5. **Deep Dive & Resolve Bottlenecks**
   - Address single points of failure, scaling strategies, caching strategies, and database sharding.

---

## 📄 Summary
* **System Design** translates business requirements into scalable software systems.
* **HLD** plans the big infrastructure components (Load Balancers, DBs, Caches, Microservices).
* **LLD** details the code implementation (Classes, DB tables, API schemas, Design patterns).
* **Requirements**: Functional Requirements define *WHAT* the system does; Non-Functional Requirements define *HOW WELL* it performs.
* Master these concepts to build resilient, enterprise-grade applications!
