# JUnit Day 2 Instructor Guide (AAA Style Solutions)

This guide provides both the **exercise prompt** and the **solution** for each of the 20 practice tasks. All solutions use **JUnit Jupiter 6.0.1** and follow the **Arrange–Act–Assert (AAA)** convention.

---

## 1. Palindrome Checker with ParameterizedTest
**Question:**
```java
class StringHelper {
    boolean isPalindrome(String s) {
        return new StringBuilder(s).reverse().toString().equals(s);
    }
}
```
**Answer:**
```java
@ParameterizedTest
@ValueSource(strings = {"madam", "racecar", "level"})
void shouldReturnTrueForPalindromes(String candidate) {
    // Arrange
    StringHelper helper = new StringHelper();

    // Act
    boolean result = helper.isPalindrome(candidate);

    // Assert
    assertTrue(result);
}
```

---

## 2. Even Number Checker
**Question:**
```java
class MathUtils {
    boolean isEven(int n) { return n % 2 == 0; }
}
```
**Answer:**
```java
@ParameterizedTest
@ValueSource(ints = {2, 4, 6})
void shouldReturnTrueForEvenNumbers(int number) {
    // Arrange
    MathUtils utils = new MathUtils();

    // Act
    boolean result = utils.isEven(number);

    // Assert
    assertTrue(result);
}
```

---

## 3. Dynamic Tests for Addition
**Question:**
```java
class Calculator {
    int add(int a, int b) { return a + b; }
}
```
**Answer:**
```java
@TestFactory
Collection<DynamicTest> dynamicTestsForAddition() {
    Calculator calc = new Calculator();
    return Arrays.asList(
        DynamicTest.dynamicTest("1+1=2", () -> assertEquals(2, calc.add(1,1))),
        DynamicTest.dynamicTest("2+3=5", () -> assertEquals(5, calc.add(2,3)))
    );
}
```

---

## 4. CSV Source for Multiplication
**Question:**
```java
class Calculator {
    int multiply(int a, int b) { return a * b; }
}
```
**Answer:**
```java
@ParameterizedTest
@CsvSource({"2,3,6", "4,5,20", "0,10,0"})
void shouldReturnCorrectMultiplication(int a, int b, int expected) {
    // Arrange
    Calculator calc = new Calculator();

    // Act
    int result = calc.multiply(a, b);

    // Assert
    assertEquals(expected, result);
}
```

---

## 5. MethodSource for String Length
**Question:**
```java
class StringHelper {
    int length(String s) { return s.length(); }
}
```
**Answer:**
```java
static Stream<Arguments> stringProvider() {
    return Stream.of(
        Arguments.of("hello", 5),
        Arguments.of("", 0)
    );
}

@ParameterizedTest
@MethodSource("stringProvider")
void shouldReturnCorrectLength(String input, int expected) {
    // Arrange
    StringHelper helper = new StringHelper();

    // Act
    int result = helper.length(input);

    // Assert
    assertEquals(expected, result);
}
```

---

## 6. UserService Nested Tests
**Answer:**
```java
@Nested
class RegistrationTests {
    @Test
    void shouldRegisterUser() {
        // Arrange
        UserService service = new UserService();

        // Act
        boolean result = service.register("user");

        // Assert
        assertTrue(result);
    }
}
```

---

## 7. BankAccount Ordered Tests
**Answer:**
```java
@TestMethodOrder(MethodOrderer.OrderAnnotation.class)
class BankAccountTest {
    BankAccount acc = new BankAccount();

    @Test @Order(1)
    void shouldDepositMoney() {
        // Arrange & Act
        acc.deposit(100);

        // Assert
        assertEquals(100, acc.getBalance());
    }

    @Test @Order(2)
    void shouldWithdrawMoney() {
        // Act
        acc.withdraw(50);

        // Assert
        assertEquals(50, acc.getBalance());
    }
}
```

---

## 11. Coverage for Calculator
**Answer:**
```java
@Test
void shouldCoverAllOperations() {
    // Arrange
    Calculator calc = new Calculator();

    // Act & Assert
    assertEquals(5, calc.add(2,3));
    assertEquals(1, calc.subtract(3,2));
    assertEquals(6, calc.multiply(2,3));
    assertEquals(2, calc.divide(4,2));
}
```

---

## 16. Mock PaymentService
**Answer:**
```java
@ExtendWith(MockitoExtension.class)
class OrderServiceTest {
    @Mock PaymentService paymentService;
    @InjectMocks OrderService orderService;

    @Test
    void shouldReturnSuccessWhenOrderPlaced() {
        // Arrange
        when(paymentService.process("123")).thenReturn("Success");

        // Act
        String result = orderService.placeOrder("123");

        // Assert
        assertEquals("Success", result);
    }
}
```

---

## 17. Spy Logger
**Answer:**
```java
@ExtendWith(MockitoExtension.class)
class ServiceTest {
    @Spy Logger logger;
    @InjectMocks Service service;

    @Test
    void shouldLogExecution() {
        // Act
        service.execute();

        // Assert
        verify(logger).log("executed");
    }
}
```

---

## 18. Stubbing with when().thenReturn()
**Answer:**
```java
@Test
void shouldStubPaymentService() {
    // Arrange
    PaymentService paymentService = mock(PaymentService.class);
    when(paymentService.process("123")).thenReturn("Success");

    // Act
    String result = paymentService.process("123");

    // Assert
    assertEquals("Success", result);
}
```
