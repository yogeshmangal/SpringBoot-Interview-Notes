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