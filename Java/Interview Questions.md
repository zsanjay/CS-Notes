
https://github.com/learning-zone/java-basics/blob/master/collections-questions.md

### Senior Java Developer Position

For a **Senior Java Developer** position, interview questions typically focus on advanced Java concepts, problem-solving, design patterns, and your ability to optimize solutions. You'll also be expected to demonstrate leadership skills, understanding of large systems, and solid experience with frameworks, multi-threading, database interactions, and performance tuning.

Here are some more advanced Java topics, along with example questions and solutions, that can help you prepare for a Senior Java Developer interview:

### 1. **Multi-threading and Concurrency**

#### **Example Question**:

Write a program to print the numbers from 1 to 10 using two threads, where one thread prints odd numbers and the other prints even numbers.

```jsx
class EvenThread extends Thread {
    private Object lock;

    public EvenThread(Object lock) {
        this.lock = lock;
    }

    public void run() {
        synchronized (lock) {
            for (int i = 2; i <= 10; i += 2) {
                System.out.println(i);
                lock.notify();
                try {
                    if (i != 10) {
                        lock.wait();
                    }
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            }
        }
    }
}

class OddThread extends Thread {
    private Object lock;

    public OddThread(Object lock) {
        this.lock = lock;
    }

    public void run() {
        synchronized (lock) {
            for (int i = 1; i <= 9; i += 2) {
                System.out.println(i);
                lock.notify();
                try {
                    if (i != 9) {
                        lock.wait();
                    }
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            }
        }
    }
}

public class OddEvenPrint {
    public static void main(String[] args) {
        Object lock = new Object();
        Thread oddThread = new OddThread(lock);
        Thread evenThread = new EvenThread(lock);
        
        oddThread.start();
        evenThread.start();
    }
}
```

### 2. **Design Patterns**

#### **Example Question**:

Implement a Singleton pattern in Java, ensuring thread safety.

```java
public class Singleton {

    // Eager Initialization (Thread-safe)
    private static final Singleton instance = new Singleton();

    // Private constructor to prevent instantiation
    private Singleton() {}

    public static Singleton getInstance() {
        return instance;
    }
}
```

Alternatively, for lazy initialization with thread safety, you can use the Double-Checked Locking pattern:

```java
public class Singleton {
    
    private static volatile Singleton instance;
    
    private Singleton() {}
    
    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```
### 3. **JVM Internals**

#### **Example Question**:

Explain the JVM Garbage Collection process. What types of Garbage Collectors are available in Java?

**Answer**:

- **GC Types**:
    - **Serial GC**: Single-threaded garbage collection, best suited for applications with low memory and minimal pause times.
    - **Parallel GC**: Multi-threaded garbage collection for improving performance in multi-core systems.
    - **CMS (Concurrent Mark and Sweep) GC**: Designed for low-latency applications, this collector minimizes the pause times during GC cycles.
    - **G1 (Garbage First) GC**: Suitable for large heap applications, it tries to balance between GC pause times and throughput.

**Key Phases of GC**:

1. **Mark**: Identify which objects are still in use.
2. **Sweep**: Remove unreferenced objects.
3. **Compact**: Optionally compact memory to reduce fragmentation.

### 4. **Java Streams API**

#### **Example Question**:

Using Java Streams, filter all even numbers from a list and double them.

```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class StreamExample {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
        
        List<Integer> result = numbers.stream()
                                      .filter(n -> n % 2 == 0)
                                      .map(n -> n * 2)
                                      .collect(Collectors.toList());
        
        System.out.println(result); // Output: [4, 8, 12, 16, 20]
    }
}
```

### 5. **Database and JDBC**

#### **Example Question**:

Write a program to connect to a database and retrieve data using JDBC.

```java
import java.sql.*;

public class JdbcExample {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/mydatabase";
        String user = "root";
        String password = "password";
        
        try (Connection connection = DriverManager.getConnection(url, user, password)) {
            String query = "SELECT * FROM employees";
            Statement stmt = connection.createStatement();
            ResultSet rs = stmt.executeQuery(query);
            
            while (rs.next()) {
                int id = rs.getInt("id");
                String name = rs.getString("name");
                System.out.println("ID: " + id + ", Name: " + name);
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

### 6. **Spring Framework and Dependency Injection**

#### **Example Question**:

What is Dependency Injection (DI) in Spring? Explain with an example.

**Answer**:

- **Dependency Injection** is a design pattern that allows you to inject dependencies (objects) into a class rather than creating them inside the class.
- In Spring, DI is implemented through **constructor-based** or **setter-based** injection.

Example using **constructor injection**:

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component
public class Car {

    private Engine engine;

    @Autowired
    public Car(Engine engine) {
        this.engine = engine;
    }

    public void start() {
        engine.run();
    }
}

@Component
class Engine {

    public void run() {
        System.out.println("Engine is running...");
    }
}
```

### 7. **Microservices and RESTful Web Services**

#### **Example Question**:

How would you design a simple RESTful API using Spring Boot?

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}

@RestController
class GreetingController {

    @GetMapping("/greet")
    public String greet(@RequestParam(value = "name", defaultValue = "World") String name) {
        return "Hello, " + name;
    }
}
```

With this, a request to `http://localhost:8080/greet?name=John` will return `"Hello, John"`.

### 8. **Data Structures and Algorithms**

#### **Example Question**:

Write a program to find the **Kth largest element** in an unsorted array.

```java
import java.util.PriorityQueue;

public class KthLargestElement {
    public static int findKthLargest(int[] nums, int k) {
        PriorityQueue<Integer> minHeap = new PriorityQueue<>(k);
        
        for (int num : nums) {
            if (minHeap.size() < k) {
                minHeap.offer(num);
            } else if (num > minHeap.peek()) {
                minHeap.poll();
                minHeap.offer(num);
            }
        }
        return minHeap.peek();
    }

    public static void main(String[] args) {
        int[] nums = {3, 2, 1, 5, 6, 4};
        int k = 2;
        System.out.println("The " + k + "th largest element is " + findKthLargest(nums, k));
    }
}
```

### 9. **System Design**

#### **Example Question**:

Design a URL shortening service like **bit.ly**. How would you approach the problem?

- **Components**:
    - **Frontend**: A simple UI where users input URLs.
    - **Backend**: A REST API to handle URL shortening and retrieval.
    - **Database**: Store the original URL and its shortened code.
    - **Hashing Function**: Generate unique short codes for URLs.

**Possible approach**:

4. Take the original URL and hash it (using something like MD5, SHA, or Base62 encoding) to create a short URL.
5. Store the mapping between the short URL and the original URL in a database.
6. For retrieval, take the short URL, hash it back to the corresponding original URL.

---

### Tips for Senior Java Developer Interviews:

- **Design patterns**: You should be familiar with common patterns like Singleton, Factory, Strategy, Observer, and others.
- **JVM internals**: Understanding how the JVM works, memory management, garbage collection, and performance tuning is key.
- **Scalability**: Be prepared to design scalable systems, focusing on load balancing, distributed caching, and fault tolerance.
- **Frameworks**: Experience with **Spring Boot**, **Hibernate**, **JPA**, and **Microservices** will be a big advantage.
- **Code optimization**: Be able to optimize solutions for time complexity (Big-O) and space complexity.
- **Leadership**: Show how you can guide junior developers, conduct code reviews, and manage large-scale projects.