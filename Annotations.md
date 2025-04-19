# Spring Core Annotations Summary  

## 1. @Component  
Marks a Java class as a Spring-managed bean.  

### @Component annotation has the following sub-types:  

- **@RestController:** Used for RESTful web services (combines `@Controller` and `@ResponseBody`).  
- **@Service:** Indicates a service layer component containing business logic.  
- **@Repository:** Marks a data access object (DAO) and provides exception translation.  

**Note:**  
If you’re creating a Java class in a Spring Boot project and none of the specialized annotations (`@RestController`, `@Service`, `@Repository`) are suitable for the purpose of that class, you can use **@Component** to mark it as a Spring-managed bean.

### Example

```
@Component
public class MyComponent {
    public void doSomething() {
        System.out.println("Doing something...");
    }
}
```


## 2. @Configuration
Marks a class as a source of bean definitions. It is used when you want to manually configure and define beans, especially when integrating third-party libraries or code that Spring doesn’t automatically detect as a bean.  

### 🔑 Why Use @Configuration Instead of @Component:  
- Third-party libraries or external code cannot be annotated with **@Component** since you don’t have control over their source code.  
- To make such classes available as beans, you use **@Configuration** to explicitly declare beans using the **@Bean** annotation.  
- **@Bean** is a Spring annotation that registers a method's return value as a Spring-managed bean inside a @Configuration class.

### Usage:  
The **@Configuration** and **@Bean** annotations are used together to define and manage beans manually.  

**Example:**  
```
@Configuration
public class AppConfig {

    @Bean
    public MyService myService() {
        return new MyService();
    }
}
```

**Note:**
In Simple terms, a Bean is a Java class that is managed and recognized by the Spring IoC container. A normal Java class is not recognized by Spring on its own. We need to provide specific annotations (like @Component, @Service, @Repository, or @Configuration) to make it a bean, and then Spring can recognize and manage it.


## 3. @Value
The `@Value` annotation injects values into fields from properties files, system environment variables, or default values.

### 🔹 **Usage Examples**  

#### **️ Injecting from `application.properties`**  
**`application.properties`**  
```properties
app.name=Spring Boot Application
```
**Java Class**  
```java
@Value("${app.name}")
private String appName;
```
✅ Injects `appName` with `Spring Boot Application`.

---

#### ** Setting Default Values**  
```java
@Value("${app.version:1.0}")
private String appVersion;
```
✅ If `app.version` is missing, `appVersion` defaults to `1.0`.

---

#### ** Injecting System Environment Variables**  
```java
@Value("${JAVA_HOME}")
private String javaHome;
```
✅ Injects system's `JAVA_HOME` path.

---

#### ** Using Spring Expression Language (SpEL)**  
```java
@Value("#{10 + 20}")
private int sum;
```
✅ `sum` is assigned `30`.


## 4. @Autowired
The `@Autowired` annotation in Spring is used for **automatic dependency injection**. It tells Spring to resolve and inject the required bean **automatically**, reducing the need for manual object creation.  

### **📌 How it Works?**  
- When `@Autowired` is placed on a **field, constructor, or setter**, Spring looks for a matching bean and injects it.  
- It works with **Spring's IoC (Inversion of Control) container** to manage dependencies.  

### **🔹 Key Points:**  
✔ `@Autowired` enables automatic dependency resolution.  
✔ Works on **fields, constructors, and setter methods**.  
✔ If multiple beans exist, use `@Qualifier` or `@Primary` to specify which bean to inject.  
✔ **Constructor injection is preferred** as it ensures required dependencies and immutability.

---

## 5. @Qualifier
When multiple beans of the same type exist, Spring doesn’t know which one to inject. `@Qualifier` helps specify the exact bean to use.  

