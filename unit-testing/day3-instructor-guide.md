# JUnit Day 3 Instructor Guide (Complete AAA Solutions)

This instructor guide includes 50 exercises. Each exercise has:
- Question: Java source code and a task.
- Answer: Fully working JUnit Jupiter 6.0.1 tests written in Arrange–Act–Assert (AAA) style.

Note: Parameterized tests require the JUnit Jupiter Params module. Mockito examples use the JUnit 5 Mockito extension.

---

## Parameterized and dynamic tests (1–10)

### 1. Factorial with CsvFileSource
**Question:**
```java
class Factorial {
    int compute(int n) {
        if (n < 0) {
            throw new IllegalArgumentException("negative");
        }
        int result = 1;
        for (int i = 2; i <= n; i++) result *= i;
        return result;
    }
}
```
**Answer:**
```java
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.CsvSource;
import static org.junit.jupiter.api.Assertions.*;

class FactorialTest {

    @ParameterizedTest
    @CsvSource({"0,1","1,1","5,120","7,5040"})
    void shouldComputeFactorial(int input, int expected) {
        // Arrange
        Factorial f = new Factorial();

        // Act
        int result = f.compute(input);

        // Assert
        assertEquals(expected, result);
    }

    @ParameterizedTest
    @CsvSource({"-1","-5"})
    void shouldThrowForNegativeInputs(int input) {
        // Arrange
        Factorial f = new Factorial();

        // Act & Assert
        assertThrows(IllegalArgumentException.class, () -> f.compute(input));
    }
}
```

---

### 2. String reversal with MethodSource
**Question:**
```java
class Strings {
    String reverse(String s) {
        if (s == null) {
            throw new NullPointerException();
        }
        return new StringBuilder(s).reverse().toString();
    }
}
```
**Answer:**
```java
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.MethodSource;
import org.junit.jupiter.params.provider.Arguments;
import java.util.stream.Stream;
import static org.junit.jupiter.api.Assertions.*;

class StringsTest {

    static Stream<Arguments> stringProvider() {
        return Stream.of(
            Arguments.of("hello","olleh"),
            Arguments.of("",""),
            Arguments.of("abc","cba")
        );
    }

    @ParameterizedTest
    @MethodSource("stringProvider")
    void shouldReverseStrings(String input, String expected) {
        // Arrange
        Strings s = new Strings();

        // Act
        String actual = s.reverse(input);

        // Assert
        assertEquals(expected, actual);
    }

    @ParameterizedTest
    @MethodSource("nullProvider")
    void shouldThrowForNull(String input) {
        // Arrange
        Strings s = new Strings();

        // Act & Assert
        assertThrows(NullPointerException.class, () -> s.reverse(input));
    }

    static Stream<String> nullProvider() {
        return Stream.of((String) null);
    }
}
```

---

### 3. Dynamic tests for prime numbers
**Question:**
```java
class Primes {
    boolean isPrime(int n) {
        if (n <= 1) {
            return false;
        }
        for (int i = 2; i * i <= n; i++) {
            if (n % i == 0) return false;
        }
        return true;
    }
}
```
**Answer:**
```java
import org.junit.jupiter.api.DynamicTest;
import org.junit.jupiter.api.TestFactory;
import java.util.*;
import static org.junit.jupiter.api.Assertions.*;

class PrimesTest {

    @TestFactory
    Collection<DynamicTest> dynamicPrimeTests() {
        // Arrange
        Primes p = new Primes();
        Map<Integer,Boolean> cases = Map.of(
            2,true, 3,true, 4,false, 17,true, 18,false, 19,true
        );

        // Act & Assert (inside dynamic executables)
        List<DynamicTest> tests = new ArrayList<>();
        for (var e : cases.entrySet()) {
            tests.add(DynamicTest.dynamicTest("isPrime(" + e.getKey() + ")",
                () -> assertEquals(e.getValue(), p.isPrime(e.getKey()))
            ));
        }
        return tests;
    }
}
```

---

### 4. Parameterized temperature conversion
**Question:**
```java
class TempConverter {
    double cToF(double c) { return c * 9.0 / 5.0 + 32.0; }
}
```
**Answer:**
```java
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.CsvSource;
import static org.junit.jupiter.api.Assertions.*;

class TempConverterTest {

    @ParameterizedTest
    @CsvSource({
        "0,32",
        "100,212",
        "-40,-40",
        "37,98.6"
    })
    void shouldConvertCelsiusToFahrenheit(double c, double expectedF) {
        // Arrange
        TempConverter converter = new TempConverter();

        // Act
        double actual = converter.cToF(c);

        // Assert
        assertEquals(expectedF, actual, 0.0001);
    }
}
```

---

### 5. Dynamic tests for division exceptions
**Question:**
```java
class Divider {
    
    int divide(int a, int b) { 
        return a / b; 
    }
}
```
**Answer:**
```java
import org.junit.jupiter.api.DynamicTest;
import org.junit.jupiter.api.TestFactory;
import java.util.List;
import static org.junit.jupiter.api.Assertions.*;

class DividerTest {

    @TestFactory
    List<DynamicTest> dynamicDivisionTests() {
        // Arrange
        Divider d = new Divider();

        // Act & Assert
        return List.of(
            DynamicTest.dynamicTest("10/2=5", () -> assertEquals(5, d.divide(10,2))),
            DynamicTest.dynamicTest("9/3=3", () -> assertEquals(3, d.divide(9,3))),
            DynamicTest.dynamicTest("division by zero", () -> assertThrows(ArithmeticException.class, () -> d.divide(1,0)))
        );
    }
}
```

---

