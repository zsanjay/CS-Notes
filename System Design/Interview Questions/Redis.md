# 1. What is Redis and Why is it fast?

**Redis** stands for **RE**mote **DI**ctionary **S**erver.

It is:

- A **key-value store** (like a giant super-fast dictionary/hashmap)
- **In-memory** (stores data in RAM, not on disk — that's a big reason it's fast)
- **NoSQL database** (doesn’t store data in tables like SQL)
- Often used for **caching**, **session storage**, **real-time counters**, **queues**, **pub/sub messaging**, etc.

---

# ⚡ Why is Redis so fast?

1. **Everything is in RAM**
    
    - RAM access is ~100,000x faster than disk access.
    - No waiting for slow hard drives or SSDs.
        
2. **Simple Data Structures**
    
    - Redis is optimized for basic types like:
        
        - Strings
        - Lists
        - Hashes
        - Sets
        - Sorted Sets
        
    - It's designed to work super fast with these.
        
3. **Single-threaded, Event Loop Model**
    
    - Redis handles one command at a time (no thread locking), using a super optimized event loop.
    - No "context switching" between threads = less CPU overhead.

4. **Efficient Networking**
    
    - Built on top of very lightweight TCP networking.
    - Supports pipelining (sending multiple commands without waiting for each one to respond).
        
5. **Minimal Overhead**
    
    - No heavy SQL parsing.
    - No complicated joins or relations.
    - Just simple get/set/incr/decr operations.

# 2. What are differences between Redis and Memcached?

|Feature|**Redis**|**Memcached**|
|---|---|---|
|**Data Types**|Strings, Lists, Sets, Hashes, Sorted Sets, Streams, Bitmaps, etc.|Only Strings|
|**Persistence**|Yes (can save data to disk — RDB/AOF snapshots)|No (purely in-memory, no disk backup)|
|**Replication**|Yes (Primary-Replica model)|No native replication|
|**Cluster Support**|Yes (Redis Cluster)|Limited (Memcached clients handle sharding)|
|**Pub/Sub Messaging**|Yes (Built-in pub/sub feature)|No|
|**Memory Efficiency**|More overhead (because of data structures)|Very efficient for simple key-value pairs|
|**Atomic Operations**|Yes (e.g., INCR, DECR, etc.)|Basic increment/decrement only|
|**Backup & Recovery**|Can restore from disk after crash|Data is lost after restart|
|**Performance at Scale**|Very fast, slightly more complex|Extremely fast, very simple|

# 3. How would you use Redis to rate limit an API?

### What is API Rate Limiting?

- You want to **restrict** how many times a user/IP can hit your API within a time window.
- Example: **"Max 100 requests per 15 minutes per user."**

If a user exceeds the limit, you return a **429 Too Many Requests** error.

### How Redis Helps

Because Redis is **fast** and **atomic**, you can:

- Track the number of requests per user/IP.
- Expire the counter automatically after a certain time window.
- Handle massive concurrency safely without race conditions.

### Basic Strategy: Redis + INCR + EXPIRE

Here's the _basic_ flow:

1. Each time a user makes a request:
    - `INCR user:{userId}:requests`
2. If it's the **first request**, also:
    - `EXPIRE user:{userId}:requests 900` (15 minutes = 900 seconds)
3. If the counter exceeds the limit:
    - Reject the request with `429 Too Many Requests`.

```python
import redis

r = redis.Redis(host='localhost', port=6379, db=0)

def is_allowed(user_id):
    key = f"rate_limit:{user_id}"
    current = r.incr(key)
    
    if current == 1:
        r.expire(key, 900)  # 15 minutes

    if current > 100:
        return False
    return True
```

# 4. Explain the difference between Redis SET and HSET.

| Feature   | **SET**                                 | **HSET**                                                             |
| --------- | --------------------------------------- | -------------------------------------------------------------------- |
| Stores    | A **single string value** against a key | A **field-value pair inside a hash** (like a mini object/dictionary) |
| Use Case  | Store one simple value                  | Store multiple related values grouped together                       |
| Structure | `key → value`                           | `key → { field1: value1, field2: value2, ... }`                      |
| Example   | `SET user:123 "Sanjay"`                 | `HSET user:123 name "Sanjay" age "30"`                               |

# 5. What is a Sorted Set (`ZSET`) in Redis?

- Like a **Set** (unique elements)
- But **each element has a score** (a floating-point number).
- Redis automatically keeps them **sorted** by score!

You can:

- Add elements with a score
- Get elements ordered by score (ascending or descending)
- Get top N, bottom N, ranges, etc.

# When Would You Use a Sorted Set?

| Use Case                                                      | Why Sorted Set is Perfect                                |
| ------------------------------------------------------------- | -------------------------------------------------------- |
| **Leaderboards** (Games, Fitness apps)                        | Players ranked by score or points                        |
| **Priority Queues**                                           | Jobs/tasks ordered by priority or deadline               |
| **Rate-limiting windows**                                     | Store timestamps with scores (for sliding window limits) |
| **Real-time ranking** (top hashtags, trending topics)         | Always show top trending items                           |
| **Expiry queues** (like scheduled messages)                   | Fetch by earliest time first                             |
| **Scoring systems** (product ratings, recommendation systems) | Order items based on weighted score                      |
| **Location-based apps** (with a hack)                         | Sort based on distance or a computed proximity score     |

# 6. What is Redis Sentinel?

**Redis Sentinel** is a **high availability** (HA) system for Redis.

It **monitors**, **detects failures**, and **automatically recovers** your Redis setup without manual intervention.

In simple words:

> **Redis Sentinel makes sure your Redis doesn't go down, and if it does, it fixes things automatically.**


# What Exactly Does Sentinel Do?

| Job                        | Description                                                                 |
| -------------------------- | --------------------------------------------------------------------------- |
| **Monitoring**             | Constantly checks if your Redis servers are alive and healthy.              |
| **Failure Detection**      | Notices if the master (primary) node is unresponsive.                       |
| **Automatic Failover**     | If master fails, it promotes a replica (slave) to become the new master.    |
| **Notification**           | Alerts other parts of your system (or you) that a failover happened.        |
| **Configuration Provider** | Clients can ask Sentinel where the current master is (even after failover). |

# Why is Sentinel Needed?

- Redis by itself = **single point of failure** (SPOF).
    
- Without Sentinel:
    - If the master crashes → your whole app crashes.

- With Sentinel:
    - Redis becomes **self-healing**.
    - You get **auto-failover**, **minimal downtime**, and **resiliency**.


# Real-World Example:

You have:

- 1 Redis Master
- 2 Redis Replicas (slaves)
- 3 Sentinel nodes

**Scenario:**

- Master server crashes.
- Sentinels detect the crash.
- Sentinels elect a new master from replicas.
- Clients reconnect to the new master automatically.

```
+-----------+            +--------------------+
|  Sentinel |  <------>   |  Sentinel  (Quorum) |
+-----------+            +--------------------+
       |                          |
       v                          v
  +----------+            +---------------+
  | Master   |    <--->    | Replica(s)     |
  +----------+            +---------------+

```

- Sentinels talk to each other (gossip protocol).
- They monitor master and replicas.
- They coordinate to promote a replica if needed.

