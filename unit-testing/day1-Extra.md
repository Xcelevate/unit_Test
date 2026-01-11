# Day 1-Extra: Advanced JUnit & Mockito Basics

## Parameterized Tests (`@ParameterizedTest`), Dynamic Tests

### Parameterized Tests
- **Purpose**: Run the same test logic with multiple inputs.
- **Annotations**:
    - `@ParameterizedTest` marks the test.
    - `@ValueSource`, `@CsvSource`, `@MethodSource` provide inputs.
- **Example**:
```java
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.ValueSource;
import static org.junit.jupiter.api.Assertions.*;

class PalindromeTest {
    @ParameterizedTest
    @ValueSource(strings = {"madam", "racecar", "level"})
    void shouldReturnTrueForPalindromes(String candidate) {
        assertTrue(new StringBuilder(candidate).reverse().toString().equals(candidate));
    }
}
```

### Dynamic Tests
- **Purpose**: Generate tests at runtime.
- **Annotation**: `@TestFactory`.
- **Example**:
```java
import org.junit.jupiter.api.DynamicTest;
import org.junit.jupiter.api.TestFactory;
import java.util.Arrays;
import java.util.Collection;

class DynamicTestsExample {
    @TestFactory
    Collection<DynamicTest> dynamicTestsForAddition() {
        return Arrays.asList(
            DynamicTest.dynamicTest("1+1=2", () -> assertEquals(2, 1+1)),
            DynamicTest.dynamicTest("2+3=5", () -> assertEquals(5, 2+3))
        );
    }
}
```

---

## Nested Classes, Test Order Control

### Nested Classes
- **Purpose**: Organize related tests together.
- **Annotation**: `@Nested`.
- **Example**:
```java
import org.junit.jupiter.api.Nested;
import org.junit.jupiter.api.Test;

class UserServiceTest {
    @Nested
    class RegistrationTests {
        @Test void shouldRegisterUser() { /* ... */ }
    }

    @Nested
    class LoginTests {
        @Test void shouldLoginUser() { /* ... */ }
    }
}
```

### Test Order Control
- **Annotation**: `@TestMethodOrder`.
- **Example**:
```java
import org.junit.jupiter.api.*;

@TestMethodOrder(MethodOrderer.OrderAnnotation.class)
class OrderedTests {
    @Test @Order(1) void testA() { /* ... */ }
    @Test @Order(2) void testB() { /* ... */ }
}
```

---

## JaCoCo - Code Coverage
- **Purpose**: Measure how much of your code is executed by tests.
- **Setup**:
    - Add JaCoCo plugin in Maven/Gradle.
- **Maven Example**:
```xml
<plugin>
  <groupId>org.jacoco</groupId>
  <artifactId>jacoco-maven-plugin</artifactId>
  <version>0.8.10</version>
  <executions>
    <execution>
      <goals><goal>prepare-agent</goal></goals>
    </execution>
    <execution>
      <id>report</id>
      <phase>test</phase>
      <goals><goal>report</goal></goals>
    </execution>
  </executions>
</plugin>
```
- **Output**: HTML report showing coverage percentages.

---

## Mockito Setup (`@ExtendWith(MockitoExtension)`)

- **Purpose**: Integrate Mockito with JUnit 5.
- **Annotation**: `@ExtendWith(MockitoExtension.class)`.
- **Example**:
```java
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.junit.jupiter.MockitoExtension;

@ExtendWith(MockitoExtension.class)
class ServiceTest {
    // mocks go here
}
```

---

## `@Mock`, `@InjectMocks`, `@Spy` Basics

- **`@Mock`**: Creates mock objects.
- **`@InjectMocks`**: Injects mocks into the tested class.
- **`@Spy`**: Wraps a real object but allows stubbing.

**Example**:
```java
import org.mockito.Mock;
import org.mockito.InjectMocks;
import org.mockito.Spy;

class OrderServiceTest {
    @Mock PaymentService paymentService;
    @InjectMocks OrderService orderService;
    @Spy Logger logger;
}
```

---

## Stubbing: `when().thenReturn()`

- **Purpose**: Define behavior of mocks.
- **Example**:
```java
import static org.mockito.Mockito.*;

@Test
void shouldReturnStubbedValue() {
    PaymentService paymentService = mock(PaymentService.class);
    when(paymentService.process("123")).thenReturn("Success");

    assertEquals("Success", paymentService.process("123"));
}
```

---

## Exercise
- Create a `UserService` class with methods `registerUser`, `loginUser`, and `deleteUser`.
- Use Mockito to:
    - Mock a `DatabaseService`.
    - Stub behavior for `saveUser` and `findUser`.
    - Write parameterized tests for different login inputs.
    - Measure coverage with JaCoCo.

---
