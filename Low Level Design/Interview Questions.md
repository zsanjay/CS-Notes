
### Low-Level Design Questions

#### 1. **Design a Parking Lot System**

- Features: Multiple levels, different vehicle types, entry/exit, payment system.
    
- Concepts: OOP, inheritance (Vehicle), interfaces (Payment), design patterns (Strategy for payment), concurrency (for ticketing).
    

#### 2. **Design an Elevator System**

- Features: Multiple elevators, request handling, load balancing.
    
- Concepts: Observer pattern (request listeners), FSM (finite state machine), Scheduler (Round Robin, Nearest Car Algorithm).
    

#### 3. **Design a BookMyShow-like Movie Ticket Booking System**

- Features: Search movies, book seats, show availability, cancellation.
    
- Concepts: Real-time seat locking, concurrency control, design patterns (Singleton for inventory manager), scalability.
    

#### 4. **Design a Ride-Sharing System like Uber**

- Features: Matching drivers with riders, tracking location, fare calculation.
    
- Concepts: Strategy pattern (fare calculation), GeoHashing, pub-sub (notifications), real-time tracking.
    

#### 5. **Design a Notification System**

- Features: Send email, SMS, push notifications; retry mechanism; user preferences.
    
- Concepts: Strategy pattern (channel selection), Queues (for retry), Observer pattern (subscribers).
    

#### 6. **Design a Rate Limiter**

- Features: Limit API calls per user/IP/time window.
    
- Concepts: Sliding window, Token bucket, Leaky bucket algorithms, concurrency handling.
    

#### 7. **Design a File Storage System like Dropbox**

- Features: Upload, download, versioning, sync across devices.
    
- Concepts: Chunking, file versioning, metadata management, synchronization.
    

#### 8. **Design a Chat System (like WhatsApp)**

- Features: One-to-one chat, group chat, online/offline, read receipts.
    
- Concepts: Pub-Sub, message queueing, event sourcing, database schema design.
    

#### 9. **Design a Cache System (like LRU)**

- Features: Get/Put operations, eviction policy.
    
- Concepts: LRU using HashMap + Doubly Linked List, concurrency (thread-safe access), TTL.
    

#### 10. **Design a Logger Utility**

- Features: Multiple log levels (INFO, ERROR), log rotation, concurrent writes.
    
- Concepts: Singleton, Thread safety, Writer buffering.
    

---

### 👔 What Interviewers Look For

- **Clarity in Requirements Gathering**: Ask the right questions.
    
- **Modular Class Design**: Follow SOLID principles.
    
- **Use of Design Patterns**: Appropriately applied.
    
- **Scalability and Extensibility**
    
- **Thread Safety & Concurrency Handling**
    
- **Code Structure and Naming Conventions**
    

---

### ✅ Pro Tips

- Always start with a **requirements clarification**.
    
- Draw a **class diagram** or describe it clearly.
    
- Talk through your **trade-offs** and **assumptions**.
    
- Think about **extensibility** – e.g., if new payment types are added.