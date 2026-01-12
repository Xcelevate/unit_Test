**Planning exercise generation**

Alright, so it looks like I’m supposed to generate 50 Day3 exercises with Java source code. I’ll include clear tasks with code snippets. I'll structure everything using markdown, keeping it neat and easy to follow. I don’t need to worry about citations or an instructor guide this time — just the code! I’ll make sure to keep the code blocks concise but still well-explained, making each exercise complete and self-contained. Let’s move forward!
# Day 3 exercises with source code to test

Below are 50 exercises organized by topic. Each exercise includes Java source code (class/method under test) and a clear testing task. Students should write JUnit Jupiter 6.0.1 tests, using parameterized tests, dynamic tests, nested classes, test ordering, JaCoCo coverage concepts, and Mockito where appropriate.

---

## Parameterized and dynamic tests (1–10)

### 1. Factorial with CsvFileSource
```java
class Factorial {
    int compute(int n) {
        if (n < 0) throw new IllegalArgumentException("negative");
        int result = 1;
        for (int i = 2; i <= n; i++) result *= i;
        return result;
    }
}
```
- Task: Use `@CsvFileSource` with rows like `0,1`, `1,1`, `5,120`, `7,5040` to verify `compute`.

### 2. String reversal with MethodSource
```java
class Strings {
    String reverse(String s) {
        if (s == null) throw new NullPointerException();
        return new StringBuilder(s).reverse().toString();
    }
}
```
- Task: Provide inputs via `@MethodSource` to test normal strings, empty string, and assert `NullPointerException` for null.

### 3. Dynamic tests for prime numbers
```java
class Primes {
    boolean isPrime(int n) {
        if (n <= 1) return false;
        for (int i = 2; i * i <= n; i++) {
            if (n % i == 0) return false;
        }
        return true;
    }
}
```
- Task: Create `@TestFactory` generating dynamic tests for a set [2,3,4,17,18,19] asserting expected primality.

### 4. Parameterized temperature conversion
```java
class TempConverter {
    double cToF(double c) { 
        return c * 9.0 / 5.0 + 32.0; 
    }
}
```
- Task: Use `@CsvSource` to test `cToF` with `(0,32)`, `(100,212)`, `(-40,-40)` including precision with `assertEquals(expected, actual, 0.0001)`.

### 5. Dynamic tests for division exceptions
```java
class Divider {
    int divide(int a, int b) { 
        return a / b; 
    }
}
```
- Task: Use `@TestFactory` to produce dynamic tests: valid divisions and division by zero using lambdas with `assertThrows`.

### 6. Parameterized palindrome detection
```java
class Palindrome {
    boolean isPalindrome(String s) {
        if (s == null) return false;
        String r = new StringBuilder(s).reverse().toString();
        return r.equalsIgnoreCase(s);
    }
}
```
- Task: Use `@ValueSource(strings = {"Madam","raceCar","not"})` asserting true for first two, false for the third.

### 7. CSV source for calculator operations
```java
class Calc {
    int add(int a, int b) {
        return a + b; 
    }
    
    int sub(int a, int b) { 
        return a - b; 
    }
    
    int mul(int a, int b) {
        return a * b; 
    }
    
    int div(int a, int b) {
        return a / b; 
    }
}
```
- Task: Use `@CsvSource` with tuples `(op,a,b,expected)` for `add,sub,mul,div`; branch by `op` inside test.

### 8. MethodSource for login validation
```java
class Auth {
    boolean login(String user, String pass) {
        if (user == null || pass == null) return false;
        return user.equals("admin") && pass.equals("secret");
    }
}
```
- Task: Supply `(user, pass, expected)` via `@MethodSource`, covering valid, invalid, nulls.

### 9. Dynamic tests for array boundaries
```java
class Arrays2 {
    int first(int[] arr) {
        if (arr == null || arr.length == 0) throw new IllegalArgumentException("empty");
        return arr[0];
    }
}
```
- Task: Generate dynamic tests for arrays: `[1]`, `[5,6]`, empty array (expect exception), and null (expect exception).

