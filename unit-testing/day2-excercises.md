# Day 2 Practice Exercises (20 Count)

## Parameterized & Dynamic Tests
1. **Palindrome Checker with ParameterizedTest**
   ```java
   class StringHelper {
       boolean isPalindrome(String s) {
           return new StringBuilder(s).reverse().toString().equals(s);
       }
   }
   ```  
   *Task*: Write a `@ParameterizedTest` with multiple palindrome inputs.

2. **Even Number Checker**
   ```java
   class MathUtils {
       boolean isEven(int n) { return n % 2 == 0; }
   }
   ```  
   *Task*: Use `@ValueSource` to test even and odd numbers.

3. **Dynamic Tests for Addition**
   ```java
   class Calculator {
       int add(int a, int b) { return a + b; }
   }
   ```  
   *Task*: Create `@TestFactory` dynamic tests for different addition cases.

4. **CSV Source for Multiplication**
   ```java
   class Calculator {
       int multiply(int a, int b) { return a * b; }
   }
   ```  
   *Task*: Use `@CsvSource` to test multiple multiplication inputs.

5. **MethodSource for String Length**
   ```java
   class StringHelper {
       int length(String s) { return s.length(); }
   }
   ```  
   *Task*: Provide test data via `@MethodSource`.

---

## Nested Classes & Test Order
6. **UserService Nested Tests**
   ```java
   class UserService {
       boolean register(String user) { return user != null; }
       boolean login(String user) { return "admin".equals(user); }
   }
   ```  
   *Task*: Organize tests into `@Nested` classes for registration and login.

7. **BankAccount Ordered Tests**
   ```java
   class BankAccount {
       private int balance = 0;
       void deposit(int amount) { balance += amount; }
       void withdraw(int amount) { balance -= amount; }
       int getBalance() { return balance; }
   }
   ```  
   *Task*: Use `@Order` to ensure deposit runs before withdraw.

8. **ShoppingCart Nested Tests**
   ```java
   class ShoppingCart {
       private List<String> items = new ArrayList<>();
       void addItem(String item) { items.add(item); }
       int size() { return items.size(); }
   }
   ```  
   *Task*: Create nested tests for adding and removing items.

9. **TestMethodOrder with Alphabetical Order**  
   *Task*: Demonstrate `@TestMethodOrder(MethodOrderer.Alphanumeric.class)`.

10. **Lifecycle with Nested Classes**  
    *Task*: Use `@BeforeEach` inside nested classes to reset state.

---

## JaCoCo Coverage
11. **Coverage for Calculator**  
    *Task*: Write tests to achieve 100% coverage for add, subtract, multiply, divide.

12. **Coverage for StringHelper**  
    *Task*: Ensure coverage includes null and empty cases.

13. **Coverage for Factorial**  
    *Task*: Cover positive, zero, and negative inputs.

14. **Coverage for EmailValidator**  
    *Task*: Cover valid, invalid, and empty email cases.

15. **Coverage for OrderService**  
    *Task*: Cover all branches of order processing logic.

---

## Mockito Basics
16. **Mock PaymentService**
   ```java
   interface PaymentService { String process(String id); }
   class OrderService {
       private PaymentService paymentService;
       OrderService(PaymentService ps) { this.paymentService = ps; }
       String placeOrder(String id) { return paymentService.process(id); }
   }
   ```  
*Task*: Use `@Mock` and `@InjectMocks` to test `placeOrder`.

17. **Spy Logger**
   ```java
   class Logger {
       void log(String msg) { System.out.println(msg); }
   }
   class Service {
       Logger logger = new Logger();
       void execute() { logger.log("executed"); }
   }
   ```  
*Task*: Use `@Spy` to verify logging.

18. **Stubbing with when().thenReturn()**  
    *Task*: Stub `paymentService.process("123")` to return `"Success"`.

19. **Multiple Stubs**  
    *Task*: Stub different inputs to return different outputs.

20. **InjectMocks with Dependency**
   ```java
   class UserRepository { String findUser(String id) { return "user"; } }
   class UserService {
       private UserRepository repo;
       UserService(UserRepository r) { this.repo = r; }
       String getUser(String id) { return repo.findUser(id); }
   }
   ```  
*Task*: Use `@Mock` and `@InjectMocks` to test `getUser`.

---
