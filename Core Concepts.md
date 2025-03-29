# **Spring Boot Core Concepts: Tomcat vs Netty, Reactive Programming, Sync/Async, Thread Pool, and Event Loop**

## ** 1. Synchronous vs Asynchronous**
- **Synchronous** → Tasks execute **one after another** (blocking).
- **Asynchronous** → Tasks **don’t wait** for completion; they execute **independently** (non-blocking).

---

## ** 2. Reactive Programming**  
- A **type of asynchronous programming** where components **react** when data is available.
- Used in **Spring WebFlux** (non-blocking, high-performance apps).
- Enables handling **millions of concurrent requests efficiently.**

---

## ** 3. Thread Pool Model (Tomcat) vs Event Loop Model (Netty)**  

### **🔹 Tomcat (Thread Pool Model)**
✅ Uses **a pool of threads**, where:  
   - **One request = One thread**  
   - If requests exceed available threads → they **wait (blocking)**.  
✅ Works well for **synchronous & traditional REST APIs**.  
✅ Used in **Spring MVC** (blocking architecture).  

### **🔹 Netty (Event Loop Model)**
✅ Uses **a few threads** where:  
   - **One thread handles multiple requests** asynchronously.  
   - Uses an **event loop** to process non-blocking requests.  
✅ Provides **high performance** and is great for real-time, scalable applications.  
✅ Used in **Spring WebFlux** (non-blocking architecture).  

---

## ** 4. Event Loop Model (Netty, Node.js)**
- A single thread **registers** multiple tasks and processes them asynchronously.
- **No thread blocking** → Thread remains free to handle other requests.
- Ideal for **handling thousands/millions of concurrent requests**.

---

## **🚀 Key Takeaways**  
| Feature          | Tomcat (Thread Pool) | Netty (Event Loop) |
|-----------------|-----------------|----------------|
| **Concurrency Model** | One request = One thread | One thread handles multiple requests |
| **Blocking/Non-blocking** | **Blocking** (Synchronous) | **Non-blocking** (Asynchronous) |
| **Performance** | Slower for high loads | Efficient for massive loads |
| **Use Case** | Traditional REST APIs (Spring MVC) | Real-time, high-performance apps (Spring WebFlux) |
| **Spring Boot Starter** | spring-boot-starter-web | spring-boot-starter-webflux |

---

# **Spring Boot Logging**  

Logging is essential for monitoring and debugging Spring Boot applications. By default, Spring Boot uses **Logback** for logging.

### 🔹 **Logging Levels Hierarchy (Lowest to Highest):**  
1. `TRACE` → Most detailed logs (not commonly used).  
2. `DEBUG` → Used for debugging during development.  
3. `INFO` → General application flow (default level).  
4. `WARN` → Indicates potential problems.  
5. `ERROR` → Logs errors that need attention.  
6. `FATAL` → Critical failures that may cause the application to crash.  

### 🔹 **How Logging Works?**  
- By **default**, Spring Boot logs at `INFO` level.
- Any log statement **below** the configured level **won’t be printed**.
- Any log statement **equal to or above** the configured level **will be printed**.

### 🔹 **Configuring Logging in `application.properties`**  
```properties
# Sets global logging level to INFO (default)
logging.level.root=INFO  
```
**Example:**
```java
log.debug("This is a debug log");  // Will NOT be printed if root level is INFO.
log.info("This is an info log");   // Will be printed if root level is INFO.
log.error("This is an error log"); // Will be printed if root level is INFO.
```

### 🔹 **Changing Log Level for Specific Packages:**  
```properties
logging.level.com.example=DEBUG  # Enables DEBUG logs for this package only.
logging.level.org.springframework=WARN  # Suppresses INFO and DEBUG logs for Spring Framework.
```

### 🔹 **If No Logs are Defined in the Service?**  
- If you **don’t add any log statements** in your code, the logs will be **empty** regardless of the log level.
- The application will still print **Spring Boot startup logs**.

### 🔹 **If No Log Level is Defined in `application.properties`?**  
- Spring Boot **defaults to INFO level**, meaning all `INFO`, `WARN`, `ERROR`, and `FATAL` logs will be printed.
- `DEBUG` and `TRACE` logs will **not** be printed unless explicitly enabled.

### **🔹 Summary Rule:**  
✅ If log level is set to **INFO**, only `INFO`, `WARN`, `ERROR`, and `FATAL` logs will be printed.  
❌ `DEBUG` and `TRACE` logs will be **ignored** unless enabled in `application.properties`.  

---
