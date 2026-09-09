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
6. [Functional vs Non-Functional Requirements](#-functional-vs-non-functional-requirements)
7. [Multiple Real-World Examples](#-multiple-real-world-examples)
   - [Example 1: Real-Time Messaging App (WhatsApp)](#example-1-real-time-chat-system-whatsapp---hld--lld)
   - [Example 2: Video Streaming Platform (YouTube/Netflix)](#example-2-video-streaming-platform-youtubenetflix---hld)
   - [Example 3: Ride-Sharing Platform (Uber)](#example-3-ride-sharing-service-uber---hld)
   - [Example 4: Object-Oriented Parking Lot System](#example-4-parking-lot-system---lld)
8. [Key System Design Trade-offs & Concepts](#-key-system-design-trade-offs--concepts)
9. [How to Approach a System Design Interview / Problem](#-how-to-approach-a-system-design-problem)

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