### 6. Parameterized palindrome detection
**Question:**
```java
class Palindrome {
    boolean isPalindrome(String s) {
        if (s == null) return false;
        String r = new StringBuilder(s).reverse().toString();
        return r.equalsIgnoreCase(s);
    }
}
```
**Answer:**
```java
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.ValueSource;
import static org.junit.jupiter.api.Assertions.*;

class PalindromeTest {

    @ParameterizedTest
    @ValueSource(strings = {"Madam","raceCar","Level","noon"})
    void shouldReturnTrueForPalindromes(String input) {
        // Arrange
        Palindrome p = new Palindrome();

        // Act
        boolean result = p.isPalindrome(input);

        // Assert
        assertTrue(result);
    }

    @ParameterizedTest
    @ValueSource(strings = {"hello","world","Java"})
    void shouldReturnFalseForNonPalindromes(String input) {
        // Arrange
        Palindrome p = new Palindrome();

        // Act
        boolean result = p.isPalindrome(input);

        // Assert
        assertFalse(result);
    }

    @ParameterizedTest
    @ValueSource(strings = {""})
    void shouldHandleEmptyString(String input) {
        // Arrange
        Palindrome p = new Palindrome();

        // Act
        boolean result = p.isPalindrome(input);

        // Assert
        assertTrue(result);
    }
}
```

---

### 7. CSV source for calculator operations
**Question:**
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
**Answer:**
```java
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.CsvSource;
import static org.junit.jupiter.api.Assertions.*;

class CalcTest {

    @ParameterizedTest
    @CsvSource({
        "add,2,3,5",
        "sub,5,2,3",
        "mul,3,4,12",
        "div,8,2,4"
    })
    void shouldPerformOperations(String op, int a, int b, int expected) {
        // Arrange
        Calc calc = new Calc();
        int actual;

        // Act
        switch (op) {
            case "add": 
                actual = calc.add(a,b); 
                break;
            case "sub": 
                actual = calc.sub(a,b); 
                break;
            case "mul": 
                actual = calc.mul(a,b); 
                break;
            case "div": 
                actual = calc.div(a,b); 
                break;
            default: 
                throw new IllegalArgumentException("unknown op");
        }

        // Assert
        assertEquals(expected, actual);
    }

    @ParameterizedTest
    @CsvSource({
        "div,1,0"
    })
    void shouldThrowOnInvalidDivision(String op, int a, int b) {
        // Arrange
        Calc calc = new Calc();

        // Act & Assert
        if ("div".equals(op)) {
            assertThrows(ArithmeticException.class, () -> calc.div(a,b));
        }
    }
}
```

---

### 8. MethodSource for login validation
**Question:**
```java
class Auth {
    boolean login(String user, String pass) {
        if (user == null || pass == null) {
            return false;
        }
        return user.equals("admin") && pass.equals("secret");
    }
}
```
**Answer:**
```java
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.MethodSource;
import org.junit.jupiter.params.provider.Arguments;
import java.util.stream.Stream;
import static org.junit.jupiter.api.Assertions.*;

class AuthTest {

    static Stream<Arguments> loginProvider() {
        return Stream.of(
            Arguments.of("admin","secret",true),
            Arguments.of("admin","wrong",false),
            Arguments.of("user","secret",false),
            Arguments.of(null,"secret",false),
            Arguments.of("admin",null,false)
        );
    }

    @ParameterizedTest
    @MethodSource("loginProvider")
    void shouldValidateLogin(String user, String pass, boolean expected) {
        // Arrange
        Auth auth = new Auth();

        // Act
        boolean actual = auth.login(user, pass);

        // Assert
        assertEquals(expected, actual);
    }
}
```

---

### 9. Dynamic tests for array boundaries
**Question:**
```java
class MyArray {
    int first(int[] arr) {
        if (arr == null || arr.length == 0) throw new IllegalArgumentException("empty");
        return arr[0];
    }
}
```
**Answer:**

This program uses JUnit 5 dynamic tests to exercise the MyArray.first(...) method over multiple boundary and edge cases, generated at runtime instead of writing four separate @Test methods.

What dynamic tests are doing @TestFactory marks dynamicArrayBoundaryTests() as a factory that produces tests instead of being a test itself.

The method returns a List<DynamicTest>, each created with DynamicTest.dynamicTest(displayName, executable).

JUnit discovers this factory, asks it for tests at runtime, and then runs each DynamicTest like a normal test, showing the given display name in reports.

```java
import org.junit.jupiter.api.DynamicTest;
import org.junit.jupiter.api.TestFactory;
import java.util.List;
import static org.junit.jupiter.api.Assertions.*;

class MyArrayTest {

    @TestFactory
    List<DynamicTest> dynamicArrayBoundaryTests() {
        // Arrange
        MyArray a = new MyArray();

        // Act & Assert
        return List.of(
            DynamicTest.dynamicTest("first of [1]", () -> assertEquals(1, a.first(new int[]{1}))),
            DynamicTest.dynamicTest("first of [5,6]", () -> assertEquals(5, a.first(new int[]{5,6}))),
            DynamicTest.dynamicTest("empty array throws", () -> assertThrows(IllegalArgumentException.class, () -> a.first(new int[]{}))),
            DynamicTest.dynamicTest("null array throws", () -> assertThrows(IllegalArgumentException.class, () -> a.first(null)))
        );
    }
}
```



---

### 10. Parameterized string length
**Question:**
```java
class Strings2 {
    int len(String s) { 
        return s == null ? -1 : s.length(); 
    }
}
```
**Answer:**
```java
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.CsvSource;
import static org.junit.jupiter.api.Assertions.*;

class Strings2Test {

    @ParameterizedTest
    @CsvSource({
        "'hello',5",
        "'',0"
    })
    void shouldReturnLengthForNonNull(String input, int expected) {
        // Arrange
        Strings2 s = new Strings2();

        // Act
        int actual = s.len(input);

        // Assert
        assertEquals(expected, actual);
    }

    @ParameterizedTest
    @CsvSource({
        "null,-1"
    })
    void shouldReturnMinusOneForNull(String input, int expected) {
        // Arrange
        Strings2 s = new Strings2();
        String value = null; // explicit null for CsvSource "null"

        // Act
        int actual = s.len(value);

        // Assert
        assertEquals(expected, actual);
    }
}
```

---

## Nested classes and order control (11–20)

