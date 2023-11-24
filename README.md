# Tic-Tac-Toe






using System;
namespace TicTaeToe2
{
    class Program
    {
        const int BOARD_SIZE = 3;
        const char EMPTY_CELL = ' ';
        const char PLAYER_X = 'X';
        const char PLAYER_O = 'O';

        static char[,] board = new char[BOARD_SIZE, BOARD_SIZE];
        static char currentPlayer = PLAYER_X;

        static void Main(string[] args)
        {
        InitializeBoard();

            do
            {
                Console.Clear();
                PrintBoard();
                MakeMove();
            } while (!IsGameFinished());


            Console.Clear();
            PrintBoard();

            if (CheckWin())
            {
                Console.WriteLine($"Играч {currentPlayer} печели!");
            }
            else
            {
                Console.WriteLine("Равенство!");
            }
        }

        static void InitializeBoard()
        {
            for (int i = 0; i < BOARD_SIZE; i++)
            {
                for (int j = 0; j < BOARD_SIZE; j++)
                {
                    board[i, j] = EMPTY_CELL;
                }
            }
        }

        static void PrintBoard()
        {
            for (int i = 0; i < BOARD_SIZE; i++)
            {
                for (int j = 0; j < BOARD_SIZE; j++)
                {
                    Console.Write($"{board[i, j]}");
                    if (j < BOARD_SIZE - 1)
                    {
                        Console.Write(" | ");
                    }
                }
                Console.WriteLine();
                if (i < BOARD_SIZE - 1)
                {
                    Console.WriteLine(new string('-', BOARD_SIZE * 4 - 1));
                }
            }
        }

        static void MakeMove()
        {
            Console.WriteLine($"Играч {currentPlayer}, въведете ред (от 1 до {BOARD_SIZE}) за вашия ход:");
            int row = int.Parse(Console.ReadLine()) - 1;

            Console.WriteLine($"Играч {currentPlayer}, въведете колона (от 1 до {BOARD_SIZE}) за вашия ход:");
            int col = int.Parse(Console.ReadLine()) - 1;

            if (IsValidMove(row, col))
            {
                board[row, col] = currentPlayer;
                SwitchPlayer();
            }
            else
            {
                Console.WriteLine("Невалиден ход. Опитайте отново.");
                Console.ReadKey(); // Позволява на играча види съобщението преди следващия ход.
            }
        }

        static bool IsValidMove(int row, int col)
        {
            return row >= 0 && row < BOARD_SIZE && col >= 0 && col < BOARD_SIZE && board[row, col] == EMPTY_CELL;
        }

        static void SwitchPlayer()
        {
            currentPlayer = (currentPlayer == PLAYER_X) ? PLAYER_O : PLAYER_X;
        }

        static bool IsGameFinished()
        {
            return CheckWin() || IsBoardFull();
        }

        static bool CheckWin()
        {
            return (CheckRows() || CheckColumns() || CheckDiagonals());
        }

        static bool CheckRows()
        {
            for (int i = 0; i < BOARD_SIZE; i++)
            {
                if (AreEqual(board[i, 0], board[i, 1], board[i, 2]))
                {
                    return true;
                }
            }
            return false;
        }

        static bool CheckColumns()
        {
            for (int i = 0; i < BOARD_SIZE; i++)
            {
                if (AreEqual(board[0, i], board[1, i], board[2, i]))
                {
                    return true;
                }
            }
            return false;
        }

        static bool CheckDiagonals()
        {
            return (AreEqual(board[0, 0], board[1, 1], board[2, 2]) ||
                    AreEqual(board[0, 2], board[1, 1], board[2, 0]));
        }

        static bool AreEqual(char a, char b, char c)
        {
            return a == b && b == c && a != EMPTY_CELL;
        }

        static bool IsBoardFull()
        {
            for (int i = 0; i < BOARD_SIZE; i++)
            {
                for (int j = 0; j < BOARD_SIZE; j++)
                {
                    if (board[i, j] == EMPTY_CELL)
                    {
                        return false;
                    }
                }
            }
            return true;
        }
    }
}
