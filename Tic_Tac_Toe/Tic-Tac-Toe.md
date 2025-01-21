# Calculator Program

A CLI implementation of Tic-Tac-Toe

### Features 
  - Interactive command-line interface with visual game board display
  - Comprehensive input validation with clear error messages
  - Turn-based gameplay alternating between X and O players
  - Automatic win detection for rows, columns, and diagonals
  - Draw detection when the board is full
  
### Usage
Players take turns entering numbers 1-9 to place their mark (X or O) on the board:
```
                                                        1 | 2 | 3
                                                        ---------
                                                        4 | 5 | 6
                                                        ---------
                                                        7 | 8 | 9 
```

## C++ Code
```cpp

#include <iostream>
#include <fstream>
#include <vector>
#include <algorithm>
#include <array>

using namespace std;

void trim(string& input)
{
    input.erase(0, input.find_first_not_of(" \t\n\r"));
    input.erase(input.find_last_not_of(" \t\n\r") + 1);
}

void displayInfo()
{
    cout << "***************************\n";
    cout << "TIC-TAC-TOE Program\n"
         << "Version 1.0\n"
         << "Copyright (c) Ford Culallad\n";
    cout << "***************************\n\n";
}

enum class InputError
{
    Input_Not_Received,
    Number_Out_Of_Range,
    Not_A_Number,
    Position_Occupied
};

class TicTacToe
{
    public :
        TicTacToe()
        {
            char m_defaultValue{' '};
            for(int row = 0; row < 3; row++)
            {
                for(int column = 0; column < 3; column++)
                {
                    m_board[row][column] = m_defaultValue;
                }
            }
            m_currentPlayer = 'X';
        }

        string getErrorMessage(InputError error)
        {
            switch(error)
                {
                    case InputError::Input_Not_Received : 
                        return "\nERROR : Input not received.\n";

                    case InputError::Number_Out_Of_Range : 
                        return "\n\nERROR : Number Out of Range. Try Again. \n";

                    case InputError::Not_A_Number : 
                        return "\nERROR : Input must be a number.\n";

                    case InputError::Position_Occupied : 
                        return "\nERROR : Position is Already Occupied. Try Again.\n";
                }
        }

        void displayBoard()
        {
            cout << "\n";
            for(int row = 0; row < 3; row++)
            {
                for(int column = 0; column < 3; column++)
                {
                    cout << m_board[row][column];
                    if(column < 2) {cout << " | ";}
                }
                cout << "\n";
                if(row < 2){cout << "----------\n";}
            }
        }

        void displayUsage()
        {
            cout << "USAGE : Enter a number from (1 - 9) to play.\n";
        }

        void displayCurrentPlayer()
        {
            cout << "\nCurrent Player : " << getCurrentPlayer() << '\n';
        }

        char getCurrentPlayer(){return m_currentPlayer;}

        bool makeMove(int position)
        {
            int row = (position - 1) / 3;
            int column = (position - 1) % 3;

            if(m_board[row][column] == 'X' || m_board[row][column] == 'O')
            {
                return false;
            }
            
            m_board[row][column] = m_currentPlayer;
            return true;
        }

        bool checkWin()
        {
            //Check Row
            for(int i = 0; i < 3; i++)
            {
                if(m_board[i][0] != ' ' &&
                   m_board[i][0] == m_board[i][1] &&
                   m_board[i][1] == m_board[i][2])
                {
                    return true;
                }
            }

            //Check Column
            for(int i = 0; i < 3; i++)
            {
                if(m_board[0][i] != ' ' &&
                   m_board[0][i] == m_board[1][i] &&
                   m_board[1][i] == m_board[2][i])
                {
                    return true;
                }
            }

            //Check Diagonals
            if(m_board[0][0] != ' ' &&
               m_board[0][0] == m_board[1][1] &&
               m_board[1][1] == m_board[2][2])
            {
                return true;
            }

            if(m_board[0][2] != ' ' &&
               m_board[0][2] == m_board[1][1] &&
               m_board[1][1] == m_board[2][0])
            {
                return true;
            }
            
            return false;
        }

        void switchPlayer()
        {
            if(m_currentPlayer == 'X')
            {
                m_currentPlayer = 'O';
            }
            else
            {
                m_currentPlayer = 'X';
            }
        }

        bool isBoardFull()
        {
            for(int row = 0; row < 3; row++)
            {
                for(int column = 0; column < 3; column++)
                {
                    if(m_board[row][column] != 'X' &&
                       m_board[row][column] != 'O')
                    {
                        return false;
                    }
                }
            }
            return true;
        }

        int getValidPositionFromUser()
        {
            string input{};
            int position{};
            
            while(1)
            {
                getline(cin, input);
                
                if(input.empty())
                {
                    cout << getErrorMessage(InputError::Input_Not_Received);
                    displayUsage();
                    displayCurrentPlayer();
                    cout << "Enter a Position : ";
                    continue;
                }
                trim(input);

                try 
                {
                    position = stoi(input);

                    if(position < 1 || position > 9)
                    {
                        cout << getErrorMessage(InputError::Number_Out_Of_Range);
                        displayUsage();
                        displayCurrentPlayer();
                        cout << "Enter a Position : ";
                        continue;
                    }
                    
                    break;
                } 
                catch(const invalid_argument&)
                {
                    cout << getErrorMessage(InputError::Not_A_Number);
                    displayUsage();
                    displayCurrentPlayer();
                    cout << "Enter a Position : ";
                    continue;
                }
            }
            return position;
        }

    private :
        char m_board[3][3];
        char m_currentPlayer{};
};

int main()
{
    TicTacToe game{};
    int position{};
    bool gameWon{false};
    displayInfo();

    cout << "Welcome to the Tic-Tac Toe Game!\n";
    cout << "USAGE : Enter a number from (1 - 9) to play.\n";

    //Game Loop
    while(!gameWon && !game.isBoardFull())
    {
        game.displayBoard();
        game.displayCurrentPlayer();
        cout << "Enter a Position : ";
        position = game.getValidPositionFromUser();
        
        if(!game.makeMove(position))
        {
            cout << game.getErrorMessage(InputError::Position_Occupied);
            game.displayUsage();
            continue;
        }

        if(game.checkWin())
        {
            game.displayBoard();
            cout << "\nPlayer " << game.getCurrentPlayer() << " Wins!!!\n";
            gameWon = true;
            break;
        }

        game.switchPlayer();
    }

    if(!gameWon && game.isBoardFull())
    {
        game.displayBoard();
        cout << "\nGame Draw!";
    }
    
    return 0;
}

```
