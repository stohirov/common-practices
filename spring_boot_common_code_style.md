# 📝 Spring Boot Code Style Guide for Technical Tasks

## 📌 General Best Practices
- **Follow Java Naming Conventions**:
  - Class names: `PascalCase` (e.g., `UserServiceImpl`)
  - Method/variable names: `camelCase` (e.g., `getUserById`)
  - Constants: `UPPER_SNAKE_CASE` (e.g., `DEFAULT_PAGE_SIZE`)
- **Use Meaningful Names**: Avoid generic names like `data`, `temp`, `obj`.
- **Keep Methods Short & Focused**: Aim for a **single responsibility** per method.
- **Avoid Magic Numbers & Strings**: Extract them as constants or use **configuration properties**.
- **Use Optional & Null-Safety Practices**: Avoid returning `null`; prefer `Optional<T>`.

---

## 📂 Spring Boot Project Structure
A well-organized project follows the **layered architecture**:

```
com.example.app
│── controller/      -> Handles HTTP requests
│── service/         -> Business logic
│── repository/      -> Database access (JPA, JDBC, etc.)
│── model/           -> Entities, DTOs
│── config/          -> Application configurations
│── exception/       -> Custom exceptions and handlers
```
- **Avoid fat controllers/services**: Move logic to services instead of controllers.

---

## 📌 Dependency Injection
- **Prefer constructor-based injection** over field injection:
  
  ```java
  @Service
  public class UserService {
      private final UserRepository userRepository;
      
      public UserService(UserRepository userRepository) {
          this.userRepository = userRepository;
      }
  }
  ```

---

## ⚙️ Configuration Management
- Use **`@ConfigurationProperties`** instead of `@Value`:
  
  ```java
  @ConfigurationProperties(prefix = "app")
  public class AppProperties {
      private String url;
      // Getters and setters
  }
  ```
- Keep sensitive configs in **`application.yml`**.

---

## 📌 Logging Best Practices
- Use **SLF4J (`@Slf4j`)** instead of `System.out.println()`:
  
  ```java
  @Slf4j
  public class UserService {
      public void processUser() {
          log.info("Processing user...");
      }
  }
  ```

---

## ❌ Exception Handling
- Use **`@ControllerAdvice`** for global error handling:
  
  ```java
  @RestControllerAdvice
  public class GlobalExceptionHandler {
      @ExceptionHandler(UserNotFoundException.class)
      public ResponseEntity<String> handleUserNotFound(UserNotFoundException ex) {
          return ResponseEntity.status(HttpStatus.NOT_FOUND).body(ex.getMessage());
      }
  }
  ```

---

## 🌍 REST API Best Practices
- **Use Proper HTTP Status Codes** (`200 OK`, `201 Created`, `404 Not Found`, etc.).
- **Consistent URL Naming** (e.g., `/users/{id}` instead of `/getUser?id=1`).
- **Use `ResponseEntity<>`** for API responses:
  
  ```java
  @GetMapping("/{id}")
  public ResponseEntity<UserDTO> getUser(@PathVariable Long id) {
      UserDTO user = userService.getUserById(id);
      return ResponseEntity.ok(user);
  }
  ```
- **DTOs over Entities in API Responses** to avoid exposing DB structure.

---

## 🗄️ JPA & Database Best Practices
- **Use Spring Data JPA instead of plain JDBC**.
- **Use `@Transactional` where necessary**.
- **Paginate large queries with `Pageable`**:
  
  ```java
  Page<User> findAll(Pageable pageable);
  ```

---

## ✅ Testing in Spring Boot
### **Unit Tests (JUnit & Mockito)**
```java
@ExtendWith(MockitoExtension.class)
public class UserServiceTest {
    @Mock private UserRepository userRepository;
    @InjectMocks private UserService userService;
}
```

### **Integration Tests**
```java
@SpringBootTest
@RunWith(SpringRunner.class)
public class UserServiceIntegrationTest { }
```

---

## 🔒 Security Best Practices
- **Use `@PreAuthorize` for method-level security**:
  ```java
  @PreAuthorize("hasRole('ADMIN')")
  public void deleteUser(Long id) { }
  ```
- **Store passwords securely** (`BCryptPasswordEncoder`).
- **Use CSRF protection** for security.

---

## 📌 Code Style Enforcement
- Use **Checkstyle / SonarLint** for consistency.
- Follow **Google Java Style Guide** or **Spring Boot Best Practices**.

---

## 🎯 Conclusion
By following these best practices, your Spring Boot code will be **clean, scalable, and professional**, aligning with industry expectations for technical tasks and interviews. 🚀
