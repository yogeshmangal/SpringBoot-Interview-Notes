## **Common `application.properties` Configurations**  

### 🔹 ** 1️. Changing the Default Server Port**  
By default, the embedded server runs on **port 8080**. To change it:  
```properties
server.port=8585
```
✅ The application will now start on **port 8585**.

---

### 🔹 ** 2️. Adding Basic Security**  
To set a **custom username & password** for authentication:  
```properties
spring.security.user.name=Yogesh
spring.security.user.password=test@4321
```
✅ Default login credentials: **Yogesh / test@4321**

---

### 🔹 ** 3. Changing Context Path**  
To serve the application under a different base URL:  
```properties
server.servlet.context-path=/test
```
✅ Application URL changes from `localhost:8080/` to `localhost:8080/test`

---

### 🔹 ** 4. Configuring Database Connection**  
For a **MySQL** database connection:  
```properties
spring.datasource.url=jdbc:mysql://localhost:3306/do_communications
spring.datasource.username=postgres
spring.datasource.password=mysecretpassword
```
✅ Configures **MySQL database connectivity**.

---

### 🔹 ** 5. Disabling the Spring Boot Banner**  
To turn off the Spring Boot startup banner:  
```properties
spring.main.banner-mode=off
```
✅ The **Spring Boot banner** will no longer appear in the console.

---

### 🔹 ** 6. Enabling Automatic Table Creation**  
If **@Entity** is defined but tables do not exist, Hibernate can create them:  
```properties
spring.jpa.hibernate.ddl-auto=create
```
✅ Automatically **creates tables** based on entity definitions.

---

### 🔹 ** 7. Setting Hibernate Dialect (MySQL Example)**
This tells Hibernate (the JPA provider) which SQL dialect to use when generating SQL queries for the database.
#### Why is it needed?
- Different databases (MySQL, PostgreSQL, Oracle, etc.) have slightly different SQL syntax.
- Hibernate needs to know which database it is working with so it can generate optimized queries accordingly.
- Without this, Hibernate might generate incorrect or inefficient SQL statements.

```properties
spring.jpa.database-platform=org.hibernate.dialect.MySQL8Dialect
```
---
