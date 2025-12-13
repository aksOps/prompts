---
description: 'Java Development Guidelines, Modern Standards, and Safety Practices.'
applyTo: '**.java, **.properties, **.gradle, **.xml, **.jar'
---

# Java Development Guidelines & Standards

## Your Mission
Your goal is to enforce modern Java standards (Java 17/21 LTS), ensuring that all Java code is robust, secure, and compatible with Windows environments. You champion composition, immutability, and type safety.

## Introduction
Java has evolved significantly. We do not write "Java 8" style code anymore. This guide establishes the standards for Enterprise Backend Development, Spring Boot, and Microservices, focusing on modern features like Records, pattern matching, and the NIO file API.

## Java Fundamentals & Architecture

### Modern Standards
- **Version:** Java 17 or 21 (LTS) only. Deprecate Java 8 patterns.
- **DTOs:** **ALWAYS** use `record` types for Data Transfer Objects. Stop generating getter/setter boilerplate.
- **Var:** Use `var` for local variable type inference where the type is obvious.

### Object-Oriented Design
- **Composition over Inheritance:** Favor composition. Inheritance creates tight coupling.
- **Interfaces:** Use Interfaces heavily to define contracts and enable loose coupling.
- **Immutability:** default to `final` fields and immutable collections (`List.of()`).

**Example - Modern DTO:**
```java
// ✅ Good: Concise, immutable, readable
public record UserDto(String id, String username, String email) {}
```
**Example - Legacy DTO (Avoid):**
```java
// ❌ Bad: Boilerplate, mutable
public class UserDto {
    private String id;
    // ... getters, setters, equals, hashcode, toString ...
}
```

## Safety & Security

### File I/O & Path Safety
Windows compatibility is paramount.
- **Path Handling:**
    - ❌ **NEVER** use string concatenation for paths: `"C:\\" + folder`.
    - ✅ **ALWAYS** use `java.nio.file.Path` and `Path.of()` to handle separators automatically.
- **Operations:** Use `java.nio.file.Files` for modern, non-blocking I/O.

**Example - Safe File Reading:**
```java
public String loadConfig(String fileName) throws IOException {
    // 🛡️ Safe for Windows and Linux
    Path path = Path.of("config", fileName); 
    return Files.readString(path);
}
```

### Dependency Injection
- Avoid `new` keywords in business logic.
- Use Constructor Injection for required dependencies.

## Testing & Quality Assurance

### Frameworks
- **Primary:** JUnit 5 (Jupiter).
- **Mocking:** Mockito.

### Testing Patterns
- **Structure:** Use `// Given`, `// When`, `// Then` comments in test bodies.
- **Isolation:** Mock external dependencies; test logic in isolation.
- **Naming:** Test method names should be descriptive (e.g., `shouldThrowExceptionWhenUserNotFound`).

**Example - JUnit 5 Test:**
```java
@Test
void shouldCalculateTotalCorrectly() {
    // Given
    var cart = new Cart();
    cart.add(new Item("Apple", 1.0));
    
    // When
    var total = cart.calculateTotal();
    
    // Then
    assertEquals(1.0, total);
}
```

## Anti-patterns
- **Lombok Abuse:** Avoid `@Data` on complex domain entities; it breaks encapsulation.
- **Swallowed Exceptions:** Never `catch (Exception e) {}`. Log it or rethrow it.
- **Field Injection:** Avoid `@Autowired` on fields. It makes testing difficult.
