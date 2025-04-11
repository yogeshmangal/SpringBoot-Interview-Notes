# **JPA vs Hibernate**

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

---

## Two Common Ways to Interact with Database in JPA/Hibernate

## 1. Manual DAO + EntityManager (Old School / Custom Way)

In this approach, you:

1. Create a DAO interface.
2. Create a class that implements the DAO interface.
3. Use `EntityManager` inside the DAO implementation to write all CRUD operations.

📌 **Example:**

```java
// DAO Interface
public interface StudentDAO {
    void save(Student theStudent);        // CREATE
    Student findById(int id);             // READ
    List<Student> findAll();              // READ All
    void delete(int id);                  // DELETE
}

// DAO Implementation using EntityManager
@Repository
public class StudentDAOImpl implements StudentDAO {

    private EntityManager entityManager;

    @Autowired
    public StudentDAOImpl(EntityManager theEntityManager) {
        this.entityManager = theEntityManager;
    }

    @Override
    @Transactional
    public void save(Student theStudent) {
        entityManager.persist(theStudent);
    }

    @Override
    public Student findById(int id) {
        return entityManager.find(Student.class, id);
    }

    @Override
    public List<Student> findAll() {
        return entityManager.createQuery("from Student", Student.class).getResultList();
    }

    @Override
    @Transactional
    public void delete(int id) {
        Student student = entityManager.find(Student.class, id);
        entityManager.remove(student);
    }
}
```

## 2. Using Spring Data JPA Repositories (Modern & Easy)

In this, You just create an interface that extends JpaRepository, and you're done.

📌 **Example:**

```java
public interface StudentRepo extends JpaRepository<Student, Integer> {
    // No need to implement anything
    // You get all CRUD methods: save, findById, delete, etc.
}
```

- ✅ Clean, minimal, and faster.
- ✅ Spring generates the implementation behind the scenes using proxies + Hibernate.
- ✅ You can even define custom queries using method names or @Query.

So the conclusion is:

- ✅ Use DAO + EntityManager if you want fine-grained control over the persistence layer.
- 🚀 Use Spring Data JPA for faster development and access to powerful built-in repository methods.

---

## What is EntityManager?

`EntityManager` is a part of **JPA (Java Persistence API)**. It acts as the main interface to interact with the database.

### 📌 Responsibilities of EntityManager:

- **Persist**: Save new entities to the database.  
- **Find**: Retrieve entities using their primary key.  
- **Merge**: Update existing entities.  
- **Remove**: Delete entities from the database.  
- **CreateQuery**: Execute JPQL or native SQL queries.

---

## ⚙️ Injecting EntityManager in Spring

In Spring applications, `EntityManager` is typically injected using constructor injection:

```java
@Autowired
public StudentDAOImpl(EntityManager theEntityManager) {
    this.entityManager = theEntityManager;
}
```

---

## Writing Custom Queries in Spring Data JPA
Spring Data JPA allows us to write custom queries in two main ways:

### 1. Using JPQL or Native SQL with **@Query** Annotation
You can write your own queries using the @Query annotation above the method in your repository interface.

📌 **Example: JPQL Query**

```java
public interface StudentRepository extends JpaRepository<Student, Integer> {

    @Query("SELECT s FROM Student s WHERE s.lastName = ?1")
    List<Student> findByLastName(String lastName);
}
```
✅ Here, ?1 refers to the first method parameter.


📌 **Example: Native SQL Query**
```java
public interface StudentRepository extends JpaRepository<Student, Integer> {

    @Query(value = "SELECT * FROM student WHERE last_name = ?1", nativeQuery = true)
    List<Student> findByLastNameNative(String lastName);
}
```
✅ You just set nativeQuery = true to write actual SQL.

**Note:** JPQL queries are written in a database-independent manner, focusing on the entities while native queries use pure SQL and are written with database-specific syntax.

### 2. Derived Query Methods (No Query Annotation Needed)
Spring Data JPA can also generate queries automatically based on method names.
```java
List<Student> findByFirstName(String firstName);
```
💡 This automatically generates:
```
SELECT * FROM student WHERE first_name = ?
```

### 🔥 Bonus: Parameters with Named Variables
```
@Query("SELECT s FROM Student s WHERE s.email = :email")
Student findByEmail(@Param("email") String email);
```
✅ More readable and flexible when you have multiple parameters.