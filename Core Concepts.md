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

## ** Inversion of Control (IoC)**  
Inversion of Control (IoC) is a design principle where the control of object creation and dependency management is given to a framework (like Spring) instead of being handled manually by the developer.  

### **📌 Example:**  
**Without IoC:**  
```java
Car car = new Car();
```

**With IoC (using Spring):**  
```java
@Autowired
Car car;  // Spring automatically provides an instance
```

## **Dependency Injection (DI)**  
Dependency Injection is a way of implementing IoC where dependencies (objects needed by a class) are provided from outside rather than being created inside the class itself.  

### **📌 Example: Without DI**  
```java
class Car {
    Engine engine = new Engine();  // Tight coupling
}
```

### **📌 With DI:**  
```java
class Car {
    private Engine engine;

    // Injecting dependency via constructor
    public Car(Engine engine) { 
        this.engine = engine;
    }
}
```
Now, `Car` doesn’t create an `Engine`, it just receives it from the outside.  

### **🔹 Types of Dependency Injection:**  
#### **1️. Constructor Injection (Recommended)**  
- Injects dependencies via constructor  
- Ensures immutability  
- Example:  
```java
@Component
class Car {
    private Engine engine;
    public Car(Engine engine) { this.engine = engine; }
}
```

#### **2️. Setter Injection**  
- Injects dependencies via setter methods  
- Example:  
```java
@Component
class Car {
    private Engine engine;
    @Autowired
    public void setEngine(Engine engine) { 
		this.engine = engine; 
	}
}
```

#### **3️. Field Injection (Less recommended)**  
- Injects dependencies directly into fields  
- Example:  
```java
@Component
class Car {
    @Autowired
    private Engine engine;
}
```

## **Autowiring**  
Autowiring is a feature in Spring that automatically injects dependencies without explicitly creating objects. It helps in reducing boilerplate code. In short, Autowiring is a way to achieve Dependency Injection in Spring.

### **🔹 Types of Autowiring in Spring:**  
- **`@Autowired` (Default)** → Automatically injects dependencies  
- **`@Qualifier("beanName")`** → Used when multiple beans exist  
- **`@Primary`** → Gives higher preference to a specific bean  
- **`@Lazy`** → Initializes bean only when needed  

### **📌 Example:**  
```java
@Component
class Engine { }

@Component
class Car {
    @Autowired
    private Engine engine;  // Spring automatically injects Engine instance
}
```

---

## **Bean Scopes in Spring**  
In Spring, a **bean scope** defines the lifecycle and visibility of a bean within the Spring container. There are **five** types of bean scopes:

---

### **1️ Singleton (Default)**
- **Only one instance** of the bean is created and shared across the entire application.
- Even if you call `getBean()` multiple times, it returns the same instance.

📌 **Example:**
```java
@Component
public class MyBean {
}
```
Since `@Component` registers the bean with default **singleton scope**, every time it's injected, the same instance is used.

📌 **Usage:**  
- When you need a **shared instance** for efficiency.  
- Used for **stateless beans** like services.

---

### **2️ Prototype**
- A **new instance is created** every time the bean is requested.
- Different from Singleton, as each `getBean()` call returns a **new object**.
- Prototype beans are lazy by default. So we dont' need to mention @Lazy annotation.

📌 **Example:**
```java
@Component
@Scope("prototype")
public class MyBean {
}
```
📌 **Usage:**  
- When you need a **separate instance** every time (e.g., per request).  
- Useful for **stateful beans**.

---

### **3️ Request (For Web Applications)**
- A **new bean instance** is created for **each HTTP request**.
- Available only in **Spring Web Applications**.

📌 **Example:**
```java
@Component
@Scope("request")
public class MyRequestBean {
}
```
📌 **Usage:**  
- Useful when you want **request-specific data**, like a user's session.

---

### **4️ Session (For Web Applications)**
- A **single bean instance** is created for a **user session**.
- The bean exists as long as the HTTP session is active.

📌 **Example:**
```java
@Component
@Scope("session")
public class MySessionBean {
}
```
📌 **Usage:**  
- Used when you need **user-specific data** throughout a session.

---

### **5️ Application (For Web Applications)**
- A **single bean instance** is shared across the **entire application** (similar to Singleton).
- Exists as long as the ServletContext exists.

📌 **Example:**
```java
@Component
@Scope("application")
public class MyApplicationBean {
}
```
📌 **Usage:**  
- When you need **global data** across all sessions and requests.

---

### **Summary Table 📝**
| Scope        | Instance Creation | Lifetime |
|-------------|----------------|----------|
| **Singleton (Default)** | One instance per Spring container | Throughout the application |
| **Prototype** | New instance every time | Created & destroyed per request |
| **Request** | One instance per HTTP request | Until request completes |
| **Session** | One instance per HTTP session | Until session ends |
| **Application** | One instance per web application | Until application stops |
