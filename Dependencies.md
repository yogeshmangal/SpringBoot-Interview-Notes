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

## 2. spring-boot-starter-security
The `spring-boot-starter-security` dependency is used to add authentication and authorization features to a Spring Boot application.  

### 🔹 What does it provide?  
- **Basic Authentication:** Enables default username/password-based authentication.  
- **Security Filters:** Automatically secures all endpoints with Spring Security.  
- **User Management:** Provides a default user (`user`) and generates a random password at startup.  
- **Customization:** Allows defining custom security configurations using `SecurityFilterChain`.  

### 🔹 How it works:  
- Adding `spring-boot-starter-security` in `pom.xml` secures all API endpoints by default.  
- Uses **Basic Authentication** with a generated password (found in logs).  
- Developers can override default security settings by defining a custom security configuration.  