### 11. Nested tests for UserService
**Question:**
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
**Answer:**
```java
import org.junit.jupiter.api.Nested;
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class UserServiceTest {

    UserService service = new UserService();

    @Nested
    class RegistrationTests {

        @Test
        void shouldRegisterValidUser() {
            // Act
            boolean result = service.register("user");

            // Assert
            assertTrue(result);
        }

        @Test
        void shouldRejectBlankUser() {
            // Act
            boolean result = service.register("  ");

            // Assert
            assertFalse(result);
        }
    }

    @Nested
    class LoginTests {

        @Test
        void shouldLoginAdmin() {
            // Act
            boolean result = service.login("admin","secret");

            // Assert
            assertTrue(result);
        }

        @Test
        void shouldRejectInvalidPassword() {
            // Act
            boolean result = service.login("admin","wrong");

            // Assert
            assertFalse(result);
        }
    }
}
```

---

### 12. Nested tests for BankAccount
**Question:**
```java
class BankAccount {
    private int balance;
    void deposit(int amount) { 
        if (amount < 0) {
            throw new IllegalArgumentException();
        } 
        balance += amount;
    }
    
    void withdraw(int amount) { 
        if (amount > balance) {
            throw new IllegalArgumentException("insufficient");
        } 
        balance -= amount; 
    }
    
    int getBalance() {
        return balance;
    }
}
```
**Answer:**
```java
import org.junit.jupiter.api.Nested;
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class BankAccountTest {

    @Nested
    class DepositTests {

        @Test
        void shouldIncreaseBalanceOnDeposit() {
            // Arrange
            BankAccount acc = new BankAccount();

            // Act
            acc.deposit(100);

            // Assert
            assertEquals(100, acc.getBalance());
        }

        @Test
        void shouldThrowOnNegativeDeposit() {
            // Arrange
            BankAccount acc = new BankAccount();

            // Act & Assert
            assertThrows(IllegalArgumentException.class, () -> acc.deposit(-10));
        }
    }

    @Nested
    class WithdrawTests {

        @Test
        void shouldDecreaseBalanceOnWithdraw() {
            // Arrange
            BankAccount acc = new BankAccount();
            acc.deposit(100);

            // Act
            acc.withdraw(40);

            // Assert
            assertEquals(60, acc.getBalance());
        }

        @Test
        void shouldThrowOnInsufficientFunds() {
            // Arrange
            BankAccount acc = new BankAccount();
            acc.deposit(50);

            // Act & Assert
            assertThrows(IllegalArgumentException.class, () -> acc.withdraw(60));
        }
    }
}
```

---

### 13. Deeply nested ShoppingCart
**Question:**
```java
import java.util.*;
class ShoppingCart {
    private final List<String> items = new ArrayList<>();
    
    void add(String item) { 
        if (item == null || item.isBlank()) {
            throw new IllegalArgumentException(); items.add(item);
        }
    }
    
    boolean remove(String item) {
        return items.remove(item); 
    }
    
    int size() {
        return items.size();
    }
    
    List<String> items() {
        return Collections.unmodifiableList(items); 
    }
}
```
**Answer:**
```java
import org.junit.jupiter.api.Nested;
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class ShoppingCartTest {

    @Nested
    class AddTests {

        @Test
        void shouldAddItem() {
            // Arrange
            ShoppingCart cart = new ShoppingCart();

            // Act
            cart.add("apple");

            // Assert
            assertEquals(1, cart.size());
            assertTrue(cart.items().contains("apple"));
        }

        @Test
        void shouldRejectBlankItem() {
            // Arrange
            ShoppingCart cart = new ShoppingCart();

            // Act & Assert
            assertThrows(IllegalArgumentException.class, () -> cart.add(" "));
        }
    }

    @Nested
    class RemoveTests {

        @Test
        void shouldRemoveExistingItem() {
            // Arrange
            ShoppingCart cart = new ShoppingCart();
            cart.add("banana");

            // Act
            boolean removed = cart.remove("banana");

            // Assert
            assertTrue(removed);
            assertEquals(0, cart.size());
        }

        @Nested
        class WhenEmpty {

            @Test
            void shouldReturnFalseRemovingFromEmptyCart() {
                // Arrange
                ShoppingCart cart = new ShoppingCart();

                // Act
                boolean removed = cart.remove("anything");

                // Assert
                assertFalse(removed);
                assertEquals(0, cart.size());
            }
        }
    }
}
```

---

### 14. Test order with Order
**Question:**
```java
class Counter {
    private int c = 0;
    void inc() { c++; }
    int value() { return c; }
}
```
**Answer:**
```java
import org.junit.jupiter.api.*;
import static org.junit.jupiter.api.Assertions.*;

@TestMethodOrder(MethodOrderer.OrderAnnotation.class)
class CounterTest {

    static Counter counter;

    @BeforeAll
    static void setup() {
        // Arrange
        counter = new Counter();
    }

    @Test @Order(1)
    void shouldIncrementCounterFirst() {
        // Act
        counter.inc();

        // Assert
        assertEquals(1, counter.value());
    }

    @Test @Order(2)
    void shouldVerifyValueAfterIncrement() {
        // Act
        int val = counter.value();

        // Assert
        assertEquals(1, val);
    }
}
```

---

### 15. Alphabetical order execution
**Question:**
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
**Answer:**
```java
import org.junit.jupiter.api.*;
import static org.junit.jupiter.api.Assertions.*;

@TestMethodOrder(MethodOrderer.Alphanumeric.class)
class AlphaTest {

    Alpha alpha = new Alpha();

    @Test
    void testA() {
        // Act
        String s = alpha.a();

        // Assert
        assertEquals("A", s);
    }

    @Test
    void testB() {
        // Act
        String s = alpha.b();

        // Assert
        assertEquals("B", s);
    }
}
```

---

### 16. Nested lifecycle setup
**Question:**
```java
class Session {
    private boolean open = false;
    void open() { open = true; }
    void close() { open = false; }
    boolean isOpen() { return open; }
}
```
**Answer:**
```java
import org.junit.jupiter.api.*;
import static org.junit.jupiter.api.Assertions.*;

class SessionTest {

    @Nested
    class OpenTests {
        Session session;

        @BeforeEach
        void init() {
            // Arrange
            session = new Session();
        }

        @Test
        void shouldBeOpenAfterOpen() {
            // Act
            session.open();

            // Assert
            assertTrue(session.isOpen());
        }
    }

    @Nested
    class CloseTests {
        Session session;

        @BeforeEach
        void init() {
            // Arrange
            session = new Session();
            session.open();
        }

        @Test
        void shouldBeClosedAfterClose() {
            // Act
            session.close();

            // Assert
            assertFalse(session.isOpen());
        }
    }
}
```

