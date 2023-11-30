*Written by Kyle Findlay*

Spock is a scripting based framework for Java using Groovy Scripts. All the code presented on this page can be found on the Spock branch of the repo.

### Basic Test Cases

Let's start off by looking at the basic functionality of Spock.

When choosing Spock when using `gradle init` grants this starting file:

```groovy
import spock.lang.Specification

class AppTest extends Specification {
    def "application has a greeting"() {
        setup:
        def app = new App()

        when:
        def result = app.greeting

        then:
        result != null
    }
}
```

All tests that will be run in the script are in a class that extends `Specification` which is imported at the top. For example the class in the generated file `AppTest`.

Now let's take a look at the test Gradle starts us with. Spock uses a `setup`, `when` and `then` system to define tests.
- `setup` is where any initialization for the test happens like making variables or objects needed for the test. In this case we initialize an object called `app` that is of class type `App`
- `when` is the actual code that the test runs in this case it gets the value of `greeting` from `app`
- `then` is the condition that is checked for the test case to pass or fail. In this case we are making sure that the result given by `app.greeting` is not `null`

*Note: There are other ways to define tests that we will see later.*

---

## Writing our own tests

To start we will look at `Board.java` and write some basic tests.

The first function I will look at is `ReplacePiece`
```java
public void ReplacePiece (Space type, int x, int y) {
    if (x < 1 || x > BoardSize || y < 1 || y > BoardSize) { return; }
    board[x][y] = type;
}
```

Let's write a basic test to see if this function does what it is supposed to do

```groovy
def "Replace a piece on the board"() {
    setup:
    def board = new Board()

    when:
    board.ReplacePiece(Space.White, 1, 1)
    def result = board.board[1][1]

    then:
    result == Space.White
}
```

This test will simply replace the piece at (1, 1) with a White piece

To view the results of our tests Gradle creates an HTML document based on our tests that can be located at `app\build\reports\tests\test\index.html`.

*Note: Spock does not care about access control and can access private things freely to make testing easier*

Now let's write something to test the `PlayMove` method

```java
public void PlayMove (Move move) {
    board[move.Position.X()][move.Position.Y()] = move.Type();

    if (move.Flips != null) {
        for (Pos pos : move.Flips) {
            Space colour = board[pos.X()][pos.Y()];
            board[pos.X()][pos.Y()] = GetReverse(colour);
        }
    }
}
```

```groovy
def "Play a move"() {
    setup:
    def board = new Board()
    def flips = new ArrayList<Pos>()
    flips.add(new Pos(4, 4))
    def move = new Move(new Pos(4, 3), Space.Black, flips)
    board.PlayMove(move)

    expect:
    board.board[x][y] == space

    where:
    x << [4, 4, 4, 5, 5]
    y << [3, 4, 5, 4, 5]
    space << [Space.Black, Space.Black, Space.Black, Space.Black, Space.White]
}
```

This test is written a little different from before. Instead of using `setup`, `when`, `then` it uses `setup`, `expect` and `where`. doing it this way makes it easier to check multiple things at once.

The `setup` is similar, setting up a couple of things we need for the test.

The `expect` is a little different. It gets used like a method with arguments `x`, `y` and `space`. These arguments are defined in the `where`.

The `where` defines the arguments that the `expect` uses and it will create a test for each index of the given values. For instance it will create a test for when:
- `x` = 4
- `y` = 3
- `space` = Space.Black

So the first value for each variable.

And then it will push it through the `expect` *expecting* to see that it is true. If it is false the test case will fail.

---

## Testing Multiple Things in Order

In a lot of case you would want to test multiple things given the same setup or objects / variables and Spock let's you do that very easily.

For this next segment of the Tutorial we will be looking at `Game.java`

We will start by making a new groovy script and setting up a couple of things.

```groovy
import spock.lang.Stepwise
import spock.lang.Shared

@Stepwise
class TestGame extends Specification {
    @Shared def game = new Game()

    def setupSpec() {
        println "Enter Setup"
        def pos = new Pos(3, 4)
        println "Positon Placed is " + pos.toString()
        game.PlacePiece(pos)
        println "Exit Setup"
    }
}
```

To start we are defining a new class with a game that is a `@Shared` variable. This allows all tests to use this object during testing.

we then define a `setupSpec` function that runs once at the start of testing for this class. The setup here simply makes a move placing a black piece at (3, 4) and flipping the piece at (4, 4) to black.

*Note: if you want to run a setup at the start of all tests in a class use `setup` instead of `setupSpec`.*

To make sure the tests run in order and stop running once one fails we use the `@Stepwise` directive above the class. Without this the tests could run in any order and if a test fails the others will still run. We don't want this to happen here since the tests we are going to write will depend on each other and *must* run in order.

You'll notice that I am using a `println` function in the setup. This allows you to print out things that are happening and it is really useful for debugging what is wrong. It will appear in the HTML output that Gradle generates.

Now we will make a test to save this board to a file.

```groovy
def "Save to file"() {
    expect:
    game.SaveToFile("test.sav")
}
```

The test is very simple because we already setup the game in `setupSpec`.

Next we will make a test to play another move to change the board.

```groovy
def "Play a move"() {
    setup:
    println "\nPlay a move"
    def turn = game.CurrentTurn()
    def pos = game.GetPossiblePositions()[0]

    println "Positon Placed is " + pos.toString()
    game.PlacePiece(pos)
    println "Exit"

    expect:
    game.board.board[pos.x][pos.y] == turn
}
```

For this test we will only check that the one piece was placed, not check that the move was made correctly just to keep things simple.

Next we will create a test to load the board that we saved in the first test case.

```groovy
def "Load a board"() {
    expect:
    game.LoadBoard("test.sav")
    game.CurrentTurn() == Space.White
}
```

And we will test if the spaces on the board are correct.

```groovy
def "Board loaded Correctly"() {
    expect:
    game.board.board[x][y] == space

    where:
    x << [3, 4, 5, 4, 5]
    y << [4, 4, 4, 5, 5]
    space << [Space.Black, Space.Black, Space.Black, Space.Black, Space.White]
}
```

Finally we will remove the save file produced using a `cleanupSpec`. Like before if you want the cleanup to run after every test you can use `cleanup` instead of `cleanupSpec`

```groovy
def cleanupSpec () {
    def file = new File("test.sav")
    if (file.delete()) {
        println "\nDeleted the save file"    
    } else {
        println "\nCouldn't Delete the save file"
    }
}
```

This class now tests saving and loading of games and even cleans up after itself!

And that is it for the tutorial on Spock!