### 10. Parameterized string length
```java
class Strings2 {
    
    int len(String s) { 
        return s == null ? -1 : s.length(); 
    }
}
```
- Task: `@CsvSource` verifying lengths including null mapped via `ArgumentsAccessor` or `NullSource` if available; expect `-1` for null.

---

## Nested classes and order control (11–20)

### 11. Nested tests for UserService
```java
class UserService {
    boolean register(String username) { 
        return username != null && !username.isBlank(); 
    }
    boolean login(String username, String password) {
        return "admin".equals(username) && "secret".equals(password); 
    }
}
```
- Task: Use `@Nested` classes `RegistrationTests` and `LoginTests` with separate setups and assertions.

### 12. Nested tests for BankAccount
```java
class BankAccount {
    private int balance;
    void deposit(int amount) { 
        if (amount < 0) 
            throw new IllegalArgumentException(); balance += amount; 
    }
    
    void withdraw(int amount) {
        if (amount > balance) 
            throw new IllegalArgumentException("insufficient"); 
        balance -= amount; 
    }
    
    int getBalance() {
        return balance; 
    }
}
```
- Task: Nested `DepositTests` and `WithdrawTests`, cover happy paths and exceptions.

### 13. Deeply nested ShoppingCart
```java
import java.util.*;
class ShoppingCart {
    private final List<String> items = new ArrayList<>();
    void add(String item) {
        if (item == null || item.isBlank()) 
            throw new IllegalArgumentException(); 
        items.add(item); 
    }
    
    boolean remove(String item) {
        return items.remove(item); 
    }
    
    int size() {
        return items.size(); 
    }
    List<String> items() { return Collections.unmodifiableList(items); }
}
```
- Task: `@Nested` levels: `AddTests`, `RemoveTests`, and within `RemoveTests` a nested `WhenEmpty` testing behavior on empty cart.

### 14. Test order with Order
```java
class Counter {
    private int c = 0;
    
    void inc() { 
        c++; 
    }
    
    int value() {
        return c; 
    }
}
```
- Task: Use `@TestMethodOrder(OrderAnnotation.class)`, run `inc` test first, then assert value in a second test.

### 15. Alphabetical order execution
```java
class Alpha {
    
    String a() {
        return "A"; 
    }
    
    String b() {
        return "B"; 
    }
}
```
- Task: Use `@TestMethodOrder(MethodOrderer.Alphanumeric.class)` to show test names `testA`, `testB` execute in alpha order.

### 16. Nested lifecycle setup
```java
class Session {
    private boolean open = false;
    
    void open() {
        open = true; 
    }
    
    void close() {
        open = false; 
    }
    
    boolean isOpen() {
        return open; 
    }
}
```
- Task: In nested classes, use `@BeforeEach` to ensure a fresh `Session` per test; assert isOpen after `open`.

### 17. Nested teardown
```java
class Resource {
    private boolean released = false;
    
    void release() {
        released = true; 
    }
    
    boolean isReleased() { 
        return released; 
    }
}
```
- Task: Use `@AfterEach` inside nested class to `release` resource, and add a test that asserts released state in a follow-up check.

### 18. Nested tests for Calculator
```java
class Calculator2 {
    int add(int a, int b) { 
        return a + b; 
    }
    
    int divide(int a, int b) {
        return a / b; 
    }
}
```
- Task: Nested groups `AddTests` and `DivideTests` including exception test for divide by zero.

### 19. Order control for integration-like flow
```java
class LifecycleService {
    private boolean prepared = false;
    void prepare() {
        prepared = true; 
    }
    
    boolean run() { 
        return prepared; 
    }
}
```
- Task: Use `@Order` to run `prepare` before `run` test, demonstrating order dependency (and discuss why to avoid in real tests).

### 20. Nested tests for exception scenarios
```java
class NameParser {
    String first(String full) {
        if (full == null || !full.contains(" ")) 
            throw new IllegalArgumentException("bad full name");
        return full.split(" ")[0];
    }
}
```
- Task: Nested classes `ValidNames` and `InvalidNames` with targeted assertions and exception checks.

---

## JaCoCo coverage targets (21–30)