#### **📌 Example (Constructor Injection):**  
```java
@Component
class PetrolEngine implements Engine { }

@Component
class DieselEngine implements Engine { }

@Component
class Car {
    private Engine engine;

    @Autowired
    public Car(@Qualifier("petrolEngine") Engine engine) {  // "petrolEngine" (first letter lowercase)
        this.engine = engine;
    }
}
```
🔹 **Key Points:**  
- The bean name must match the class name with a lowercase first letter.
- `@Qualifier("petrolEngine")` tells Spring to inject `PetrolEngine` instead of `DieselEngine`.  

---

## 6. @Primary
If multiple beans exist and no `@Qualifier` is used, Spring injects the bean marked with `@Primary` by default.  

#### **📌 Example:**  
```java
@Component
@Primary  // Marks DieselEngine as the default bean
class DieselEngine implements Engine { }

@Component
class PetrolEngine implements Engine { }

@Component
class Car {
    private Engine engine;

    @Autowired
    public Car(Engine engine) {  // No @Qualifier used
        this.engine = engine;
    }
}
```
🔹 **Key Points:**  
- `@Primary` removes the need for `@Qualifier` unless we want to override it.
- If both `@Primary` and `@Qualifier` are used, **`@Qualifier` takes precedence**.  

---

## 7. @Lazy
By default, Spring creates all beans at startup. `@Lazy` prevents this, delaying bean creation until it's actually needed.  

#### **📌 Example:**  
```java
@Component
@Lazy  // This bean will not be created at startup
class HeavyDatabaseConnection {
    public HeavyDatabaseConnection() {
        System.out.println("Database Connection Initialized!");
    }
}

@Component
class Application {
    private HeavyDatabaseConnection dbConnection;

    @Autowired
    public Application(HeavyDatabaseConnection dbConnection) {
        this.dbConnection = dbConnection;
    }
}
```
🔹 **Key Points:**  
- Without `@Lazy`, the `HeavyDatabaseConnection` instance is created at startup.
- With `@Lazy`, it is only created when `Application` accesses it for the first time.  

---

### **Summary:**  
- **`@Qualifier`** → Specifies which bean to inject when multiple exist
- **`@Primary`** → Sets a default bean, avoiding the need for `@Qualifier` everywhere.
- **`@Lazy`** → Delays bean creation until it's actually needed.  

---

## **@PostConstruct & @PreDestroy Annotations**  

In Spring, `@PostConstruct` and `@PreDestroy` annotations are used for lifecycle callback methods, allowing developers to define custom initialization and cleanup logic for beans.  

## 8. @PostConstruct(Initialization Method)  
- Runs **after the bean is created and dependencies are injected**.  
- Used to perform setup operations (e.g., initializing resources).  
- It is invoked **only once**, after dependency injection.  

📌 **Example:**  
```java
@Component
public class MyBean {
    @PostConstruct
    public void init() {
        System.out.println("Bean is initialized!");
    }
}
```
**Output when bean is created:**  
```
Bean is initialized!
```

## 9. @PreDestroy Cleanup Method)  
- Runs **just before the bean is destroyed**.  
- Used to release resources, close connections, etc.  
- It is **not invoked for prototype beans**, as Spring doesn’t manage their lifecycle fully.  

📌 **Example:**  
```java
@Component
public class MyBean {
    @PreDestroy
    public void cleanup() {
        System.out.println("Bean is about to be destroyed!");
    }
}
```
**Output when application shuts down:**  
```
Bean is about to be destroyed!
```

### 🔹 **Important Notes:**  
- These annotations work **only with Spring-managed beans**.  
- `@PostConstruct` runs **only once**, even for singleton beans.  
- `@PreDestroy` is ignored for **prototype beans** since Spring does not manage their complete lifecycle.  
- Alternative: Instead of these annotations, you can define lifecycle methods using `initMethod` and `destroyMethod` in `@Bean` annotation.  

📌 **Example using @Bean:**  
```java
@Bean(initMethod = "init", destroyMethod = "cleanup")
public MyBean myBean() {
    return new MyBean();
}
```
---

##JPA & Spring Data Annotations

## 10. @Query
`@Query` is used in Spring Data JPA to define custom queries using JPQL or native SQL.

