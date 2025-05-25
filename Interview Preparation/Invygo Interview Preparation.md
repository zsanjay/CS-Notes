
The main difference between Layer 4 and Layer 7 load balancers lies in which layer of the OSI (Open Systems Interconnection) model they operate.

### Layer 4 Load Balancer (Transport Layer)

- **Works at the Transport Layer (TCP/UDP)**: A Layer 4 load balancer operates at the transport layer, meaning it deals with IP addresses, TCP, and UDP ports.
- **Routing Mechanism**: It uses basic information like IP addresses, port numbers, and protocol (TCP or UDP) to route traffic.
- **Speed and Efficiency**: Layer 4 load balancers generally offer faster performance because they don't need to inspect the content of the packets.
- **Limited to Network Information**: It can't make decisions based on the application-level content (e.g., URLs, cookies, HTTP headers).

**Example Use Cases:**

- Simple load balancing for services that don't need to inspect content.
- Real-time applications such as voice and video, where speed and low latency are crucial.

### Layer 7 Load Balancer (Application Layer)

- **Works at the Application Layer (HTTP, HTTPS, etc.)**: A Layer 7 load balancer operates at the application layer, where it inspects the actual content of the request.
- **Routing Mechanism**: It can make routing decisions based on the request's content, such as URLs, HTTP headers, cookies, or even request body data.
- **Greater Flexibility**: Layer 7 load balancers allow more sophisticated configurations, such as routing traffic based on the type of content or user session data.
- **Performance Overhead**: Since it inspects the content of each request, it can introduce more latency compared to Layer 4.

**Example Use Cases:**

- Web applications where decisions need to be made based on the content of the HTTP request (e.g., user authentication, session routing).
- Load balancing for websites where different URLs or paths should be routed to different servers.

### Key Differences:

| Feature                 | Layer 4 Load Balancer                           | Layer 7 Load Balancer                              |
| ----------------------- | ----------------------------------------------- | -------------------------------------------------- |
| **OSI Layer**           | Transport Layer (Layer 4)                       | Application Layer (Layer 7)                        |
| **Traffic Information** | IP addresses, ports, protocols                  | HTTP headers, URLs, cookies, etc.                  |
| **Routing Decisions**   | Based on network data (IP, TCP/UDP)             | Based on application content (URLs, cookies, etc.) |
| **Speed**               | Faster, lower latency                           | Slower due to content inspection                   |
| **Complexity**          | Simpler, less flexible                          | More complex, highly customizable                  |
| **Use Case**            | Simple network services, real-time applications | Web apps, services needing content-based routing   |

In summary, Layer 4 is typically faster and better suited for simpler use cases, while Layer 7 provides more granular control and flexibility at the cost of added complexity and potential performance overhead.

Note that sometimes you'll need to have specific features from your load balancer, like sticky sessions or persistent connections. The most common decision to make is whether to use an L4 (layer 4) or L7 (layer 7) load balancer.

You can somewhat shortcut this decision with a simple rule of thumb: if you have persistent connections like websockets, you'll likely want to use an L4 load balancer. Otherwise, an L7 load balancer offers great flexibility in routing traffic to different services while minimizing the connection load downstream.

##### What are the most common load balancers?

