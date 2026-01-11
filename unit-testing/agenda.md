
## Day 1: JUnit Fundamentals

### Unit Testing Principles
- Introduction to unit testing principles.

### JUnit Architecture and Maven/Gradle Setup
- How JUnit fits in the software development cycle.
- Setting up JUnit with Maven and Gradle.

### Basic `@Test`, Assertions (`assertEquals`, `assertTrue`, `assertThrows`)
- Writing basic test methods using the `@Test` annotation.
- Using assertions to verify test outcomes:
    - `assertEquals`
    - `assertTrue`
    - `assertThrows`

### Hands-on: First Test Class for Simple Methods
- Create a simple class and write unit tests for it.

### Lifecycle Annotations:
- `@BeforeEach`, `@AfterEach`, `@BeforeAll`, `@AfterAll`
    - Managing test setup and teardown.

### `@DisplayName`, `@Disabled`, Exception Testing
- Customizing test method names with `@DisplayName`.
- Skipping tests with `@Disabled`.
- Handling and testing exceptions.

### Exercises:
- Test a **Calculator** class covering:
    - Basic arithmetic operations.
    - Edge cases.

### Discussion:
- Test Naming Conventions for readability and maintainability.

---

## Day 2: Advanced JUnit & Mockito Basics

### Parameterized Tests (`@ParameterizedTest`), Dynamic Tests
- Running the same test with different inputs using `@ParameterizedTest`.
- Creating dynamic tests using `@TestFactory`.

### Nested Classes, Test Order Control
- Using nested test classes for better organization.
- Controlling test execution order.

### JaCoCo - Code Coverage
- Introduction to JaCoCo and code coverage.

### Mockito Setup (`@ExtendWith(MockitoExtension)`)
- Setting up Mockito in JUnit tests using `@ExtendWith(MockitoExtension)`.

### `@Mock`, `@InjectMocks`, `@Spy` Basics
- Understanding and using mock objects:
    - `@Mock` for mock objects.
    - `@InjectMocks` for injecting mocks into a class.
    - `@Spy` for partial mocking.

### Stubbing: `when().thenReturn()`
- Stubbing behavior of mocked objects.

### Exercise:
- Practice with Mockito-based tests.

---

## Day 3: Mockito Advanced & Best Practices

### Argument Matchers (`any()`, `eq()`)
- Using argument matchers to match parameters in stubbing and verification.

### Verification: `verify()`, `times()`, `atLeastOnce()`
- Verifying that interactions with mocks occurred as expected:
    - `verify()`
    - `times()`
    - `atLeastOnce()`

### Void Stubbing, Spying Real Objects, Mocked Static
- Stubbing void methods.
- Spying on real objects to allow partial mocking.
- Mocking static methods with Mockito.

### Hands-on: Multi-Dependency Service Tests
- Writing tests for services with multiple dependencies.

### Best Practices:
- Test Isolation: Keep tests independent.
- Avoid Over-Mocking: Do not mock everything.
- Test-Driven Development (TDD) principles.

### Common Pitfalls and Refactoring
- Reviewing common mistakes in unit testing and how to refactor tests.

### Coverage Review, Q&A, Quiz
- Review test coverage.
- Open Q&A session.
- A brief quiz to recap key learnings.