### Example:
```java
@Query("SELECT s FROM Student s WHERE s.lastName = ?1")
List<Student> findByLastName(String lastName);

@Query(value = "SELECT * FROM student WHERE last_name = ?1", nativeQuery = true)
List<Student> findByLastNameNative(String lastName);
```

## 11. @Modifying
`@Modifying` used with @Query when executing update or delete operations (non-select queries). It tells Spring Data JPA that this query modifies the data.

### Example:
```java
@Modifying
@Query("DELETE FROM Student s WHERE s.lastName = ?1")
void deleteByLastName(String lastName);
```

> ⚠️ Must be used with `@Transactional`

## 12. @Transactional
`@Transactional` marks a method or class to be executed within a transaction. Ensures commit or rollback happens automatically.

### Example:
```java
@Transactional
@Modifying
@Query("UPDATE Student s SET s.email = ?2 WHERE s.id = ?1")
void updateEmailById(Integer id, String email);
```

## 13. @Entity
Marks a class as a JPA entity.

### Example:
```java
@Entity
public class Student {
    // fields and annotations
}
```

## 14. @Table
Defines table name if it's different from the class name.

### Example:
```java
@Entity
@Table(name = "students")
public class Student {
    // fields
}
```

## 15. @Id
Marks the primary key of the entity.

### Example:
```java
@Id
private int id;
```

## 16. @GeneratedValue
Specifies how the primary key should be generated.

### Strategies:
- `AUTO`
- `IDENTITY`
- `SEQUENCE`
- `TABLE`

### Example:
```java
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
private int id;
```

## 17. @Column
Used to map Java fields to database columns.

### Options:
- `name` - column name
- `nullable` - whether the column is nullable
- `length` - length of a String column

### Example:
```java
@Column(name = "first_name", nullable = false, length = 50)
private String firstName;
```

- **Note:** To use all the annotations we’ve discussed so far (@Entity, @Table, @Id, @GeneratedValue, @Query, @Modifying, @Transactional), you'll mainly need Spring Data JPA and a database driver (MySql or PostgreSql). 

---

# Path Parameter vs Query Parameter in Spring Boot

## 18. Path Parameter
Path parameters are part of the **URL path** and are used to identify a specific resource.

### ✅ Use Case:
When you want to retrieve or operate on a specific resource by its ID or name.

### 📌 Example:
```java
@GetMapping("/students/{id}")
public Student getStudentById(@PathVariable int id) {
    return studentService.findById(id);
}
```

### 🛣️ Request URL:
```
GET /students/10
```
In this case, `10` is the path parameter.

---

## 19. Query Parameter
Query parameters are passed **after the `?` in the URL** and are usually used for filtering, sorting, or pagination.

### ✅ Use Case:
When you want to pass optional data to refine results.

### 📌 Example:
```java
@GetMapping("/students")
public List<Student> findByDepartment(@RequestParam String department) {
    return studentService.findByDepartment(department);
}
```

### 🔗 Request URL:
```
GET /students?department=ComputerScience
```

In this case, `department` is a query parameter.

---

## 20. Combining Both
You can use both path and query parameters together in one endpoint.

### 📌 Example:
```java
@GetMapping("/departments/{deptId}/students")
public List<Student> getStudentsByDeptAndYear(@PathVariable int deptId,
                                              @RequestParam int year) {
    return studentService.findByDeptAndYear(deptId, year);
}
```

### 🔗 Request:
```
GET /departments/3/students?year=2024
```

---

## 📝 Summary Table
| Feature         | Path Parameter              | Query Parameter                  |
|-----------------|-----------------------------|-----------------------------------|
| **Location**        | Part of the URL path        | After `?` in URL                 |
| **Use**             | Identify resource            | Filter, sort, paginate           |
| **Annotation**      | `@PathVariable`              | `@RequestParam`                 |
| **Required**        | Always required              | Optional (can set `required=false`) |

---