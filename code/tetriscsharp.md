```
using System;
using System.Threading;

class Tetris
{
    private static int Width = 10;
    private static int Height = 20;
    private static char[,] Board = new char[Height, Width];
    private static int CurrentX = 0;
    private static int CurrentY = 0;
    private static int[,] CurrentPiece;
    private static Random Random = new Random();
    private static bool GameOver = false;
    private static int Score = 0;

    // Tetromino shapes (4x4 grid for each piece)
    private static int[][][] Pieces = new int[][][]
    {
        new int[][] { new int[] { 0, 1, 1, 0 }, new int[] { 0, 1, 1, 0 }, new int[] { 0, 0, 0, 0 }, new int[] { 0, 0, 0, 0 } }, // Square
        new int[][] { new int[] { 0, 0, 0, 0 }, new int[] { 1, 1, 1, 1 }, new int[] { 0, 0, 0, 0 }, new int[] { 0, 0, 0, 0 } }, // I
        new int[][] { new int[] { 0, 0, 0, 0 }, new int[] { 0, 1, 1, 1 }, new int[] { 0, 1, 0, 0 }, new int[] { 0, 0, 0, 0 } }, // T
        new int[][] { new int[] { 0, 0, 0, 0 }, new int[] { 0, 1, 1, 0 }, new int[] { 0, 0, 1, 1 }, new int[] { 0, 0, 0, 0 } }, // S
        new int[][] { new int[] { 0, 0, 0, 0 }, new int[] { 0, 1, 1, 0 }, new int[] { 1, 1, 0, 0 }, new int[] { 0, 0, 0, 0 } }, // Z
        new int[][] { new int[] { 0, 0, 0, 0 }, new int[] { 1, 1, 1, 0 }, new int[] { 0, 0, 1, 0 }, new int[] { 0, 0, 0, 0 } }, // L
        new int[][] { new int[] { 0, 0, 0, 0 }, new int[] { 0, 1, 1, 1 }, new int[] { 0, 1, 0, 0 }, new int[] { 0, 0, 0, 0 } }  // J
    };

    static void Main()
    {
        InitializeBoard();
        CurrentPiece = Pieces[Random.Next(Pieces.Length)];
        CurrentX = Width / 2 - 2;
        CurrentY = 0;

        while (!GameOver)
        {
            DrawBoard();
            HandleInput();
            if (!MoveDown())
            {
                PlacePiece();
                ClearLines();
                CurrentPiece = Pieces[Random.Next(Pieces.Length)];
                CurrentX = Width / 2 - 2;
                CurrentY = 0;
                if (!CanPlacePiece())
                {
                    GameOver = true;
                }
            }
            Thread.Sleep(100);
        }
        Console.Clear();
        Console.WriteLine($"Game Over! Score: {Score}");
    }

    private static void InitializeBoard()
    {
        for (int y = 0; y < Height; y++)
        {
            for (int x = 0; x < Width; x++)
            {
                Board[y, x] = '.';
            }
        }
    }

    private static void DrawBoard()
    {
        Console.Clear();
        Console.WriteLine($"Score: {Score}");
        char[,] display = (char[,])Board.Clone();

        // Overlay current piece
        for (int y = 0; y < 4; y++)
        {
            for (int x = 0; x < 4; x++)
            {
                if (CurrentPiece[y][x] == 1)
                {
                    int boardY = CurrentY + y;
                    int boardX = CurrentX + x;
                    if (boardY >= 0 && boardY < Height && boardX >= 0 && boardX < Width)
                    {
                        display[boardY, boardX] = '#';
                    }
                }
            }
        }

        // Draw board
        for (int y = 0; y < Height; y++)
        {
            for (int x = 0; x < Width; x++)
            {
                Console.Write(display[y, x] + " ");
            }
            Console.WriteLine();
        }
    }

    private static void HandleInput()
    {
        if (Console.KeyAvailable)
        {
            var key = Console.ReadKey(true).Key;
            switch (key)
            {
                case ConsoleKey.LeftArrow:
                    if (CanMove(CurrentX - 1, CurrentY))
                        CurrentX--;
                    break;
                case ConsoleKey.RightArrow:
                    if (CanMove(CurrentX + 1, CurrentY))
                        CurrentX++;
                    break;
                case ConsoleKey.DownArrow:
                    if (!MoveDown())
                    {
                        PlacePiece();
                        ClearLines();
                        CurrentPiece = Pieces[Random.Next(Pieces.Length)];
                        CurrentX = Width / 2 - 2;
                        CurrentY = 0;
                        if (!CanPlacePiece())
                            GameOver = true;
                    }
                    break;
                case ConsoleKey.UpArrow:
                    RotatePiece();
                    break;
            }
        }
    }

    private static bool CanMove(int newX, int newY)
    {
        for (int y = 0; y < 4; y++)
        {
            for (int x = 0; x < 4; x++)
            {
                if (CurrentPiece[y][x] == 1)
                {
                    int boardX = newX + x;
                    int boardY = newY + y;
                    if (boardY >= Height || boardX < 0 || boardX >= Width)
                        return false;
                    if (boardY >= 0 && Board[boardY, boardX] != '.')
                        return false;
                }
            }
        }
        return true;
    }

    private static bool MoveDown()
    {
        if (CanMove(CurrentX, CurrentY + 1))
        {
            CurrentY++;
            return true;
        }
        return false;
    }

    private static void PlacePiece()
    {
        for (int y = 0; y < 4; y++)
        {
            for (int x = 0; x < 4; x++)
            {
                if (CurrentPiece[y][x] == 1)
                {
                    int boardY = CurrentY + y;
                    int boardX = CurrentX + x;
                    if (boardY >= 0 && boardY < Height && boardX >= 0 && boardX < Width)
                    {
                        Board[boardY, boardX] = '#';
                    }
                }
            }
        }
    }

    private static bool CanPlacePiece()
    {
        for (int y = 0; y < 4; y++)
        {
            for (int x = 0; x < 4; x++)
            {
                if (CurrentPiece[y][x] == 1)
                {
                    int boardY = CurrentY + y;
                    int boardX = CurrentX + x;
                    if (boardY >= Height || boardX < 0 || boardX >= Width || (boardY >= 0 && Board[boardY, boardX] != '.'))
                        return false;
                }
            }
        }
        return true;
    }

    private static void RotatePiece()
    {
        int[,] rotated = new int[4, 4];
        for (int y = 0; y < 4; y++)
        {
            for (int x = 0; x < 4; x++)
            {
                rotated[x, 3 - y] = CurrentPiece[y][x];
            }
        }
        if (CanMove(CurrentX, CurrentY))
        {
            CurrentPiece = rotated;
        }
    }

    private static void ClearLines()
    {
        for (int y = Height - 1; y >= 0; y--)
        {
            bool isFull = true;
            for (int x = 0; x < Width; x++)
            {
                if (Board[y, x] == '.')
                {
                    isFull = false;
                    break;
                }
            }
            if (isFull)
            {
                Score += 100;
                for (int yy = y; yy > 0; yy--)
                {
                    for (int x = 0; x < Width; x++)
                    {
                        Board[yy, x] = Board[yy - 1, x];
                    }
                }
                for (int x = 0; x < Width; x++)
                {
                    Board[0, x] = '.';
                }
                y++;
            }
        }
    }
}


```