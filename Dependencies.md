# Spring Core Dependencies Summary  

## 1. spring-boot-starter-web
The `spring-boot-starter-web` dependency is used to build web applications, including RESTful APIs.  

### 🔹 What does it provide?  
- **Embedded Web Server:** Includes **Tomcat** (default), Jetty, or Undertow.  
- **Spring MVC:** For handling HTTP requests and responses.  
- **Jackson:** For automatic JSON serialization and deserialization.  
- **Validation:** Supports request validation using annotations like `@Valid`.  

### 🔹 How it works:  
- Adding `spring-boot-starter-web` in `pom.xml` auto-configures everything needed for a web application.  
- It sets up an embedded Tomcat server and enables Spring MVC.  
- Handles JSON conversion automatically using Jackson.  

**In short:**  
This dependency provides all essential components to create a web application in Spring Boot with minimal configuration.

---

## 2. spring-boot-starter-security
The `spring-boot-starter-security` dependency is used to add authentication and authorization features to a Spring Boot application.  

### 🔹 What does it provide?  
- **Basic Authentication:** Enables default username/password-based authentication.  
- **Security Filters:** Automatically secures all endpoints with Spring Security.  
- **User Management:** Provides a default user (`user`) and generates a random password at startup.  
- **Customization:** Allows defining custom security configurations using `SecurityFilterChain`.  

### 🔹 How it works:  
- Adding `spring-boot-starter-security` in `pom.xml` secures all API endpoints by default.  
- Uses **Basic Authentication** with username as `user` and a generated password (found in logs).
- Developers can override default security settings by defining a custom security configuration.  

### 🔹 **Important Notes:**  
- We can also give our username and password. So for that in application.properties file:
```yaml
  spring.security.user.name: Yogesh
  spring.security.user.password: test@4321
  ```

---

## 3. spring-boot-devtools
The `spring-boot-devtools` dependency enhances the development experience by providing features like automatic restart, live reload, and disabling template caching.  

### 🔹 What does it provide?  
- **Automatic Restart:** Restarts the application when code changes are detected.  
- **Live Reload:** Refreshes the browser when static resources change (requires LiveReload extension).  
- **Disable Template Caching:** Ensures templates (Thymeleaf, FreeMarker, etc.) reflect changes without restart.  
- **Remote Debugging Support:** Allows DevTools to work with remote applications.  

### 🔹 How it works:  
- Adding `spring-boot-devtools` in `pom.xml` enables automatic restart and live reload.  
- It is **disabled in production** (automatically turned off in packaged JAR/WAR files).  
- Works best in **IDE development mode** for fast iteration cycles.  

---

## 4. spring-boot-starter-actuator 

The `spring-boot-starter-actuator` dependency provides production-ready features like monitoring, metrics, health checks, and system information for a Spring Boot application.  

### 🔹 **What does it provide?**  
- **Health Checks:** Monitors the application's health (e.g., `/actuator/health`).  
- **Metrics & Monitoring:** Provides insights into CPU usage, memory, and request statistics.  
- **Environment Info:** Exposes application properties, beans, and configuration details.  
- **Thread Dump & Heap Dump:** Helps in debugging performance issues.  
- **Custom Endpoints:** Developers can create custom actuator endpoints.  

### 🔹 **How it works?**  
- Add `spring-boot-starter-actuator` in `pom.xml`.  
- Enable/disable specific endpoints via `application.properties` or `.yml`.  
- Use `/actuator` endpoints for monitoring and management.  

### 🔹 **Important Notes:**  
- The **prefix is always `/actuator`** for all endpoints.  
- By default, only `/health` is exposed. To expose additional endpoints, configure them in `application.yml`:  
  
  ```yaml
  management.endpoints.web.exposure.include = health,info
  management.info.env.enabled=true
  ```  
  
  Now, `/info` can be accessed via:  
  
  `http://127.0.0.1:8080/actuator/info`

- To expose **all** actuator endpoints, use `*` in `application.yml`:  
  
  ```yaml
  management.endpoints.web.exposure.include = "*"
  ```  
  This ensures that all endpoints, including `/info`, are fully accessible.

---

## 5. spring-boot-starter-data-jpa
Provides all the necessary components to use Spring Data JPA with Hibernate as the default ORM provider.
**Includes:**  
- Hibernate
- Spring ORM
- JPA Annotations (`@Entity`, `@Id`, `@GeneratedValue`, etc.)
- Repository support (`JpaRepository`, `CrudRepository`, etc.)

**Use Case:**  
Use this to interact with relational databases using JPA and repositories.

---

## 6. postgresql
JDBC driver for PostgreSQL. Required to connect your Spring Boot app to a PostgreSQL database.

---

## 7. h2
Lightweight, in-memory database for testing and rapid development.
```
<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <scope>runtime</scope>
</dependency>
```
✅ If you use H2, you don’t need to install/configure any external database like MySQL or PostgreSQL. It works straight out of the box — no setup hustle.
### **🧪 It’s mainly used for:**
- Quick testing
- Learning
- Small-scale internal demos
- Running integration tests
📉 But since everything is stored in memory, the data vanishes as soon as the app stops (unless you use file-based mode).

---

## 8. Note:   
Whenever we **add or remove dependencies**, **change the project structure**, or **set up a new Spring Boot project**, it’s recommended to do a **clean build** to avoid conflicts or stale artifacts.

#### Commands
- **If using Maven:**
```bash
mvn clean install
```
- **If using Gradle:**
```bash
./gradlew clean build
```
---

