#JUnit 5 vs JUnit 4.md


Unit 5, which is the next generation of JUnit, promises to be a programmer-friendly testing framework for Java 8. In this post, JUnit 5 vs JUnit 4, let’s discover some major differences between them.

JUnit 5 vs JUnit 4
JUnit 5 vs JUnit 4

1. Architecture
JUnit 4.

 
All in one.

JUnit 5.
There are 3 separated modules:

JUnit 5 = JUnit Platform + JUnit Jupiter + JUnit Vintage

JUnit Platform

Foundation for launching testing frameworks on the JVM. Defines the TestEngine API for developing a testing framework that runs on the platform.


 
JUnit Jupiter

Includes new programming model and extension model for writing tests and extensions in JUnit 5 such as new assertions, new annotations, Java 8 Lambda Expressions, etc.

JUnit Vintage

Supports running JUnit 3 and JUnit 4 based tests on the JUnit 5 platform.

2. Supported Java Versions
JUnit 4
Java 5 or higher.

JUnit 5
Java 8 or higher.

3. Annotations
Features
JUnit 5
JUnit 4
Declares a test method	@Test	@Test
Denotes that the annotated method will be executed before all test methods  in the current class	@BeforeAll	@BeforeClass
Denotes that the annotated method will be executed after all test methods  in the current class	@AfterAll	@AfterClass
Denotes that the annotated method will be executed before each test method	@BeforeEach	@Before
Denotes that the annotated method will be executed after each test method	@AfterEach	@After
Disable a test method or a test class	@Disable	@Ignore
Denotes a method is a test factory for dynamic tests in JUnit 5	@TestFactory	N/A
Denotes that the annotated class is a nested, non-static test class	@Nested	N/A
Declare tags for filtering tests	@Tag	@Category
Register custom extensions in JUnit 5	@ExtendWith	N/A
Repeated Tests in JUnit 5	@RepeatedTest	N/A
4. Assertions.
JUnit 4
JUnit 5
fail	fail
assertTrue	assertTrue
assertThat	N/A
assertSame	assertSame
assertNull	assertNull
assertNotSame	assertNotSame
assertNotEquals	assertNotEquals
assertNotNull	assertNotNull
assertFalse	assertFalse
assertEquals	assertEquals
assertArrayEquals	assertArrayEquals
assertAll
assertThrows
Essential notes

Except for 2 last annotations (assertAll, assertThrows) only existing in JUnit 5, almost remaining annotations are the same between JUnit 5 vs JUnit 4. However, there is some difference in method signatures  as below:

4.1. Difference  in the position of optional assertion message parameter
JUnit 4.

assertEquals( "The optional assertion message.",1, 1);
1
assertEquals( "The optional assertion message.",1, 1);
The optional assertion message is the first parameter applied for all assertion methods support it.

JUnit 5.

assertEquals(1, 1, "The optional assertion message.");
1
assertEquals(1, 1, "The optional assertion message.");
The optional assertion message is the last parameter applied for all assertion methods support it.

4.2. Assert methods in JUnit 5 can be used with Java 8 Lambdas.
For examples:

assertTrue(1 == 1, () -> "Assertion messages can be provided by Java 8 Lambdas ");
1
assertTrue(1 == 1, () -> "Assertion messages can be provided by Java 8 Lambdas ");
 Throwable exception = expectThrows(IllegalArgumentException.class, () -> {
            throw new IllegalArgumentException("Invalid age.");
        });
1
2
3
 Throwable exception = expectThrows(IllegalArgumentException.class, () -> {
            throw new IllegalArgumentException("Invalid age.");
        });
5. Assumptions
JUnit 4
JUnit 5
assumeFalse	assumeFalse
assumeNoException	
assumeNotNull	
assumeThat	assumeThat
assumeTrue	assumeTrue
JUnit 5 provides fewer assumptions than JUnit 4. Note that JUnit 5 overloads each its assumption method to be used with Java 8 Lambda Expressions, for ex:

assumeTrue("QA".equals(System.getenv("ENV")),
            () -> "Should run on QA environment");
1
2
assumeTrue("QA".equals(System.getenv("ENV")),
            () -> "Should run on QA environment");
6. Tagging and Filtering (JUnit 5) vs Label and Group tests (JUnit 4)
JUnit 4
Use @category annotation for label and group tests.

Example:

public interface FastTests { /* category marker */ }
public interface SlowTests { /* category marker */ }

public class A {
  @Test
  public void a() {
    fail();
  }

  @Category(SlowTests.class)
  @Test
  public void b() {
  }
}

@Category({SlowTests.class, FastTests.class})
public class B {
  @Test
  public void c() {

  }
}

@RunWith(Categories.class)
@IncludeCategory(SlowTests.class)
@SuiteClasses( { A.class, B.class }) // Note that Categories is a kind of Suite
public class SlowTestSuite {
  // Will run A.b and B.c, but not A.a
}


