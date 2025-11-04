# Tetris-csharp
a simple proof of concept tetris game to show cSharp reading writing and use.


Explanation of the Tetris Implementation


# Game size:
The game uses a 20x10 grid (Board) to represent the play area. Each cell is either empty (.) or occupied by a tetromino (#).
Tetrominoes: Seven standard tetromino shapes (Square, I, T, S, Z, L, J) are defined as 4x4 arrays in the Pieces array. Each piece is randomly selected when a new piece is needed.
Game Loop: The Main method runs a loop that:

Draws the current state of the board.
Handles user input for moving or rotating the piece.
Attempts to move the piece down every 100ms.
Places the piece when it cannot move further, checks for completed lines, and spawns a new piece.blocking input detection in a Unix-like environment (Linux/Mac). For Windows, it may require adjustments (example: using msvcrt).

# Input Handling: The HandleInput method processes:

Left/Right Arrow: Moves the piece horizontally if possible.
Down Arrow: Forces the piece to move down immediately.
Up Arrow: Rotates the piece 90 degrees clockwise.


# Collision Detection:
Collision Detection: The CanMove and CanPlacePiece methods check if a piece can move to a new position or be placed without overlapping existing blocks or exceeding board boundaries.
Rotation: The RotatePiece method rotates the current piece by transforming its 4x4 grid. It ensures the rotation is valid before applying it.
Line Clearing: The ClearLines method checks for completed rows, removes them, shifts the board down, and increments the score by 100 points per line.
Game Over: The game ends if a new piece cannot be placed at the starting position.

# How to Play (and rules)
How to Play

Run the Program: Compile and run the C# code in a .NET environment (e.g., Visual Studio or .NET CLI).
Controls:

Left Arrow: Move piece left.
Right Arrow: Move piece right.
Down Arrow: Move piece down faster.
Up Arrow: Rotate piece clockwise.


Objective: Stack tetrominoes to form complete horizontal lines, which are cleared to increase your score. The game ends when a new piece cannot be placed.

## Notes:: 
Notes

This is a console-based implementation for simplicity. For a graphical version, consider using a library like SFML.NET or Unity with C#.
The game speed is controlled by Thread.Sleep(100). Adjust this value to make the game faster or slower.
The score increments by 100 per cleared line. You can enhance this by adding bonuses for multiple lines cleared simultaneously.

Do keep in mind this is proof of concept.

