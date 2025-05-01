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

## 8. @PostConstruct (Initialization Method)  
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

## 9. @PreDestroy (Cleanup Method)  
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

- **Note:** To use all the annotations we’ve discussed so far (@Entity, @Table, @Id, @GeneratedValue, @Query, @Modifying, @Transactional), you'll mainly need spring-boot-starter-data-jpa dependency and a database driver (MySql or PostgreSql) dependency. 

---

# Path Parameter(@PathVariable) vs Query Parameter(@RequestParam) in Spring Boot

## 18. @PathVariable
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

## 19. @RequestParam
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
| **Required**        | Always required              | Optional (can set `required=false`)|

---
# Optional @RequestParam in Spring Boot

In Spring Boot, you can make a `@RequestParam` optional by setting `required = false`. This allows the parameter to be excluded from the request without causing an error.

## Basic Usage
```java
@GetMapping("/greet")
public String greetUser(@RequestParam(required = false) String name) {
    return name != null ? "Hello, " + name : "Hello, Guest!";
}
```

## With Default Value
You can also specify a `defaultValue` so you don't have to check for null manually:
```java
@GetMapping("/greet")
public String greetUser(@RequestParam(required = false, defaultValue = "Guest") String name) {
    return "Hello, " + name;
}
```

## Example Calls
- `/greet?name=Yogesh` → `"Hello, Yogesh"`
- `/greet` → `"Hello, Guest"`

Using `defaultValue` simplifies the logic and ensures consistent behavior even when the parameter is not provided.

---

# Exception Handling in Spring Boot

## 21. @ExceptionHandler

`@ExceptionHandler` is used to handle exceptions at the **controller level**.

### Example:
```java
@RestController
public class StudentController {

    @GetMapping("/student/{id}")
    public Student getStudent(@PathVariable int id) {
        if (id < 1) {
            throw new IllegalArgumentException("Invalid ID");
        }
        return new Student();
    }

    @ExceptionHandler(IllegalArgumentException.class)
    public ResponseEntity<String> handleInvalidId(IllegalArgumentException ex) {
        return new ResponseEntity<>(ex.getMessage(), HttpStatus.BAD_REQUEST);
    }
}
```

📌 Handles the `IllegalArgumentException` only within this controller.

---

## 22. @ControllerAdvice

`@ControllerAdvice` is used to define **global exception handlers** that work across all controllers.

### Example:
```java
@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(IllegalArgumentException.class)
    public ResponseEntity<String> handleIllegalArgument(IllegalArgumentException ex) {
        return new ResponseEntity<>("Global Handler: " + ex.getMessage(), HttpStatus.BAD_REQUEST);
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<String> handleGeneralException(Exception ex) {
        return new ResponseEntity<>("Something went wrong: " + ex.getMessage(), HttpStatus.INTERNAL_SERVER_ERROR);
    }
}
```

📌 Catches exceptions thrown from any controller in the app.

---

## When to Use

| Scenario                                           | Use                    |
|----------------------------------------------------|-------------------------|
| Handle exceptions within a specific controller     | `@ExceptionHandler`     |
| Handle exceptions globally across all controllers  | `@ControllerAdvice`     |

---

# @Valid Annotation in Spring Boot

## 23. @Valid
`@Valid` is used to trigger validation on an object (often a request body) before the controller processes it. It comes from the **javax.validation** (Jakarta) package and works with the **Bean Validation API** like Hibernate Validator.

---

## 🔍 How Does It Work?
When you annotate a method parameter with `@Valid`, Spring Boot:
- Automatically checks for constraint annotations (`@NotNull`, `@Size`, `@Email`, etc.) on the object.
- Throws a `MethodArgumentNotValidException` if validation fails.

---

## 🧪 Controller Example
```java
@RestController
@RequestMapping("/api/students")
public class StudentController {

    @PostMapping
    public ResponseEntity<String> createStudent(@RequestBody @Valid Student student) {
        return ResponseEntity.ok("Student created successfully!");
    }
}
```

---

## 🧾 Student Entity Example
```java
public class Student {

    @NotBlank(message = "Name is mandatory")
    private String name;

    @Min(value = 18, message = "Age must be at least 18")
    private int age;

    @Email(message = "Email should be valid")
    private String email;

    // Getters and Setters
}
```

---

## 📝 Summary
- `@Valid` enables automatic request validation.
- Works with constraint annotations inside Java classes.

---

# Bean Validation: @NotNull vs @NotBlank

## 24. @NotNull
- Ensures that a field is **not null**.
- Allows empty values like `""` (empty string).

```java
@NotNull
private String name;
```

📌 **Fails if:** `name = null`
✅ **Passes if:** `name = ""`

## 25. @NotBlank
- Ensures that a field is **not null, not empty, and not just whitespace**.
- More strict than `@NotNull`.

```java
@NotBlank
private String name;
```

📌 **Fails if:** `name = null`, `name = ""`, or `name = "   "`

## Quick Comparison
| Use Case | Annotation |
|----------|------------|
| Must not be `null` | `@NotNull` |
| Must not be `null`, `""`, or whitespace | `@NotBlank` |
| Must not be `null` or empty (for arrays/collections) | `@NotEmpty` |