public interface FastTests { /* category marker */ }
public interface SlowTests { /* category marker */ }
 
public class A {
  @Test
  public void a() {
    fail();
  }
 
  @Category(SlowTests.class)
  @Test
  public void b() {
  }
}
 
@Category({SlowTests.class, FastTests.class})
public class B {
  @Test
  public void c() {
 
  }
}
 
@RunWith(Categories.class)
@IncludeCategory(SlowTests.class)
@SuiteClasses( { A.class, B.class }) // Note that Categories is a kind of Suite
public class SlowTestSuite {
  // Will run A.b and B.c, but not A.a
}
JUnit 5
Use @tag annotation for tagging and filtering.

@Tag("fast")
@Tag("model")
class TaggingDemo {

    @Test
    @Tag("taxes")
    void testingTaxCalculation() {
    }


@Tag("fast")
@Tag("model")
class TaggingDemo {
 
    @Test
    @Tag("taxes")
    void testingTaxCalculation() {
    }
 
}
7. Parameterized Tests
JUnit 5 supports Parameterized Tests by default. This feature allows us to run a test multiple times with different arguments.

For example, let’s see the following test:

@ParameterizedTest
@ValueSource(strings = { "Hello", "World" })
void testWithStringParameter(String argument) {
    assertNotNull(argument);
}
1
2
3
4
5
@ParameterizedTest
@ValueSource(strings = { "Hello", "World" })
void testWithStringParameter(String argument) {
    assertNotNull(argument);
}
The @ParameterizedTest and @ValueSource annotations make the test can run with each value provided by the @ValueSource annotation. For instance, the ConsoleLauncher will print output similar to the following:

testWithStringParameter(String) ✔
├─ [1] Hello ✔
└─ [2] World ✔
1
2
3
testWithStringParameter(String) ✔
├─ [1] Hello ✔
└─ [2] World ✔
Besides the @ValueSource, JUnit 5 provides many kinds of sources can be used with Parameterized Tests such as:

@CsvFileSource: lets us use CSV files from the classpath. Each line from a CSV file results in one invocation of the parameterized test.
@MethodSource: allows us to refer to one or multiple methods of the test class. Each method must return a Stream, an Iterable, an Iterator, or an array of arguments.
@ArgumentsSource: can be used to specify a custom, reusable ArgumentsProvider.
@EnumSource: provides a convenient way to use Enum constants.
8. Repeated Tests
A new feature in JUnit5 which allows us to repeat a test in a specified number of times is Repeated Tests. Let’s see an example which declares a test that will be repeated in 100 times:

@RepeatedTest(100)
void repeatedTest() {
    // ...
}
1
2
3
4
@RepeatedTest(100)
void repeatedTest() {
    // ...
}
9. Dynamic Tests
JUnit 5 introduces the concept of Dynamic Tests which are tests that can be generated at runtime by a factory method. Let’s see an example which we generate 2 tests at runtime:

@TestFactory
Collection<DynamicTest> dynamicTestsFromCollection() {
    return Arrays.asList(
        dynamicTest("1st dynamic test", () -> assertTrue(true)),
        dynamicTest("2nd dynamic test", () -> assertEquals(4, 2 * 2))
    );
}

@TestFactory
Collection<DynamicTest> dynamicTestsFromCollection() {
    return Arrays.asList(
        dynamicTest("1st dynamic test", () -> assertTrue(true)),
        dynamicTest("2nd dynamic test", () -> assertEquals(4, 2 * 2))
    );
}
You can refer to another article: JUnit 5 Dynamic Tests – Generate Tests at Run-time for more detail.

10. Conclusions
We have just get through JUnit 5 vs JUnit 4 in major points. We may see that JUnit 5 has a big change in its architecture which related to platform launcher, integration with build tool, IDE, other Unit test frameworks, etc. In regarding the writing test, JUnit 5 support Java 8 Lambdas. All annotation, assumptions are almost the same with JUnit 4. On the next post, I’d like to go into detail about how to write tests, use annotations, assumption, and integrate with other testing frameworks.

If you want to start with JUnit 5, you can refer to my recent posts:

JUnit 5 Tutorial

JUnit 5 and Spring Boot Example

JUnit 5 Basic Introduction

JUnit 5 Annotations Example

JUnit 5 Assertions Example

JUnit 5 Disable or Ignore A Test

JUnit 5 Exception Testing

JUnit 5 Test Suite – Aggregating Tests In Suites

JUnit 5 Assumptions With Assume

JUnit 5 Nested Tests Examples

JUnit 5 Maven Example

JUnit 5 with Gradle Example

JUnit 5 Parameter Resolution Example

Timeout Test in JUnit 5