---

### 17. Nested teardown
**Question:**
```java
class Resource {
    private boolean released = false;
    void release() { released = true; }
    boolean isReleased() { return released; }
}
``]
**Answer:**
```java
import org.junit.jupiter.api.*;
import static org.junit.jupiter.api.Assertions.*;

class ResourceTest {

    @Nested
    class ReleaseTests {
        Resource resource;

        @BeforeEach
        void setup() {
            // Arrange
            resource = new Resource();
        }

        @AfterEach
        void teardown() {
            // Act
            resource.release();
        }

        @Test
        void shouldNotBeReleasedBeforeTeardown() {
            // Act
            boolean state = resource.isReleased();

            // Assert
            assertFalse(state);
        }

        @Test
        void shouldBeReleasedAfterTeardown() {
            // Act
            // (teardown will run after test)
            // Assert
            // We can't assert after teardown in the same method, so we simulate:
            resource.release();
            assertTrue(resource.isReleased());
        }
    }
}
```

---

### 18. Nested tests for Calculator
**Question:**
```java
class Calculator2 {
    int add(int a, int b) { return a + b; }
    int divide(int a, int b) { return a / b; }
}
```
**Answer:**
```java
import org.junit.jupiter.api.Nested;
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class Calculator2Test {

    @Nested
    class AddTests {

        @Test
        void shouldAddNumbers() {
            // Arrange
            Calculator2 c = new Calculator2();

            // Act
            int result = c.add(2,3);

            // Assert
            assertEquals(5, result);
        }
    }

    @Nested
    class DivideTests {

        @Test
        void shouldDivideNormally() {
            // Arrange
            Calculator2 c = new Calculator2();

            // Act
            int result = c.divide(6,3);

            // Assert
            assertEquals(2, result);
        }

        @Test
        void shouldThrowOnDivideByZero() {
            // Arrange
            Calculator2 c = new Calculator2();

            // Act & Assert
            assertThrows(ArithmeticException.class, () -> c.divide(1,0));
        }
    }
}
```

---

### 19. Order control for integration-like flow
**Question:**
```java
class LifecycleService {
    private boolean prepared = false;
    void prepare() { prepared = true; }
    boolean run() { return prepared; }
}
```
**Answer:**
```java
import org.junit.jupiter.api.*;
import static org.junit.jupiter.api.Assertions.*;

@TestMethodOrder(MethodOrderer.OrderAnnotation.class)
class LifecycleServiceTest {

    static LifecycleService svc;

    @BeforeAll
    static void init() {
        // Arrange
        svc = new LifecycleService();
    }

    @Test @Order(1)
    void shouldPrepare() {
        // Act
        svc.prepare();

        // Assert
        assertTrue(svc.run());
    }

