## **JPA vs Hibernate**

### 🔹 What is JPA?
- **JPA (Java Persistence API)** is a **specification** provided by Java to define how Java objects should be persisted into a relational database.
- JPA defines interfaces and annotations but **does not provide any implementation**.
- It's a set of **rules and guidelines** for ORM (Object Relational Mapping).
- It includes annotations like `@Entity`, `@Id`, `@Column`, etc.

### 🔹 What is Hibernate?
- **Hibernate** is a popular **implementation** of the JPA specification.
- It provides the actual functionality for saving, updating, deleting, and retrieving objects from the database.
- Hibernate automatically generates SQL queries based on entity definitions and manages the interaction with the DB.
- In short, Hibernate is the implementation of those JPA rules. It handles the low-level work like generating SQL queries, managing database connections, caching, etc.

### 🔹 How do they work together?
- You write your code using **JPA annotations and interfaces**.
- Under the hood, **Hibernate** handles the actual database interaction.

### 🔹 Example:
```java
// JPA in action
public interface StudentRepo extends JpaRepository<Student, Integer> {
    // No implementation needed
}
```
Here,
- `JpaRepository` is part of **JPA**.
- Spring Boot internally uses **Hibernate** to implement this repository.

### ✅ Summary:
- **JPA** = What to do (rules + annotations)
- **Hibernate** = How to do (actual implementation)

> You use **JPA to write clean, framework-independent code**, and let **Hibernate handle the heavy lifting** behind the scenes.