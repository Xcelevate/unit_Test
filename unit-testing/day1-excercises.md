# 📝 JUnit Practice Exercises (Day 1)

## 1. Addition Method
```java
class MathUtils {
    int add(int a, int b) {
        return a + b; 
    }
}
```
**Exercise**: Write tests for positive numbers, negative numbers, and zero.

---

## 2. Division Method with Exception
```java
class MathUtils {
    int divide(int a, int b) {
        return a / b; 
    }
}
```
**Exercise**: Test normal division and division by zero using `assertThrows`.

---

## 3. String Length
```java
class StringHelper {
    int length(String s) {
        return s.length(); 
    }
}
```
**Exercise**: Test with empty string, normal string, and null (expect `NullPointerException`).

---

## 4. Palindrome Checker
```java
class StringHelper {
    boolean isPalindrome(String s) {
        return new StringBuilder(s).reverse().toString().equals(s);
    }
}
```
**Exercise**: Test with `"madam"`, `"hello"`, and single‑character strings.

---

## 5. Factorial Calculator
```java
class MathUtils {
    int factorial(int n) {
        if (n < 0) throw new IllegalArgumentException("Negative not allowed");
        return (n == 0) ? 1 : n * factorial(n-1);
    }
}
```
**Exercise**: Test factorial of 0, 5, and negative input.

---

## 6. Temperature Converter
```java
class Converter {
    double celsiusToFahrenheit(double c) { return (c * 9/5) + 32; }
}
```
**Exercise**: Test conversion for 0°C, 100°C, and negative values.

---

## 7. Prime Number Checker
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
**Exercise**: Test with prime (7), non‑prime (8), and edge cases (0, 1).

---

## 8. Greeting Generator
```java
class Greeter {
    String greet(String name) { return "Hello, " + name; }
}
```
**Exercise**: Test with normal name, empty string, and null input.

---

## 9. Array Sum
```java
class ArrayUtils {
    int sum(int[] arr) {
        int total = 0;
        for (int n : arr) total += n;
        return total;
    }
}
```
**Exercise**: Test with empty array, positive numbers, and mixed numbers.

---

## 10. Max Finder
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
**Exercise**: Test with ascending array, descending array, and empty array.

---

## 11. Email Validator
```java
class Validator {
    boolean isValidEmail(String email) {
        return email.contains("@") && email.contains(".");
    }
}
```
**Exercise**: Test with valid email, invalid email, and empty string.

---

## 12. Bank Account
```java
class BankAccount {
    private int balance = 0;
    
    void deposit(int amount) {
        balance += amount; 
    }
    
    void withdraw(int amount) {
        if (amount > balance) throw new IllegalArgumentException("Insufficient funds");
        balance -= amount;
    }
    
    int getBalance() {
        return balance; 
    }
}
```
**Exercise**: Test deposit, withdraw, and exception for insufficient funds.

---

## 13. Lifecycle Demo
```java
class Counter {
    private int count = 0;
    
    void increment() {
        count++; 
    }
    
    int getCount() {
        return count; 
    }
}
```
**Exercise**: Use `@BeforeEach` to reset counter before each test, and verify increments.

---

## 14. Disabled Test Example
```java
class Feature {
    String upcomingFeature() { return "Not implemented"; }
}
```
**Exercise**: Write a test with `@Disabled("Feature not ready")`.

---

## 15. Calculator Class (Comprehensive)
```java
class Calculator {
    
    int add(int a, int b) {
        return a + b; 
    }
    
    int subtract(int a, int b) {
        return a - b; 
    }
    
    int multiply(int a, int b) {
        return a * b; 
    }
    
    int divide(int a, int b) {
        return a / b; 
    }
}
```
**Exercise**: Write tests for all operations, including edge cases (division by zero, negative numbers).

---

### ✅ Notes for Students
- Use `@Test`, `assertEquals`, `assertTrue`, `assertThrows`.
- Apply `@BeforeEach` and `@AfterEach` where setup/cleanup is needed.
- Use `@DisplayName` for readability.
- Follow naming conventions: `shouldReturnXWhenY`.
