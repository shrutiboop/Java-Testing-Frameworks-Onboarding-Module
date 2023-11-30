*Written by Kyle Findlay*

This code is a terminal implementation of the board game Othello, the rules of which can be found [Here](https://www.worldothello.org/about/about-othello/othello-rules/official-rules/english).

The program contains 6 different classes and an enum type

*Note: Some classes are described further below*

| Name | Description |
|-|-|
| AI | This contains a couple of static functions that implement a computer player |
| App | This class contains main and is responsible for all terminal prints and terminal interaction |
| Board | This class contains the actual data structure representing the game board and implements the logic for finding possible moves on the board |
| Game | The Game class implements the actual rules for Othello as well as saving and loading of a game |
| Move | This is a data structure that contains move data: <ul><li>Positon to place the piece</li><li>Colour of the piece being placed</li><li>An ArrayList of all the pieces that get flipped once the piece is placed</li></ul> |
| Pos | This is a simple data structure that represents positions on the board. It simply contains an (x, y) position on the board |
| Space | This is an enum type that denotes what is on a space on the board. the types are <ul><li>None</li><li>Black</li><li>White</li></ul> |

---

## Board

**Methods**

| Name | Inputs | Description | Returns |
|-|-|-|-|
| GetReverse | <ul><li>Space rev</li></ul> | Reverses Space.White to Space.Black and vice versa. Leaves Space.None the same | The reversed Space type |
| ReplacePiece | <ul><li>Space type</li><li>int x</li><li>int y</li></ul> | Replaces a piece at position (x, y) with type |  |
| PlayMove | <ul><li>Move move</li></ul> | Follows the instructions in move to update the board |  |
| GetPossibleMoves | <ul><li>Space Colour</li></ul> | Gets the list of all possible moves for the player of the colour Colour | An ArrayList<Move\> containing all the possible moves for the given colour |
| GetFlips | <ul><li>Space colour</li><li>int x</li><li>int y</li></ul> | Uses the colour and the position given to calculate all the flips that would happen if the piece was placed in the position given | An ArrayList<Pos\> that contains all spaces that would be flipped as a result of placing the piece in the given position |
| LookInDirection | <ul><li>ArrayList<Pos\> flips</li><li>Space colour</li><li>Space look</li><li>int startx</li><li>int starty</li>int changex<li></li>int changey</ul> | Calculates the flips that would occur if the piece was placed in the start location (startx, starty) in the direction (changex, changey) and appends them to flips |  |
| CountSpaces | <ul><li>Space Colour</li></ul> | Counts all the spaces of a given colour on the board | The amount of spaces of that colour on the board |
| toString |  | Generates a string representation of the board where: <ul><li>'.' means None</li><li>'b' means Black</li><li>'w' means White</li></ul> |  |

**Additional Notes**
- The board class creates a two dimensional array of size [BoardSize + 2, BoardSize + 2] this is to make the other functions of the Board class not go out of bounds while calculating flips
- The outer edge of the board should always be empty

---

## Game

**Methods**

| Name | Inputs | Description | Returns |
|-|-|-|-|
| GetPossiblePositions |  | Gets the possible moves for the current turn player | An ArrayList<Pos\> of all the possible positions the current turn player can place a piece |
| GetBoardString |  |  | A string representation of the board |
| PlacePiece | <ul><li>Pos position</li></ul> | Checks if the position given is possible for the current turn player and places the piece if it is. Also updates the board accordingly (flips pieces) | true if the piece was placed, false if not |
| MakeAIMove |  | Asks the AI for the move it would choose out of the possible moves and updates the board |  |
| SwitchTurn |  | Switches the turn player and checks if the player is forced to pass. Also checks if the game is over |  |
| SaveToFile | <ul><li>String path</li></ul> | Saves the current game to a file at the path given. If no file exists it creates one and if one already exists it overwrites the file | true if the save was successful and false if it was not |
| LoadBoard | <ul><li>String path</li></ul> | Loads a game from a file located at path | true if the file was loaded correctly and false if it was not |
| CalculateWinner |  | Counts all the pieces on the board and updates the Winner |  |
