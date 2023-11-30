## Concept

Mocking refers to the development of objects, which are a mock or clone of real objects; 

## Use Case
In unit testing, Mock are used when the program/function you try to test have a dependency on an external system, such as a database interaction ,or an external service. 

for example, if we need to test a function sendInvitation(MailServer mailServer), which sends a static email to the given user

```
class mailServer {
public boolean sendInvitation(sender){

}
...
}
```

you can Mock the mailServer object, and define the return of the "mocked" mailServer.sendInvitation based on the parameters received. 

```
        mailServer  mailServer = mock(mailServer .class);
        when(mailServer.sendInvitation(any())).thenReturn(true);
        
        mailSucceed = mailServer.sendInvitation('asdf@gmail.com');
        assertEquals( true, mailSucceed);
```
and then carry on with the rest of testing.

## Advantages of using mocking

pro:
1. requires less setup
2. no cleanup. Since the updates are not made, you don't need to rollback the changes;
3. no dependencies are required.
4. isolated; Task will still pass, even if DB/service fails; 

con:
1. less confidence, as you are using a fake environment;

## Setup
1. Add the following code to the build.gradle file;
```
testImplementation 'org.mockito:mockito-junit-jupiter:5.7.0'
testImplementation 'org.junit.jupiter:junit-jupiter:5.9.3'
```

## Steps (mock)

1. import
```
import static org.mockito.Mockito.mock;
import static org.mockito.Mockito.when;
```
2. create a mock class
```
        board = mock(Board.class);
```
3. stub the class method to return the expected result
```
        when(board.CountSpaces(Space.Black)).thenReturn(1);
        when(board.CountSpaces(Space.White)).thenReturn(2);
```
4. call function using the mock class
```
        int white = board.CountSpaces(Space.White);
```

## Basic Example (mock)


The testing code mocks the board object, defines different returns for the board.CountSpaces function when Space.Black & Space.White is passed in.

```
    @Test
    public void testCalculateWinner () {
        game.board = mock(Board.class);
        when(game.board.CountSpaces(Space.Black)).thenReturn(1);
        when(game.board.CountSpaces(Space.White)).thenReturn(2);

        int white = game.board.CountSpaces(Space.White);
        int black = game.board.CountSpaces(Space.Black);
        
        
        if (white > black) {
            game.Winner = Space.White;
        } else if (black > white) {
            game.Winner = Space.Black;
        }

        assertEquals( Space.White, game.Winner);
        
    }
```

## Steps (spy)

1. import
```
        import static org.mockito.Mockito.spy;
	import static org.mockito.Mockito.times;
        import static org.mockito.Mockito.verify;
```
2. create any object, and uses it to create spy.
```
        ArrayList<Pos> positions = new ArrayList<Pos>();
        ArrayList<Pos> spyList = spy(positions);
```
3. uses the new "spyList" in place of "position"
```
        for (Move move : game.possible) {
            spyList.add(new Pos(move.Position));
        }
```
4. verify how many times the function is called
```
        verify(spyList, times(4)).add(any());
```

## Basic Example (Spy)

```
    @Test
    public void testGetPossiblePositions () {
        ArrayList<Pos> positions = new ArrayList<Pos>();
        ArrayList<Pos> spyList = spy(positions);

        for (Move move : game.possible) {
            spyList.add(new Pos(move.Position));
        }
        
        
        verify(spyList, times(4)).add(any());
        
    }
```