The most common load balancers are [AWS Elastic Load Balancer](https://aws.amazon.com/elasticloadbalancing/) (a hosted offering from AWS), [NGINX](https://www.nginx.com/) (an open-source webserver frequently used as a load balancer), and [HAProxy](https://www.haproxy.org/) (a popular open-source load balancer). Note that for problems with extremely high traffic, specialized hardware load balancers will outperform software load balancers you'd host yourself - you'll quickly be pulled into the crazy world of network engineering.

##### Things you should know about queues for your interview

1. **Message Ordering**: Most queues are FIFO (first in, first out), meaning that messages are processed in the order they were received. However, some queues (like [Kafka](https://www.hellointerview.com/learn/system-design/deep-dives/kafka)) allow for more complex ordering guarantees, such as ordering based on a specified priority or time.
    
2. **Retry Mechanisms**: Many queues have built-in retry mechanisms that attempt to redeliver a message a certain number of times before considering it a failure. You can configure retries, including the delay between attempts, and the maximum number of attempts.
    
3. **Dead Letter Queues**: Dead letter queues are used to store messages that cannot be processed. They're useful for debugging and auditing, as it allows you to inspect messages that failed to be processed and understand why they failed.
    
4. **Scaling with Partitions**: Queues can be partitioned across multiple servers so that they can scale to handle more messages. Each partition can be processed by a different set of workers. Just like databases, you will need to specify a partition key to ensure that related messages are stored in the same partition.
    
5. **Backpressure**: The biggest problem with queues is they make it easy to overwhelm your system. If my system supports 200 requests per second but I'm receiving 300 requests per second, I'll never finish them! A queue is just obscuring the problem that I don't have enough capacity. The answer is backpressure. Backpressure is a way of slowing down the production of messages when the queue is overwhelmed. This helps prevent the queue from becoming a bottleneck in your system. For example, if a queue is full, you might want to reject new messages or slow down the rate at which new messages are accepted, potentially returning an error to the user or producer.

##### What are the most common queueing technologies?

The most common queueing technologies are [Kafka](https://kafka.apache.org/) and [SQS](https://aws.amazon.com/sqs/). Kafka is a distributed streaming platform that can be used as a queue ([we have a deep-dive which goes into significant detail about how to use it](https://www.hellointerview.com/learn/system-design/deep-dives/kafka)), while SQS is a fully managed queue services provided by AWS.

#### Event sourcing

Event sourcing is a technique where changes in application state are stored as a sequence of events. These events can be replayed to reconstruct the application's state at any point in time, making it an effective strategy for systems that require a detailed audit trail or the ability to reverse or replay transactions.

##### Things you should know about streams for your interview

6. **Scaling with Partitioning**: In order to scale streams, they can be partitioned across multiple servers. Each partition can be processed by a different consumer, allowing for horizontal scaling. Just like databases, you will need to specify a partition key to ensure that related events are stored in the same partition.

7. **Multiple Consumer Groups**: Streams can support multiple consumer groups, allowing different consumers to read from the same stream independently. This is useful for scenarios where you need to process the same data in different ways. For example, in a real-time analytics system, one consumer group might process events to update a dashboard, while another group processes the same events to store them in a database for historical analysis.

8. **Replication**: In order to support fault tolerance, just like databases, streams can replicate data across multiple servers. This ensures that if a server fails, the data can still be read from another server.

9. **Windowing**: Streams can support windowing, which is a way of grouping events together based on time or count. This is useful for scenarios where you need to process events in batches, such as calculating hourly or daily aggregates of data. Think about a real-time dashboard that shows mean delivery time per region over the last 24 hours.

##### What are the most common streaming technologies?

The most common stream technologies are [Kafka](https://kafka.apache.org/), [Flink](https://flink.apache.org/), and [Kinesis](https://aws.amazon.com/kinesis/). Our [deep-dive on Kafka](https://www.hellointerview.com/learn/system-design/deep-dives/kafka) goes into significant detail about how it can be used in system design questions.

##### Things you should know about distributed locks for your interview

10. **Locking Mechanisms**: There are different ways to implement distributed locks. One common implementation uses Redis and is called Redlock. Redlock uses multiple Redis instances to ensure that a lock is acquired and released in a safe and consistent manner.

11. **Lock Expiry**: Distributed locks can be set to expire after a certain amount of time. This is important for ensuring that locks don't get stuck in a locked state if a process crashes or is killed.

12. **Locking Granularity**: Distributed locks can be used to lock a single resource or a group of resources. For example, you might want to lock a single ticket in a ticketing system or you might want to lock a group of tickets in a section of a stadium.

13. **Deadlocks**: Deadlocks can occur when two or more processes are waiting for each other to release a lock. Think about a situation where two processes are trying to acquire two locks at the same time. One process acquires lock A and then tries to acquire lock B, while the other process acquires lock B and then tries to acquire lock A. This can lead to a situation where both processes are waiting for each other to release a lock, causing a deadlock. You should be prepared to discuss how to prevent this - a common mistake is to have locks pulled from far-flung pieces of infrastructure or your code, this makes it hard to recognize and prevent deadlocks.

#### How to prevent deadlock?

To prevent **deadlocks** in the case of two processes, you need to ensure that the conditions for a deadlock do not occur. In a **deadlock**, the processes are stuck in a situation where each is waiting for the other to release a resource, resulting in a cyclic dependency. The four necessary conditions for a deadlock to occur are:

14. **Mutual Exclusion**: At least one resource must be held in a non-shareable mode (i.e., it can't be simultaneously used by more than one process).
15. **Hold and Wait**: A process must be holding at least one resource and waiting to acquire additional resources held by other processes.
16. **No Preemption**: Resources cannot be preempted (taken away) from processes holding them.
17. **Circular Wait**: A set of processes must exist where each process is waiting for a resource that the next process in the cycle holds.

To prevent deadlocks between two processes, here are several strategies you can use:

### 1. **Avoid Circular Wait**

- **Enforce an ordering of resource acquisition**: One simple way to prevent circular wait is to define a global ordering of resources and require that processes always request resources in that order. This ensures that processes can't form a cycle.
    - Example: If Process 1 requests Resource A and then Resource B, Process 2 must request Resource A before Resource B, or neither can request both resources at the same time.

### 2. **Use Timeout (or Resource Request Limiting)**

- Set a **timeout** for waiting for resources. If a process cannot acquire all the resources it needs within a certain time frame, it releases any resources it has already acquired and retries. This prevents indefinite blocking.
    - Example: If Process A requests both Resource 1 and Resource 2, but Resource 2 is unavailable, Process A will release Resource 1 after a timeout and retry.

### 3. **Prevent Hold and Wait**

- **Require processes to request all the resources they need at once**. By doing so, the process either gets all the resources it needs, or none at all.
    - Example: Before starting, Process A must request Resource 1 and Resource 2 simultaneously. If it can't get both, it has to wait and retry later.

### 4. **Allow Preemption**

- **Allow resources to be preempted** from a process that holds them if needed by another process. This could involve rolling back processes, saving their state, and letting the resources be taken away.
    - Example: If Process A holds Resource 1 and Process B needs it, the system can preempt the resource from Process A, roll back its state, and give it to Process B.

### 5. **Use a Resource Allocation Graph (RAG)**

- **Track resource allocation and requests** using a resource allocation graph, which helps identify potential deadlocks by detecting cycles in the graph.
    - If no cycle exists, the system can grant the request safely. If a cycle is detected, the request is denied.

### 6. **Deadlock Detection and Recovery**

- While not exactly a prevention method, some systems allow deadlocks to occur and then detect them using techniques like **resource allocation graphs** or other methods.
- Once detected, processes involved in the deadlock are killed or rolled back, and resources are released to break the cycle.
    - **Detection**: The system periodically checks for deadlock situations and resolves them by aborting one of the processes or taking other corrective actions.

### Summary of Strategies:

- **Enforce a resource ordering** to avoid circular wait.
- **Request all resources upfront** to avoid hold and wait.
- **Timeouts** prevent indefinite blocking.
- **Preemption** allows resources to be taken from a process.
- **Deadlock detection** and recovery can identify and resolve deadlocks after they happen.

In the case of two processes, if you ensure that one of these strategies is in place, you can effectively prevent a deadlock situation from occurring. A good approach is often a combination of resource ordering and timeout to ensure that the processes don't get stuck indefinitely.


##### Things you should know about CDNs

18. **CDNs are not just for static assets**. While CDNs are often used to cache static assets like images, videos, and javascript files, they can also be used to cache dynamic content. This is especially useful for content that is accessed frequently, but changes infrequently. For example, a blog post that is updated once a day can be cached by a CDN.

19. **CDNs can be used to cache API responses**. If you have an API that is accessed frequently, you can use a CDN to cache the responses. This can help reduce the load on your servers and improve the performance of your API.

20. **Eviction policies**. Like other caches, CDNs have eviction policies that determine when cached content is removed. For example, you can set a time-to-live (TTL) for cached content, or you can use a cache invalidation mechanism to remove content from the cache when it changes.


##### Examples of CDNs

Some of the most popular CDNs are [Cloudflare](https://www.cloudflare.com/), [Akamai](https://www.akamai.com/), and [Amazon CloudFront](https://aws.amazon.com/cloudfront/). These CDNs offer a range of features, including caching, DDoS protection, and web application firewalls. They also have a global network of edge locations, which means that they can deliver content to users around the world with low latency.


Non Functional Requirements

- The system should be highly availability, prioritizing availability over consistency
- The system should be able to scale to support 100M+ DAUs
- The system should be low latency, rendering feeds in under 200ms

It's important that non-functional requirements are put in the context of the system and, where possible, are quantified. For example, "the system should be low latency" is obvious and not very meaningful—nearly all systems should be low latency. "The system should have low latency search, < 500ms," is much more useful as it identifies the part of the system that most needs to be low latency and provides a target.


Coming up with non-functional requirements can be challenging, especially if you're not familiar with the domain. Here is a checklist of things to consider that might help you identify the most important non-functional requirements for your system. You'll want to identify the top 3-5 that are most relevant to your system.

21. **CAP Theorem**: Should your system prioritize consistency or availability? Note, partition tolerance is a given in distributed systems.
    
22. **Environment Constraints**: Are there any constraints on the environment in which your system will run? For example, are you running on a mobile device with limited battery life? Running on devices with limited memory or limited bandwidth (e.g. streaming video on 3G)?
    
23. **Scalability**: All systems need to scale, but does this system have unique scaling requirements? For example, does it have bursty traffic at a specific time of day? Are there events, like holidays, that will cause a significant increase in traffic? Also consider the read vs write ratio here. Does your system need to scale reads or writes more?
    
24. **Latency**: How quickly does the system need to respond to user requests? Specifically consider any requests that require meaningful computation. For example, low latency search when designing Yelp.
    
25. **Durability**: How important is it that the data in your system is not lost? For example, a social network might be able to tolerate some data loss, but a banking system cannot.
    
26. **Security**: How secure does the system need to be? Consider data protection, access control, and compliance with regulations.
    
27. **Fault Tolerance**: How well does the system need to handle failures? Consider redundancy, failover, and recovery mechanisms.
    
28. **Compliance**: Are there legal or regulatory requirements the system needs to meet? Consider industry standards, data protection laws, and other regulations.

#### Important Note:

Notice how there is no userId in the POST /v1/tweet endpoint? This is because we will get the id of the user initiating the request from the authentication token in the request header. Putting sensitive information like user ids in the request body is a security risk and a mistake that many candidates make. Don't be one of them!

### Must to refer

https://www.youtube.com/watch?v=bBTPZ9NdSk8