### 21. Coverage for Calculator
```java
class CoverageCalc {
    int add(int a, int b) { 
        return a + b; 
    }
    
    int sub(int a, int b) {
        return a - b; 
    }
    
    int mul(int a, int b) {
        return a * b; 
    }
    
    int div(int a, int b) { 
        if (b == 0) 
            throw new ArithmeticException(); 
        return a / b; 
    }
}
```
- Task: Write tests covering all methods and the divide-by-zero branch to reach 100% line and branch coverage.

### 22. Coverage for StringHelper
```java
class StringHelper2 {
    String safeUpper(String s) {
        if (s == null) 
            return "";
        return s.toUpperCase();
    }
}
```
- Task: Cover null and non-null inputs; assert outputs.

### 23. Coverage for Factorial (iterative)
```java
class Factorial2 {
    long compute(int n) {
        if (n < 0) throw new IllegalArgumentException();
        long r = 1;
        for (int i = 2; i <= n; i++) r *= i;
        return r;
    }
}
```
- Task: Cover zero, positive, and negative inputs.

### 24. Coverage for EmailValidator
```java
class EmailValidator {
    boolean isValid(String email) {
        if (email == null || email.isBlank()) return false;
        int at = email.indexOf('@');
        int dot = email.lastIndexOf('.');
        return at > 0 && dot > at + 1 && dot < email.length() - 1;
    }
}
```
- Task: Cover valid, missing `@`, missing dot, edge positions, blank/null.

### 25. Coverage for UserService
```java
class UserService2 {
    String role(String user) {
        if (user == null) 
            return "guest";
        return user.equals("admin") ? "admin" : "user";
    }
}
```
- Task: Cover all branches: null, admin, other.

### 26. Coverage for OrderService (branching)
```java
class OrderService {
    String status(boolean paid, boolean inStock) {
        if (!inStock) {
            return "out_of_stock";
        }
        
        if (!paid) {
            return "pending_payment";
        }
        return "confirmed";
    }
}
```
- Task: Achieve branch coverage for all outcomes.

### 27. Coverage for PaymentService
```java
class PaymentService2 {
    String process(int amount) {
        if (amount <= 0) {
            return "invalid";
        }
        
        if (amount < 100) {
            return "small";
        }
        return "ok";
    }
}
```
- Task: Cover invalid, small, ok paths.

### 28. Coverage for ShoppingCart
```java
class Cart2 {
    private int items = 0;
    void add() { 
        items++; 
    }
    
    boolean remove() { 
        if (items == 0) {
            return false;
        }
        items--; 
        return true; 
    }
    
    int count() { 
        return items; 
    }
}
```
- Task: Cover add, remove success/failure, count.

### 29. Coverage for BankAccount branches
```java
class BankAccount2 {
    private int balance;
    boolean deposit(int amt) { 
        if (amt <= 0) 
            return false; 
        balance += amt; 
        return true; 
    }
    
    boolean withdraw(int amt) { 
        if (amt <= 0 || amt > balance) 
            return false; 
        balance -= amt; return true; 
    }
    
    int balance() { 
        return balance; 
    }
}
```
- Task: Cover return true/false branches for deposit/withdraw.

### 30. Coverage for Logger (conditional)
```java
class Logger2 {
    String logLevel(int severity) {
        if (severity < 0) {
            throw new IllegalArgumentException();
        }
        
        if (severity == 0) {
            return "DEBUG";
        }

        if (severity <= 3) {
            return "INFO";
        }
        return "ERROR";
    }
}
```
- Task: Cover exception, DEBUG, INFO, ERROR.

---

## Mockito basics (31–40)

### 31. Mock PaymentService
```java
interface PaymentGateway { String charge(String id, int amount); }
class CheckoutService {
    private final PaymentGateway gateway;
    
    CheckoutService(PaymentGateway g) { 
        this.gateway = g; 
    }
    String checkout(String id, int amount) {
        return gateway.charge(id, amount); 
    }
}
```
- Task: Use `@Mock` for `PaymentGateway` and test `checkout` with stubbed return.

