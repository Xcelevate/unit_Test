# JUnit Day 1 Instructor Guide (AAA Style Solutions)

This guide provides both the **exercise prompt** and the **solution** for each of the 15 practice tasks. All solutions use **JUnit Jupiter 6.0.1** and follow the **Arrange–Act–Assert (AAA)** convention.

---

## 1. Addition Method
**Question:**
```java
class MathUtils {
    int add(int a, int b) { return a + b; }
}
```
**Answer:**
```java
@Test
void shouldReturnSumOfTwoNumbers() {
    // Arrange
    MathUtils mathUtils = new MathUtils();

    // Act
    int result = mathUtils.add(2, 3);

    // Assert
    assertEquals(5, result);
}
```

---

## 2. Division Method with Exception
**Question:**
```java
class MathUtils {
    int divide(int a, int b) { return a / b; }
}
```
**Answer:**
```java
@Test
void shouldThrowExceptionWhenDividingByZero() {
    // Arrange
    MathUtils mathUtils = new MathUtils();

    // Act & Assert
    assertThrows(ArithmeticException.class, () -> mathUtils.divide(10, 0));
}
```

---

## 3. String Length
**Question:**
```java
class StringHelper {
    int length(String s) { return s.length(); }
}
```
**Answer:**
```java
@Test
void shouldReturnLengthOfString() {
    // Arrange
    StringHelper helper = new StringHelper();

    // Act
    int result = helper.length("hello");

    // Assert
    assertEquals(5, result);
}
```

---

## 4. Palindrome Checker
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
@Test
void shouldReturnTrueForPalindrome() {
    // Arrange
    StringHelper helper = new StringHelper();

    // Act
    boolean result = helper.isPalindrome("madam");

    // Assert
    assertTrue(result);
}
```

---

## 5. Factorial Calculator
**Question:**
```java
class MathUtils {
    int factorial(int n) {
        if (n < 0) throw new IllegalArgumentException("Negative not allowed");
        return (n == 0) ? 1 : n * factorial(n-1);
    }
}
```
**Answer:**
```java
@Test
void shouldReturnFactorialOfFive() {
    // Arrange
    MathUtils mathUtils = new MathUtils();

    // Act
    int result = mathUtils.factorial(5);

    // Assert
    assertEquals(120, result);
}
```

---

## 6. Temperature Converter
**Question:**
```java
class Converter {
    double celsiusToFahrenheit(double c) { return (c * 9/5) + 32; }
}
```
**Answer:**
```java
@Test
void shouldConvertZeroCelsiusToFahrenheit() {
    // Arrange
    Converter converter = new Converter();

    // Act
    double result = converter.celsiusToFahrenheit(0);

    // Assert
    assertEquals(32, result);
}
```

---

## 7. Prime Number Checker
**Question:**
```java
class MathUtils {
    boolean isPrime(int n) {
        if (n <= 1) return false;
        for (int i=2; i<=Math.sqrt(n); i++) {
            if (n % i == 0) return false;
        }
        return true;
    }
}
```
**Answer:**
```java
@Test
void shouldReturnTrueForPrimeNumber() {
    // Arrange
    MathUtils mathUtils = new MathUtils();

    // Act
    boolean result = mathUtils.isPrime(7);

    // Assert
    assertTrue(result);
}
```

---

## 8. Greeting Generator
**Question:**
```java
class Greeter {
    String greet(String name) { return "Hello, " + name; }
}
```
**Answer:**
```java
@Test
void shouldReturnGreetingMessage() {
    // Arrange
    Greeter greeter = new Greeter();

    // Act
    String result = greeter.greet("John");

    // Assert
    assertEquals("Hello, John", result);
}
```

---

## 9. Array Sum
**Question:**
```java
class ArrayUtils {
    int sum(int[] arr) {
        int total = 0;
        for (int n : arr) total += n;
        return total;
    }
}
```
**Answer:**
```java
@Test
void shouldReturnSumOfArrayElements() {
    // Arrange
    ArrayUtils utils = new ArrayUtils();

    // Act
    int result = utils.sum(new int[]{1,2,3});

    // Assert
    assertEquals(6, result);
}
```

---

## 10. Max Finder
**Question:**
```java
class ArrayUtils {
    int max(int[] arr) {
        if (arr.length == 0) throw new IllegalArgumentException("Empty array");
        int max = arr[0];
        for (int n : arr) if (n > max) max = n;
        return max;
    }
}
```
**Answer:**
```java
@Test
void shouldReturnMaxValue() {
    // Arrange
    ArrayUtils utils = new ArrayUtils();

    // Act
    int result = utils.max(new int[]{1,2,3,4,5});

    // Assert
    assertEquals(5, result);
}
```

---

## 11. Email Validator
**Question:**
```java
class Validator {
    boolean isValidEmail(String email) {
        return email.contains("@") && email.contains(".");
    }
}
```
**Answer:**
```java
@Test
void shouldReturnTrueForValidEmail() {
    // Arrange
    Validator validator = new Validator();

    // Act
    boolean result = validator.isValidEmail("test@example.com");

    // Assert
    assertTrue(result);
}
```

---

## 12. Bank Account
**Question:**
```java
class BankAccount {
    private int balance = 0;
    void deposit(int amount) { balance += amount; }
    void withdraw(int amount) {
        if (amount > balance) throw new IllegalArgumentException("Insufficient funds");
        balance -= amount;
    }
    int getBalance() { return balance; }
}
```
**Answer:**
```java
@Test
void shouldUpdateBalanceAfterDeposit() {
    // Arrange
    BankAccount acc = new BankAccount();

    // Act
    acc.deposit(100);

    // Assert
    assertEquals(100, acc.getBalance());
}
```

---

## 13. Lifecycle Demo
**Question:**
```java
class Counter {
    private int count = 0;
    void increment() { count++; }
    int getCount() { return count; }
}
```
**Answer:**
```java
@BeforeEach
void setup() {
    // Arrange
    counter = new Counter();
}

@Test
void shouldIncrementCounter() {
    // Act
    counter.increment();

    // Assert
    assertEquals(1, counter.getCount());
}
```

---

## 14. Disabled Test Example
**Question:**
```java
class Feature {
    String upcomingFeature() { return "Not implemented"; }
}
```
**Answer:**
```java
@Disabled("Feature not ready")
@Test
void shouldSkipTestForUpcomingFeature() {
    // Arrange
    Feature feature = new Feature();

    // Act
    String result = feature.upcomingFeature();

    // Assert
    assertEquals("Implemented", result);
}
```

---

## 15. Calculator Class (Comprehensive)
**Question:**
```java
class Calculator {
    int add(int a, int b) { return a + b; }
    int subtract(int a, int b) { return a - b; }
    int multiply(int a, int b) { return a * b; }
    int divide(int a, int b) { return a / b; }
}
```
**Answer:**
```java
@Test
void shouldReturnCorrectAddition() {
    // Arrange
    Calculator calc = new Calculator();

    // Act
    int result = calc.add(2,3);

    // Assert
    assertEquals(5, result);
}
```

---
