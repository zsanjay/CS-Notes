
## Problems That Require Real-Time Updates — System Design Deep Dive

---

### 1. 🧑‍🤝‍🧑 **Chat/Messaging Apps** (e.g., WhatsApp, Slack)

#### 🔹 Requirements:

- Instant delivery of messages
    
- Read receipts, typing indicators
    
- Group chat synchronization
    
- Offline support
    

#### 🧠 Key Design Points:

- **Bidirectional communication**: Use WebSockets
    
- **Message broker**: Kafka, RabbitMQ for async processing
    
- **Storage**: NoSQL (e.g., MongoDB) or a hybrid (for fast writes)
    
- **User presence service** (online/offline tracking)
    

#### ⚙️ Tech Stack:

- WebSocket or Socket.IO
    
- Redis Pub/Sub (broadcast within cluster)
    
- Kafka (for durability and event sourcing)
    
- PostgreSQL or MongoDB
    

---

### 2. 📈 **Stock Market Dashboards / Crypto Platforms**

#### 🔹 Requirements:

- Sub-millisecond latency on price updates
    
- Real-time order book
    
- Huge volume of concurrent users
    

#### 🧠 Key Design Points:

- **Event-driven architecture** with minimal latency
    
- **Message queuing** to distribute updates
    
- **Efficient client push** using WebSocket
    
- **Throttling/batching** to reduce overhead
    

#### ⚙️ Tech Stack:

- Kafka or NATS for streaming updates
    
- Redis or Memcached for caching
    
- WebSocket (scale using sticky sessions or reverse proxies)
    
- TimescaleDB or InfluxDB for storing time series
    

---

### 3. 📍 **Real-Time Location Tracking (e.g., Uber, food delivery)**

#### 🔹 Requirements:

- Track location of users/drivers/riders
    
- Low latency map updates
    
- ETA recalculations
    

#### 🧠 Key Design Points:

- **Frequent location updates** (every few seconds)
    
- **Geo-indexed data** (e.g., using H3, S2, or geohash)
    
- **Region-based broadcasting** (only notify nearby users)
    
- **Push location data to both server and clients**
    

#### ⚙️ Tech Stack:

- WebSockets or MQTT for low-overhead messaging
    
- Redis with Geo-indexing or ElasticSearch
    
- Kafka for location streams
    
- PostGIS / MongoDB with geospatial queries
    

---

### 4. 📝 **Collaborative Document Editing (e.g., Google Docs, Figma)**

#### 🔹 Requirements:

- Multi-user editing without conflict
    
- Instant visibility of changes
    
- Versioning and rollback
    

#### 🧠 Key Design Points:

- **Operational Transformation (OT)** or **Conflict-Free Replicated Data Types (CRDTs)**
    
- **Real-time broadcast** of cursor and text changes
    
- **Concurrency handling**
    
- **Document locking / permission system**
    

#### ⚙️ Tech Stack:

- WebSockets + OT/CRDT libraries
    
- Redis for user-session tracking
    
- Event sourcing for document changes
    
- PostgreSQL (append-only change logs)
    

---

### 5. 🛒 **Flash Sales / Live Bidding**

#### 🔹 Requirements:

- Real-time inventory updates
    
- High concurrency during spikes
    
- Prevent overselling or double bidding
    

#### 🧠 Key Design Points:

- **Distributed locking** or optimistic concurrency control
    
- **Rate limiting**
    
- **Pessimistic inventory deduction**
    
- **Low-latency event delivery**
    

#### ⚙️ Tech Stack:

- Redis for counters and atomic operations
    
- Kafka for bidding events
    
- WebSockets for pushing updates
    
- PostgreSQL with row-level locks
    

---

### 6. 📉 **Real-Time Monitoring / Metrics Dashboards**

#### 🔹 Requirements:

- Ingest and display system/app metrics in real-time
    
- Aggregation and filtering
    
- Alerting on thresholds
    

#### 🧠 Key Design Points:

- **Time-series database** optimized for writes
    
- **Data aggregation pipelines**
    
- **Event batching**
    
- **Threshold monitoring**
    

#### ⚙️ Tech Stack:

- Prometheus or InfluxDB for TS data
    
- Grafana for visualization
    
- Kafka or Fluentd for log/metric shipping
    
- Redis for alert states
    

---

## 🔧 Core Design Challenges in Real-Time Systems

|Challenge|How to Handle|
|---|---|
|**Latency**|Use persistent connections (WebSockets), in-memory cache (Redis)|
|**Scalability**|Stateless WebSocket handlers + sticky sessions or consistent hashing|
|**Backpressure**|Throttle client updates, batch messages, use reactive streams|
|**Data consistency**|Use CRDTs, distributed locks, or eventual consistency|
|**Availability**|Load balancers, failover DBs, message queue replication|
|**Security**|WebSocket token auth, rate limiting, message integrity checks|

---

## 🌐 Protocol Choices

|Protocol|Use When|
|---|---|
|**WebSocket**|Bi-directional real-time communication|
|**Server-Sent Events (SSE)**|One-way server-to-client updates|
|**MQTT**|Lightweight IoT / mobile device messaging|
|**GraphQL Subscriptions**|Real-time GraphQL data sync|
|**gRPC Streaming**|Efficient communication in microservices|

---

## ⚙️ Real-Time Architecture Pattern (Generalized)

```css
[Client App]
    ↓ WebSocket/API
[API Gateway / Load Balancer]
    ↓
[Real-Time Server (WS handler)]  ←→  [Redis Pub/Sub or Kafka]
    ↓                                 ↓
[Service Layer]                 [Storage + Analytics]
```