### 32. InjectMocks into OrderService
```java
interface Inventory { 
    boolean inStock(String sku); 
}
class OrderService3 {
    private final Inventory inv; 
    private final PaymentGateway gw;
    OrderService3(Inventory inv, PaymentGateway gw) {
        this.inv = inv; this.gw = gw; 
    }
    
    String place(String sku, String id, int amount) {
        if (!inv.inStock(sku)) 
            return "out_of_stock";
        return gw.charge(id, amount);
    }
}
```
- Task: Use `@Mock` for both dependencies and `@InjectMocks` for `OrderService3`. Stub inventory and payment behaviors.

### 33. Spy on Logger
```java
class AuditLogger {
    void write(String msg) {
        System.out.println(msg); 
    }
}

class AuditService {
    private final AuditLogger logger;
    AuditService(AuditLogger l) {
        this.logger = l; 
    }
    
    void record(String event) {
        logger.write("EVENT:" + event); 
    }
}
```
- Task: Use `@Spy` on `AuditLogger` and verify `write("EVENT:...")` was called.

### 34. Stub process with thenReturn
```java
interface EmailSender { 
    boolean send(String to, String body); 
}

class NotificationService {
    private final EmailSender sender;
    
    NotificationService(EmailSender s) {
        this.sender = s; 
    }
    
    boolean notifyUser(String to) {
        return sender.send(to, "Welcome"); 
    }
}
```
- Task: Stub `send` to return true; assert `notifyUser` returns true.

### 35. thenThrow on failure
```java
interface Storage { void save(String key, String value); }
class ConfigService {
    private final Storage storage;
    
    ConfigService(Storage s) {
        this.storage = s; 
    }
    
    boolean update(String key, String value) {
        storage.save(key, value);
        return true;
    }
}
```
- Task: Stub `storage.save` to throw `RuntimeException`; assert exception via `assertThrows`.

### 36. doReturn().when() for spies
```java
class MathLib {
    int square(int n) {
        return n * n; 
    }
    
    int sumSquares(int a, int b) {
        return square(a) + square(b); 
    }
}
```
- Task: Create a `@Spy` MathLib; use `doReturn(100).when(spy).square(10)`; assert `sumSquares(10,2)` reflects stubbed square.

### 37. Verify method calls
```java
interface Reporter { 
    void send(String msg); 
}

class ReportService {
    private final Reporter reporter;
    ReportService(Reporter r) {
        this.reporter = r;
    }
    
    void publish(String msg) {
        reporter.send(msg);
    }
}
```
- Task: Verify `send` invoked exactly once with expected message.

### 38. Verify call counts with times
```java
class RetryService {
    private final Reporter reporter;
    RetryService(Reporter r) {
        this.reporter = r;
    }
    
    void publishTwice(String msg) {
        reporter.send(msg); reporter.send(msg);
    }
}
```
- Task: Verify `send` called `times(2)`.

### 39. Verify never called
```java
class ConditionalService {
    private final Reporter reporter;
    ConditionalService(Reporter r) { 
        this.reporter = r; 
    }
    
    void maybeSend(String msg, boolean flag) {
        if (flag) {
            reporter.send(msg);
        }
    }
}
```
- Task: Verify `send` is `never()` called when flag is false.

### 40. Argument matchers
```java
class SearchService {
    private final Reporter reporter;
    SearchService(Reporter r) { 
        this.reporter = r; 
    }
    
    void search(String term) {
        reporter.send("search:" + term); 
    }
}
```
- Task: Use `eq("search:test")` and `anyString()` matchers with `verify`.

---

## Advanced Mockito (41–50)

### 41. ArgumentCaptor for UserRepository
```java
interface UserRepository { 
    void save(String username); 
}

class RegistrationService {
    private final UserRepository repo;
    RegistrationService(UserRepository r) {
        this.repo = r; 
    }
    
    void register(String username) {
        if (username == null) {
            throw new IllegalArgumentException();
        } 
        repo.save(username.trim()); 
    }
}
```
- Task: Use `ArgumentCaptor<String>` to assert trimmed username is saved.

