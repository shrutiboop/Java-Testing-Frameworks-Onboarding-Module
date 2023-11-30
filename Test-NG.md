# What is TestNG?

TestNG is a powerful testing framework for Java, offering features beyond basic test execution. Here are some key capabilities:

## 1. Annotations:
- TestNG provides a rich set of annotations for controlling test flow.
- Commonly used annotations include `@BeforeSuite`, `@AfterSuite`, `@BeforeTest`, `@AfterTest`, `@BeforeClass`, `@AfterClass`, `@BeforeMethod`, and `@AfterMethod`.
- These annotations facilitate setting up and tearing down test environments and provide flexibility in test execution.

## 2. Parameterization:
- TestNG supports parameterization of test methods using `@Parameters` annotation, allowing the same test to run with different data sets.

## 3. Groups:
- TestNG allows grouping of test methods using the `groups` attribute of the `@Test` annotation, enabling the execution of specific sets of tests.
  ```java
  @Test(groups = "smoke")
  public void smokeTest1() {
    // Smoke test code
  }

## 4. Listeners:
- TestNG listeners allow customization and extension of TestNG's behavior.
- Custom listeners can perform actions before or after test methods, suites, classes, etc.

  ```java
  import org.testng.ITestListener;
  import org.testng.ITestResult;

  public class CustomListener implements ITestListener {
    @Override
    public void onTestSuccess(ITestResult result) {
      System.out.println("Test passed: " + result.getName());
    }

## 5. Data Providers:
- TestNG supports data providers for data-driven testing.
- Using `@DataProvider` annotation, you can supply data to test methods.

  ```java
  @Test(dataProvider = "myDataProvider")
  public void dataDrivenTest(String data) {
    System.out.println("Data-driven test with data: " + data);
  }

  @DataProvider(name = "myDataProvider")
  public Object[][] provideData() {
    return new Object[][] {
      {"Data1"},
      {"Data2"},
      // More data
    };
   }


## 6. Parallel Execution:

TestNG empowers developers to achieve parallel execution of test methods at various levels—suite, test, class, or method. This parallelism is configured within the TestNG XML file.

### Example 1:

  ```xml
  <!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd">
   <suite name="MyTestSuite">
    <test name="MyTest" parallel="methods">
      <classes>
        <class name="com.example.MyTestClass"/>
      </classes>
    </test>
   </suite>
  ```


# Advantages of TestNG over JUnit

TestNG, a testing framework for Java, offers several advantages over JUnit, making it a preferred choice for many developers.

## 1. Parallel Execution:

TestNG: Supports parallel test execution out of the box.
JUnit: Limited support for parallel test execution, often requiring additional configuration.

## 2. Flexible Test Configuration

TestNG: Offers flexible configuration through annotations and XML, allowing for easy customization.
JUnit: Configuration is primarily annotation-based, with fewer options for customization


## 3. Grouping of Tests::

TestNG: Allows grouping of tests for selective execution.
JUnit: Grouping is possible but may not be as straightforward as in TestNG.

## 4. Additional Setup/Teardown Levels:

TestNG introduces additional levels of setup and teardown with annotations like `@BeforeSuite`, `@AfterSuite`, `@BeforeTest`, `@AfterTest`, `@BeforeGroup`, and `@AfterGroup`. This granularity provides more control over the test environment.


These advantages collectively contribute to TestNG's popularity among Java developers for writing efficient, organized, and easily maintainable test suites. But TestNG and Junit are almost similar than testing with Spock.



**Setup**

Open your build.gradle file and add the TestNG dependency:

  ```
  dependencies {
    testImplementation 'org.testng:testng:7.8.0'
  }
  ```
Run Tests with Gradle:

Open a terminal, navigate to your project's root directory, and execute the following command:
  ```
  gradle test
  ```
View TestNG Reports:

After running tests, TestNG generates HTML reports in the build/reports/tests directory. Open the HTML file in a web browser to view detailed test results.

### Basic Example - BoardTest.java

  ```java
  import org.testng.annotations.Test;
  import static org.testng.Assert.*;
  import java.util.ArrayList;
  public class BoardTest {
   @Test
   public void testReplacePiece() {
   Board board = new Board();
   board.replacePiece(Space.White, 3, 3);
   assertEquals(board.getPieceAt(3, 3), Space.White);
   }
   @Test
   public void testPlayMove() {
   Board board = new Board();
   Move move = new Move(new Pos(4, 4), Space.Black, null); 
   board.playMove(move);
   assertEquals(board.getPieceAt(4, 4), Space.Black);
   }
   @Test
   public void testGetPossibleMoves() {
   Board board = new Board();
   ArrayList<Move> moves = board.getPossibleMoves(Space.Black);
   }
   @Test
   public void testCountSpaces() {
   Board board = new Board();
   int count = board.countSpaces(Space.White);
   assertEquals(count, 2); 
   }
   @Test
   public void testGetReverse() {
   assertEquals(Board.getReverse(Space.Black), Space.White);
   assertEquals(Board.getReverse(Space.White), Space.Black);
   assertEquals(Board.getReverse(Space.None), Space.None);
   }
 
   @Test
   public void testLookInDirection() {
   Board board = new Board();
   ArrayList<Pos> flips = new ArrayList<>();
   board.lookInDirection(flips, Space.Black, Space.White, 4, 4, 1, 0);
   }
   @Test
   public void testGetFlips() {
   Board board = new Board();
   ArrayList<Pos> flips = board.getFlips(Space.Black, 4, 4);
   }
   ```
### Test Methods Explained

 **testReplacePiece**

 **Purpose**: Verifies that the replacePiece method sets the correct type of space at a specified position.

Steps:

1. Creates a Board instance.
2. Calls replacePiece to replace a piece at a specific position with the color white.
3. Asserts that the space at the specified position is white.

**testPlayMove**

**Purpose**: Verifies that the playMove method correctly plays a move on the board, updating the pieces and flipping the necessary ones.

Steps:

1.Creates a Board instance.
2. Defines a move (replace with a valid move).
3. Calls playMove to play the move on the board.
4. Asserts that the piece at the move position is the correct color.

**testGetPossibleMoves**

**Purpose**: Verifies that the getPossibleMoves method returns the expected possible moves for a given color on the board.

Steps:

1. Creates a Board instance.
2. Calls getPossibleMoves for black pieces.
3. Asserts the expected behavior based on the initial state of the board.

**testCountSpaces**

**Purpose**: Verifies that the countSpaces method correctly counts the number of spaces of a specified color on the board.

Steps:

1. Creates a Board instance.
2. Calls countSpaces for white pieces.
3. Asserts that the count matches the initial count of white spaces on the board.

**testGetReverse**

**Purpose**: Verifies that the getReverse method correctly reverses the color.

Steps:

1. Calls getReverse for black, white, and none.
2. Asserts that the reversed colors are correct.

**testLookInDirection**

**Purpose:** Verifies that the lookInDirection method correctly identifies flips in a specific direction.

Steps:

1. Creates a Board instance.
2. Calls lookInDirection for a specific direction.
3. Asserts the expected behavior based on the initial state of the board.

**testGetFlips**

**Purpose**: Verifies that the getFlips method correctly identifies flips for a given move position.

Steps:

1. Creates a Board instance.
2. Calls `getFlips`