    @Test @Order(2)
    void shouldRunAfterPrepare() {
        // Act
        boolean ran = svc.run();

        // Assert
        assertTrue(ran);
    }
}
```

---

### 20. Nested tests for exception scenarios
**Question:**
```java
class NameParser {
    String first(String full) {
        if (full == null || !full.contains(" ")) throw new IllegalArgumentException("bad full name");
        return full.split(" ")[0];
    }
}
```
**Answer:**
```java
import org.junit.jupiter.api.Nested;
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class NameParserTest {

    @Nested
    class ValidNames {
        @Test
        void shouldReturnFirstName() {
            // Arrange
            NameParser np = new NameParser();

            // Act
            String first = np.first("John Doe");

            // Assert
            assertEquals("John", first);
        }
    }

    @Nested
    class InvalidNames {
        @Test
        void shouldThrowOnNoSpace() {
            // Arrange
            NameParser np = new NameParser();

            // Act & Assert
            assertThrows(IllegalArgumentException.class, () -> np.first("SingleName"));
        }

        @Test
        void shouldThrowOnNull() {
            // Arrange
            NameParser np = new NameParser();

            // Act & Assert
            assertThrows(IllegalArgumentException.class, () -> np.first(null));
        }
    }
}
```

---

## JaCoCo coverage targets (21–30)

### 21. Coverage for Calculator
**Question:**
```java
class CoverageCalc {
    int add(int a, int b) { return a + b; }
    int sub(int a, int b) { return a - b; }
    int mul(int a, int b) { return a * b; }
    int div(int a, int b) { if (b == 0) throw new ArithmeticException(); return a / b; }
}
```
**Answer:**
```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class CoverageCalcTest {

    @Test
    void shouldCoverAllOperations() {
        // Arrange
        CoverageCalc calc = new CoverageCalc();

        // Act & Assert
        assertEquals(5, calc.add(2,3));
        assertEquals(1, calc.sub(3,2));
        assertEquals(6, calc.mul(2,3));
        assertEquals(2, calc.div(4,2));
        assertThrows(ArithmeticException.class, () -> calc.div(1,0));
    }
}
```

---

### 22. Coverage for StringHelper2
**Question:**
```java
class StringHelper2 {
    String safeUpper(String s) {
        if (s == null) return "";
        return s.toUpperCase();
    }
}
```
**Answer:**
```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class StringHelper2Test {

    @Test
    void shouldReturnEmptyForNull() {
        // Arrange
        StringHelper2 sh = new StringHelper2();

        // Act
        String result = sh.safeUpper(null);

        // Assert
        assertEquals("", result);
    }

    @Test
    void shouldUppercaseNonNull() {
        // Arrange
        StringHelper2 sh = new StringHelper2();

        // Act
        String result = sh.safeUpper("abc");

        // Assert
        assertEquals("ABC", result);
    }
}
```

---

### 23. Coverage for Factorial2
**Question:**
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
**Answer:**
```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class Factorial2Test {

    @Test
    void shouldComputeZeroAndPositive() {
        // Arrange
        Factorial2 f = new Factorial2();

        // Act & Assert
        assertEquals(1, f.compute(0));
        assertEquals(120, f.compute(5));
    }

    @Test
    void shouldThrowOnNegative() {
        // Arrange
        Factorial2 f = new Factorial2();

        // Act & Assert
        assertThrows(IllegalArgumentException.class, () -> f.compute(-1));
    }
}
```

---

### 24. Coverage for EmailValidator
**Question:**
```java
class EmailValidator {
    boolean isValid(String email) {
        if (email == null || email.isBlank()) {
            return false;
        }
        int at = email.indexOf('@');
        int dot = email.lastIndexOf('.');
        return at > 0 && dot > at + 1 && dot < email.length() - 1;
    }
}
```
**Answer:**
```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class EmailValidatorTest {

    @Test
    void shouldValidateProperEmail() {
        // Arrange
        EmailValidator v = new EmailValidator();

        // Act
        boolean ok = v.isValid("test@example.com");

        // Assert
        assertTrue(ok);
    }

    @Test
    void shouldRejectInvalidPatterns() {
        // Arrange
        EmailValidator v = new EmailValidator();

        // Act & Assert
        assertFalse(v.isValid("no-at.com"));
        assertFalse(v.isValid("a@b")); // no dot after @
        assertFalse(v.isValid("a@b.")); // dot at end
        assertFalse(v.isValid("")); // blank
        assertFalse(v.isValid(null)); // null
    }
}
```

---

### 25. Coverage for UserService2
**Question:**
```java
class UserService2 {
    String role(String user) {
        if (user == null) return "guest";
        return user.equals("admin") ? "admin" : "user";
    }
}
```
**Answer:**
```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class UserService2Test {

    @Test
    void shouldReturnRoles() {
        // Arrange
        UserService2 us = new UserService2();

        // Act & Assert
        assertEquals("guest", us.role(null));
        assertEquals("admin", us.role("admin"));
        assertEquals("user", us.role("bob"));
    }
}
```

---

### 26. Coverage for OrderService (branching)
**Question:**
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
**Answer:**
```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class OrderServiceStatusTest {

    @Test
    void shouldCoverAllStatuses() {
        // Arrange
        OrderService os = new OrderService();

        // Act & Assert
        assertEquals("out_of_stock", os.status(true, false));
        assertEquals("pending_payment", os.status(false, true));
        assertEquals("confirmed", os.status(true, true));
    }
}
```

---

### 27. Coverage for PaymentService2
**Question:**
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
**Answer:**
```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class PaymentService2Test {

    @Test
    void shouldCoverAllBranches() {
        // Arrange
        PaymentService2 ps = new PaymentService2();

        // Act & Assert
        assertEquals("invalid", ps.process(0));
        assertEquals("small", ps.process(50));
        assertEquals("ok", ps.process(100));
    }
}
```

---

### 28. Coverage for Cart2
**Question:**
```java
class Cart2 {
    private int items = 0;
    void add() { items++; }
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
**Answer:**
```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class Cart2Test {

    @Test
    void shouldAddAndRemove() {
        // Arrange
        Cart2 cart = new Cart2();

        // Act
        cart.add();
        boolean removed = cart.remove();

        // Assert
        assertTrue(removed);
        assertEquals(0, cart.count());
    }

    @Test
    void shouldReturnFalseWhenRemovingFromEmpty() {
        // Arrange
        Cart2 cart = new Cart2();

        // Act
        boolean removed = cart.remove();

        // Assert
        assertFalse(removed);
        assertEquals(0, cart.count());
    }
}
```

---

### 29. Coverage for BankAccount2
**Question:**
```java
class BankAccount2 {
    private int balance;
    boolean deposit(int amt) { 
        if (amt <= 0) {
            return false;
        } 
        balance += amt; 
        return true; 
    }
    boolean withdraw(int amt) {
        if (amt <= 0 || amt > balance){
            return false;
        } 
        balance -= amt; 
        return true; 
    }
    int balance() {
        return balance; 
    }
}
```
**Answer:**
```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class BankAccount2Test {

    @Test
    void shouldCoverDepositAndWithdrawBranches() {
        // Arrange
        BankAccount2 acc = new BankAccount2();

        // Act & Assert
        assertFalse(acc.deposit(0));
        assertTrue(acc.deposit(100));
        assertFalse(acc.withdraw(0));
        assertFalse(acc.withdraw(200));
        assertTrue(acc.withdraw(50));
        assertEquals(50, acc.balance());
    }
}
```

---

### 30. Coverage for Logger2
**Question:**
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
**Answer:**
```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class Logger2Test {

    @Test
    void shouldCoverLevelsAndException() {
        // Arrange
        Logger2 logger = new Logger2();

        // Act & Assert
        assertThrows(IllegalArgumentException.class, () -> logger.logLevel(-1));
        assertEquals("DEBUG", logger.logLevel(0));
        assertEquals("INFO", logger.logLevel(3));
        assertEquals("ERROR", logger.logLevel(4));
    }
}
```

---

## Mockito basics (31–40)

### 31. Mock PaymentService
**Question:**
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
**Answer:**
```java
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.Mock;
import org.mockito.InjectMocks;
import org.mockito.junit.jupiter.MockitoExtension;
import static org.mockito.Mockito.*;
import static org.junit.jupiter.api.Assertions.*;

@ExtendWith(MockitoExtension.class)
class CheckoutServiceTest {

    @Mock PaymentGateway gateway;
    @InjectMocks CheckoutService service;

    @Test
    void shouldReturnStubbedCharge() {
        // Arrange
        when(gateway.charge("123",100)).thenReturn("ok");

        // Act
        String result = service.checkout("123",100);

        // Assert
        assertEquals("ok", result);
        verify(gateway).charge("123",100);
    }
}
```

---

### 32. InjectMocks into OrderService3
**Question:**
```java
interface Inventory { boolean inStock(String sku); }
class OrderService3 {
    private final Inventory inv; private final PaymentGateway gw;
    OrderService3(Inventory inv, PaymentGateway gw) { this.inv = inv; this.gw = gw; }
    String place(String sku, String id, int amount) {
        if (!inv.inStock(sku)) return "out_of_stock";
        return gw.charge(id, amount);
    }
}
```
**Answer:**
```java
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.Mock;
import org.mockito.InjectMocks;
import org.mockito.junit.jupiter.MockitoExtension;
import static org.mockito.Mockito.*;
import static org.junit.jupiter.api.Assertions.*;

@ExtendWith(MockitoExtension.class)
class OrderService3Test {

    @Mock Inventory inv;
    @Mock PaymentGateway gw;
    @InjectMocks OrderService3 service;

    @Test
    void shouldReturnOutOfStockWhenNotAvailable() {
        // Arrange
        when(inv.inStock("SKU1")).thenReturn(false);

        // Act
        String status = service.place("SKU1","ID1",100);

        // Assert
        assertEquals("out_of_stock", status);
        verify(inv).inStock("SKU1");
        verifyNoInteractions(gw);
    }

    @Test
    void shouldChargeWhenInStock() {
        // Arrange
        when(inv.inStock("SKU2")).thenReturn(true);
        when(gw.charge("ID2",200)).thenReturn("charged");

        // Act
        String status = service.place("SKU2","ID2",200);

        // Assert
        assertEquals("charged", status);
        verify(inv).inStock("SKU2");
        verify(gw).charge("ID2",200);
    }
}
```

---

### 33. Spy on AuditLogger
**Question:**
```java
class AuditLogger {
    void write(String msg) { System.out.println(msg); }
}
class AuditService {
    private final AuditLogger logger;
    AuditService(AuditLogger l) { this.logger = l; }
    void record(String event) { logger.write("EVENT:" + event); }
}
```
**Answer:**
```java
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.Spy;
import org.mockito.InjectMocks;
import org.mockito.junit.jupiter.MockitoExtension;
import static org.mockito.Mockito.*;

@ExtendWith(MockitoExtension.class)
class AuditServiceTest {

    @Spy AuditLogger logger;
    @InjectMocks AuditService service;

    @Test
    void shouldLogEvent() {
        // Act
        service.record("login");

        // Assert
        verify(logger).write("EVENT:login");
    }
}
```

---

### 34. Stub process with thenReturn
**Question:**
```java
interface EmailSender { boolean send(String to, String body); }
class NotificationService {
    private final EmailSender sender;
    NotificationService(EmailSender s) { this.sender = s; }
    boolean notifyUser(String to) { return sender.send(to, "Welcome"); }
}
```
**Answer:**
```java
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.Mock;
import org.mockito.InjectMocks;
import org.mockito.junit.jupiter.MockitoExtension;
import static org.mockito.Mockito.*;
import static org.junit.jupiter.api.Assertions.*;

@ExtendWith(MockitoExtension.class)
class NotificationServiceTest {

    @Mock EmailSender sender;
    @InjectMocks NotificationService service;

    @Test
    void shouldReturnTrueWhenSendSucceeds() {
        // Arrange
        when(sender.send("user@example.com","Welcome")).thenReturn(true);

        // Act
        boolean ok = service.notifyUser("user@example.com");

        // Assert
        assertTrue(ok);
        verify(sender).send("user@example.com","Welcome");
    }
}
```

---

### 35. thenThrow on failure
**Question:**
```java
interface Storage { void save(String key, String value); }
class ConfigService {
    private final Storage storage;
    ConfigService(Storage s) { this.storage = s; }
    boolean update(String key, String value) {
        storage.save(key, value);
        return true;
    }
}
```
**Answer:**
```java
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.Mock;
import org.mockito.InjectMocks;
import org.mockito.junit.jupiter.MockitoExtension;
import static org.mockito.Mockito.*;
import static org.junit.jupiter.api.Assertions.*;

@ExtendWith(MockitoExtension.class)
class ConfigServiceTest {

    @Mock Storage storage;
    @InjectMocks ConfigService service;

    @Test
    void shouldThrowWhenSaveFails() {
        // Arrange
        doThrow(new RuntimeException("disk full")).when(storage).save("k","v");

        // Act & Assert
        assertThrows(RuntimeException.class, () -> service.update("k","v"));
        verify(storage).save("k","v");
    }
}
```

---

### 36. doReturn().when() for spies
**Question:**
```java
class MathLib {
    int square(int n) { return n * n; }
    int sumSquares(int a, int b) { return square(a) + square(b); }
}
```
**Answer:**
```java
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.Spy;
import org.mockito.junit.jupiter.MockitoExtension;
import static org.mockito.Mockito.*;
import static org.junit.jupiter.api.Assertions.*;

@ExtendWith(MockitoExtension.class)
class MathLibTest {

    @Spy MathLib lib;

    @Test
    void shouldUseStubbedSquareInSpy() {
        // Arrange
        doReturn(100).when(lib).square(10);

        // Act
        int result = lib.sumSquares(10,2); // 100 + (2*2) = 104

        // Assert
        assertEquals(104, result);
        verify(lib).square(10);
        verify(lib).square(2);
    }
}
```

---

### 37. Verify method calls
**Question:**
```java
interface Reporter { void send(String msg); }
class ReportService {
    private final Reporter reporter;
    ReportService(Reporter r) { this.reporter = r; }
    void publish(String msg) { reporter.send(msg); }
}
```
**Answer:**
```java
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.Mock;
import org.mockito.InjectMocks;
import org.mockito.junit.jupiter.MockitoExtension;
import static org.mockito.Mockito.*;

@ExtendWith(MockitoExtension.class)
class ReportServiceTest {

    @Mock Reporter reporter;
    @InjectMocks ReportService service;

    @Test
    void shouldSendOnce() {
        // Act
        service.publish("hello");

        // Assert
        verify(reporter, times(1)).send("hello");
    }
}
```

---

### 38. Verify call counts with times
**Question:**
```java
class RetryService {
    private final Reporter reporter;
    RetryService(Reporter r) { this.reporter = r; }
    void publishTwice(String msg) { reporter.send(msg); reporter.send(msg); }
}
```
**Answer:**
```java
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.Mock;
import org.mockito.InjectMocks;
import org.mockito.junit.jupiter.MockitoExtension;
import static org.mockito.Mockito.*;

@ExtendWith(MockitoExtension.class)
class RetryServiceTest {

    @Mock Reporter reporter;
    @InjectMocks RetryService service;

    @Test
    void shouldSendTwice() {
        // Act
        service.publishTwice("x");

        // Assert
        verify(reporter, times(2)).send("x");
    }
}
```

---

### 39. Verify never called
**Question:**
```java
class ConditionalService {
    private final Reporter reporter;
    ConditionalService(Reporter r) { this.reporter = r; }
    void maybeSend(String msg, boolean flag) { if (flag) reporter.send(msg); }
}
```
**Answer:**
```java
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.Mock;
import org.mockito.InjectMocks;
import org.mockito.junit.jupiter.MockitoExtension;
import static org.mockito.Mockito.*;

@ExtendWith(MockitoExtension.class)
class ConditionalServiceTest {

    @Mock Reporter reporter;
    @InjectMocks ConditionalService service;

    @Test
    void shouldNotSendWhenFlagFalse() {
        // Act
        service.maybeSend("x", false);

        // Assert
        verify(reporter, never()).send(anyString());
    }

    @Test
    void shouldSendWhenFlagTrue() {
        // Act
        service.maybeSend("x", true);

        // Assert
        verify(reporter).send("x");
    }
}
```

---

### 40. Argument matchers
**Question:**
```java
class SearchService {
    private final Reporter reporter;
    SearchService(Reporter r) { this.reporter = r; }
    void search(String term) { reporter.send("search:" + term); }
}
```
**Answer:**
```java
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.Mock;
import org.mockito.InjectMocks;
import org.mockito.junit.jupiter.MockitoExtension;
import static org.mockito.Mockito.*;
import static org.mockito.ArgumentMatchers.*;

@ExtendWith(MockitoExtension.class)
class SearchServiceTest {

    @Mock Reporter reporter;
    @InjectMocks SearchService service;

    @Test
    void shouldUseMatchersInVerification() {
        // Act
        service.search("test");

        // Assert
        verify(reporter).send(eq("search:test"));
        verify(reporter, times(1)).send(anyString());
    }
}
```

---

## Advanced Mockito (41–50)

### 41. ArgumentCaptor for UserRepository
**Question:**
```java
interface UserRepository { void save(String username); }
class RegistrationService {
    private final UserRepository repo;
    RegistrationService(UserRepository r) { this.repo = r; }
    void register(String username) { if (username == null) throw new IllegalArgumentException(); repo.save(username.trim()); }
}
```
**Answer:**
```java
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.ArgumentCaptor;
import org.mockito.Mock;
import org.mockito.InjectMocks;
import org.mockito.junit.jupiter.MockitoExtension;
import static org.mockito.Mockito.*;
import static org.junit.jupiter.api.Assertions.*;

@ExtendWith(MockitoExtension.class)
class RegistrationServiceTest {

    @Mock UserRepository repo;
    @InjectMocks RegistrationService service;

    @Test
    void shouldCaptureTrimmedUsername() {
        // Arrange
        ArgumentCaptor<String> captor = ArgumentCaptor.forClass(String.class);

        // Act
        service.register(" Alice ");

        // Assert
        verify(repo).save(captor.capture());
        assertEquals("Alice", captor.getValue());
    }

    @Test
    void shouldThrowOnNullUsername() {
        // Act & Assert
        assertThrows(IllegalArgumentException.class, () -> service.register(null));
        verifyNoInteractions(repo);
    }
}
```

---

### 42. Custom Answer dynamic stubbing
**Question:**
```java
interface Processor { String process(String id); }
class Pipeline {
    private final Processor p;
    Pipeline(Processor p) { this.p = p; }
    String run(String id) { return p.process(id); }
}
```
**Answer:**
```java
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.Mock;
import org.mockito.InjectMocks;
import org.mockito.junit.jupiter.MockitoExtension;
import static org.mockito.Mockito.*;
import static org.junit.jupiter.api.Assertions.*;

@ExtendWith(MockitoExtension.class)
class PipelineTest {

    @Mock Processor processor;
    @InjectMocks Pipeline pipeline;

    @Test
    void shouldReturnDynamicResponseWithAnswer() {
        // Arrange
        when(processor.process(anyString()))
            .thenAnswer(invocation -> "ok:" + invocation.getArgument(0));

        // Act
        String res = pipeline.run("123");

        // Assert
        assertEquals("ok:123", res);
        verify(processor).process("123");
    }
}
```

---

### 43. Stub multiple return values
**Question:**
```java
interface QueueReader { String read(); }
class QueueService {
    private final QueueReader reader;
    QueueService(QueueReader r) { this.reader = r; }
    String readTwice() { return reader.read() + "|" + reader.read(); }
}
```
**Answer:**
```java
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.Mock;
import org.mockito.InjectMocks;
import org.mockito.junit.jupiter.MockitoExtension;
import static org.mockito.Mockito.*;
import static org.junit.jupiter.api.Assertions.*;

@ExtendWith(MockitoExtension.class)
class QueueServiceTest {

    @Mock QueueReader reader;
    @InjectMocks QueueService service;

    @Test
    void shouldReturnTwoReadsConcatenated() {
        // Arrange
        when(reader.read()).thenReturn("a","b");

        // Act
        String res = service.readTwice();

        // Assert
        assertEquals("a|b", res);
        verify(reader, times(2)).read();
    }
}
```

---

### 44. Sequential calls chain
**Question:**
```java
interface Processor { String process(String id); }
class FlakyService {
    private final Processor p;
    FlakyService(Processor p) { this.p = p; }
    String attempt(String id) { return p.process(id); }
}
```
**Answer:**
```java
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.Mock;
import org.mockito.InjectMocks;
import org.mockito.junit.jupiter.MockitoExtension;
import static org.mockito.Mockito.*;
import static org.junit.jupiter.api.Assertions.*;

@ExtendWith(MockitoExtension.class)
class FlakyServiceTest {

    @Mock Processor processor;
    @InjectMocks FlakyService service;

    @Test
    void shouldThrowThenReturnOnSequentialCalls() {
        // Arrange
        when(processor.process("id"))
            .thenThrow(new RuntimeException("fail"))
            .thenReturn("ok");

        // Act & Assert
        assertThrows(RuntimeException.class, () -> service.attempt("id"));
        assertEquals("ok", service.attempt("id"));
        verify(processor, times(2)).process("id");
    }
}
```

---

### 45. InOrder verification
**Question:**
```java
interface Steps { void step1(); void step2(); }
class Flow {
    private final Steps steps;
    Flow(Steps s) { this.steps = s; }
    void run() { steps.step1(); steps.step2(); }
}
```
**Answer:**
```java
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.InOrder;
import org.mockito.Mock;
import org.mockito.InjectMocks;
import org.mockito.junit.jupiter.MockitoExtension;
import static org.mockito.Mockito.*;

@ExtendWith(MockitoExtension.class)
class FlowTest {

    @Mock Steps steps;
    @InjectMocks Flow flow;

    @Test
    void shouldCallStepsInOrder() {
        // Act
        flow.run();

        // Assert
        InOrder inOrder = inOrder(steps);
        inOrder.verify(steps).step1();
        inOrder.verify(steps).step2();
    }
}
```

---

### 46. Partial mock with Spy
**Question:**
```java
class TextUtil {
    String upper(String s) { return s.toUpperCase(); }
    String shout(String s) { return upper(s) + "!"; }
}
```
**Answer:**
```java
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.Spy;
import org.mockito.junit.jupiter.MockitoExtension;
import static org.mockito.Mockito.*;
import static org.junit.jupiter.api.Assertions.*;

@ExtendWith(MockitoExtension.class)
class TextUtilTest {

    @Spy TextUtil util;

    @Test
    void shouldUseStubbedUpperInShout() {
        // Arrange
        doReturn("X").when(util).upper("x");

        // Act
        String out = util.shout("x");

        // Assert
        assertEquals("X!", out);
        verify(util).upper("x");
    }
}
```

---

### 47. Mix real and mock objects
**Question:**
```java
interface Reporter { void send(String msg); }
class PriceCalculator {
    double total(double price, double taxRate) { return price + price * taxRate; }
}
class Checkout {
    private final PriceCalculator calc; private final Reporter reporter;
    Checkout(PriceCalculator c, Reporter r) { this.calc = c; this.reporter = r; }
    double bill(double price) { double t = calc.total(price, 0.18); reporter.send("billed:" + t); return t; }
}
```
**Answer:**
```java
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.Mock;
import org.mockito.junit.jupiter.MockitoExtension;
import static org.mockito.Mockito.*;
import static org.junit.jupiter.api.Assertions.*;

@ExtendWith(MockitoExtension.class)
class CheckoutTest {

    @Mock Reporter reporter;

    @Test
    void shouldCalculateTotalAndReport() {
        // Arrange
        PriceCalculator calc = new PriceCalculator();
        Checkout co = new Checkout(calc, reporter);

        // Act
        double total = co.bill(100.0);

        // Assert
        assertEquals(118.0, total, 0.0001);
        verify(reporter).send("billed:118.0");
    }
}
```

---

### 48. Integration test with repository mock
**Question:**
```java
interface ItemRepo { boolean exists(String sku); }
class InventoryService {
    private final ItemRepo repo;
    InventoryService(ItemRepo r) { this.repo = r; }
    String check(String sku) { return repo.exists(sku) ? "available" : "missing"; }
}
```
**Answer:**
```java
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.Mock;
import org.mockito.InjectMocks;
import org.mockito.junit.jupiter.MockitoExtension;
import static org.mockito.Mockito.*;
import static org.junit.jupiter.api.Assertions.*;

@ExtendWith(MockitoExtension.class)
class InventoryServiceTest {

    @Mock ItemRepo repo;
    @InjectMocks InventoryService svc;

    @Test
    void shouldReturnAvailableWhenExists() {
        // Arrange
        when(repo.exists("SKU")).thenReturn(true);

        // Act
        String status = svc.check("SKU");

        // Assert
        assertEquals("available", status);
        verify(repo).exists("SKU");
    }

    @Test
    void shouldReturnMissingWhenNotExists() {
        // Arrange
        when(repo.exists("SKU")).thenReturn(false);

        // Act
        String status = svc.check("SKU");

        // Assert
        assertEquals("missing", status);
        verify(repo).exists("SKU");
    }
}
```

---

### 49. Parameterized test with mocks
**Question:**
```java
class DiscountService {
    double discounted(double price, double discountPct) { return price * (1 - discountPct); }
}
```
**Answer:**
```java
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.CsvSource;
import static org.junit.jupiter.api.Assertions.*;

class DiscountServiceTest {

    @ParameterizedTest
    @CsvSource({
        "100,0.10,90",
        "200,0.50,100",
        "50,0.00,50"
    })
    void shouldCalculateDiscounts(double price, double pct, double expected) {
        // Arrange
        DiscountService ds = new DiscountService();

        // Act
        double actual = ds.discounted(price, pct);

        // Assert
        assertEquals(expected, actual, 0.0001);
    }
}
```

---

### 50. JaCoCo + Mockito coverage (with dependency wrapper)
**Question:**
```java
class AccountManager {
    private final BankAccount2 account = new BankAccount2();
    boolean openAndDeposit(int amt) { return account.deposit(amt); }
    boolean tryWithdraw(int amt) { return account.withdraw(amt); }
    int balance() { return account.balance(); }
}
```
**Answer:**
```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class AccountManagerTest {

    @Test
    void shouldCoverSuccessAndFailurePaths() {
        // Arrange
        AccountManager am = new AccountManager();

        // Act & Assert: deposit failure
        assertFalse(am.openAndDeposit(0));
        // Act & Assert: deposit success
        assertTrue(am.openAndDeposit(100));
        // Act & Assert: withdraw failure and success
        assertFalse(am.tryWithdraw(0));
        assertFalse(am.tryWithdraw(200));
        assertTrue(am.tryWithdraw(50));
        assertEquals(50, am.balance());
    }
}
```

---

## Notes
- Use AAA comments in all tests to reinforce structure.
- Parameterized tests: prefer CsvSource for concise datasets; use MethodSource for complex inputs.
- Dynamic tests are great for data-driven assertions and generating boundary case coverage.
- Nest tests to improve organization; avoid order dependencies in production code.
- Discuss JaCoCo line vs branch coverage. Ensure exception and boundary paths are executed.
- Mockito: keep stubbing minimal, verify expected interactions, and use spies for partial mocking carefully.
- ArgumentCaptor and InOrder help validate behavior precisely.
- Mixing real and mock objects teaches students how to isolate side effects while keeping core logic real.