## Bonus: When to Use What
- 🧪 Use `@NotNull` if you only want to avoid null references.
- 🔒 Use `@NotBlank` when you require meaningful, non-whitespace input.
- 📦 Use `@NotEmpty` for validating collections or strings that shouldn't be empty.

---

# @RequestMapping and Shortcut Annotations in Spring Boot

## 26. @RequestMapping

`@RequestMapping` is used to map HTTP requests to specific controller methods. It can be used at both class and method levels.

### Example:
```java
@RestController
@RequestMapping("/api")
public class MyController {

    @RequestMapping(value = "/hello", method = RequestMethod.GET)
    public String sayHello() {
        return "Hello, World!";
    }

    @RequestMapping(value = "/create", method = RequestMethod.POST, consumes = "application/json")
    public ResponseEntity<String> createSomething(@RequestBody MyDto dto) {
        return ResponseEntity.ok("Created");
    }
}
```

## Shortcut Annotations
Spring Boot provides specific annotations as shortcuts for common HTTP methods:
-(i)  PostMapping
-(ii) PutMapping
-(iii)DeleteMapping
-(iv) GetMapping

## 27. @PostMapping
Maps HTTP POST requests to a method.
```java
@PostMapping("/create")
public ResponseEntity<String> createSomething(@RequestBody MyDto dto) {
    return ResponseEntity.ok("Created");
}
```

## 28. @PutMapping
Maps HTTP PUT requests to a method.
```java
@PutMapping("/update")
public ResponseEntity<String> updateSomething(@RequestBody MyDto dto) {
    return ResponseEntity.ok("Updated");
}
```

## 29. @DeleteMapping
Maps HTTP DELETE requests to a method.
```java
@DeleteMapping("/delete/{id}")
public ResponseEntity<String> deleteSomething(@PathVariable int id) {
    return ResponseEntity.ok("Deleted");
}
```

## 30. @GetMapping
Maps HTTP GET requests to a method.
```java
@GetMapping("/something")
public ResponseEntity<String> getSomething() {
    return ResponseEntity.ok("Hey");
}
```

## One-line Definitions
- `@PostMapping` → Maps HTTP POST requests to handler methods.
- `@PutMapping` → Maps HTTP PUT requests to handler methods.
- `@DeleteMapping` → Maps HTTP DELETE requests to handler methods.
- `@GetMapping` → Maps HTTP GET requests to handler methods.

These annotations improve code readability and are preferred over `@RequestMapping` when handling specific HTTP methods.

---

# Lombok Constructors Cheat Sheet

A quick reference guide to `@AllArgsConstructor`, `@NoArgsConstructor`, and `@RequiredArgsConstructor` with their behavior on `final`, `non-final`, and `static` fields.

---

## 31. @NoArgsConstructor

- **Description:** Generates a constructor with **no parameters**.
- **Use Case:** Needed when frameworks (like Hibernate, Jackson, JPA) require a no-arg constructor and you have defined other constructors manually.
- **Behavior:**
  - **Final Fields:** Not initialized unless you use `force = true`.
  - **Non-Final Fields:** Initialized to their default values (null, 0, false).
  - **Static Fields:** Ignored.
- **Extra:**
  - Use `@NoArgsConstructor(force = true)` if final fields exist and you still need a no-args constructor.

```java
@NoArgsConstructor(force = true)
public class Example {
    private final String name;
    private int age;
}
```

---

## 32. @AllArgsConstructor

- **Description:** Generates a constructor with **one parameter for each field** in the class.
- **Use Case:** When you want to easily create objects by passing all fields.
- **Behavior:**
  - **Final Fields:** Included as parameters.
  - **Non-Final Fields:** Included as parameters.
  - **Static Fields:** Ignored.

```java
@AllArgsConstructor
public class Example {
    private final String name;
    private int age;
    private static String staticField; // Ignored
}
```

Constructor generated:
```java
public Example(String name, int age) {
    this.name = name;
    this.age = age;
}
```

---

## 33. @RequiredArgsConstructor

- **Description:** Generates a constructor for **final fields** and fields annotated with `@NonNull`.
- **Use Case:** When you want only essential fields (final or @NonNull) to be required for object creation.
- **Behavior:**
  - **Final Fields:** Included as parameters.
  - **@NonNull Non-Final Fields:** Included as parameters.
  - **Static Fields:** Ignored.
  - **Other Non-Final Fields:** Ignored.

```java
@RequiredArgsConstructor
public class Example {
    private final String name;
    @NonNull private Integer age;
    private String city; // Ignored
}
```

Constructor generated:
```java
public Example(String name, Integer age) {
    this.name = name;
    this.age = age;
}
```

---

# Quick Summary Table

| Annotation              | Final Fields | Non-Final Fields | Static Fields | Notes |
|-------------------------|--------------|------------------|---------------|-------|
| `@NoArgsConstructor`     | Not initialized unless `force=true` | Default values | Ignored | Needed for frameworks |
| `@AllArgsConstructor`    | Included | Included | Ignored | All fields constructor |
| `@RequiredArgsConstructor` | Included | Only `@NonNull` fields | Ignored | Minimal required fields |

---

# Additional Tip
- **Static fields** are **never** included in any constructor generated by Lombok.
- If you manually define any constructor, **Java will NOT** provide a default no-args constructor, so you may need `@NoArgsConstructor`.

---