### 42. Custom Answer dynamic stubbing
```java
interface Processor { String process(String id); }
class Pipeline {
    private final Processor p;
    Pipeline(Processor p) { this.p = p; }
    String run(String id) { return p.process(id); }
}
```
- Task: `when(p.process(anyString())).thenAnswer(inv -> "ok:" + inv.getArgument(0));` assert dynamic responses.

### 43. Stub multiple return values
```java
interface QueueReader { String read(); }
class QueueService {
    private final QueueReader reader;
    QueueService(QueueReader r) { this.reader = r; }
    String readTwice() { return reader.read() + "|" + reader.read(); }
}
```
- Task: `when(reader.read()).thenReturn("a","b");` assert `"a|b"`.

### 44. Sequential calls chain
```java
class FlakyService {
    private final Processor p;
    FlakyService(Processor p) { this.p = p; }
    String attempt(String id) { return p.process(id); }
}
```
- Task: Chain `thenThrow(new RuntimeException()).thenReturn("ok")` and assert first call throws, second returns.

### 45. InOrder verification
```java
interface Steps { void step1(); void step2(); }
class Flow {
    private final Steps steps;
    Flow(Steps s) { this.steps = s; }
    void run() { steps.step1(); steps.step2(); }
}
```
- Task: Use `InOrder` to verify `step1()` then `step2()`.

### 46. Partial mock with Spy
```java
class TextUtil {
    String upper(String s) { return s.toUpperCase(); }
    String shout(String s) { return upper(s) + "!"; }
}
```
- Task: Spy `TextUtil`, `doReturn("X").when(spy).upper("x")`, assert `shout("x")` equals `"X!"`.

### 47. Mix real and mock objects
```java
class PriceCalculator {
    double total(double price, double taxRate) { return price + price * taxRate; }
}
class Checkout {
    private final PriceCalculator calc; private final Reporter reporter;
    Checkout(PriceCalculator c, Reporter r) { this.calc = c; this.reporter = r; }
    double bill(double price) { double t = calc.total(price, 0.18); reporter.send("billed:" + t); return t; }
}
```
- Task: Use real `PriceCalculator`, mock `Reporter`, verify message and assert returned total.

### 48. Integration test with repository mock
```java
interface ItemRepo { boolean exists(String sku); }
class InventoryService {
    private final ItemRepo repo;
    InventoryService(ItemRepo r) { this.repo = r; }
    String check(String sku) { 
        return repo.exists(sku) ? "available" : "missing"; 
    }
}
```
- Task: Mock `ItemRepo` with both outcomes; assert `"available"` and `"missing"`.

### 49. Parameterized test with mocks
```java
class DiscountService {
    private final PriceCalculator calc;
    DiscountService(PriceCalculator c) { this.calc = c; }
    double discounted(double price, double discountPct) { return price * (1 - discountPct); }
}
```
- Task: Parameterize `(price,discountPct,expected)` via `@CsvSource`; optionally mock `PriceCalculator` if used in a variant method.

### 50. JaCoCo + Mockito coverage
```java
class AccountManager {
    private final BankAccount2 account = new BankAccount2();
    
    boolean openAndDeposit(int amt) {
        return account.deposit(amt); 
    }
    
    boolean tryWithdraw(int amt) {
        return account.withdraw(amt); 
    }
    
    int balance() { 
        return account.balance(); 
    }
}
```
- Task: Write tests covering success/failure paths to raise coverage; use Mockito only if refactoring to inject account dependency.

---

## Notes
- Use AAA comments (Arrange, Act, Assert) in all tests.
- For parameterized tests, balance data variety with readability; prefer `@CsvSource` for small sets, `@CsvFileSource` for larger datasets.
- For dynamic tests, demonstrate `DynamicTest.dynamicTest("name", executable)` and grouping via collections/streams.
- Use nested classes to model domains (e.g., registration vs login) and keep setups localized.
- For coverage, discuss line vs branch coverage and how exception paths must be explicitly exercised.
- In Mockito sections, emphasize clear contracts, minimal stubbing, and verification that matches behavior, not implementation details.

If you want, I can package this into a single Markdown file you can drop into your repo’s Day 3 folder.
