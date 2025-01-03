# Calculator Program

CLI Program that performs basic operations on integers

### Features 
  - Supports (+,-,*,/)
  - Integer Input Validation
  - Division by Zero Prevention
  - Division Resulting in Fractional Value Gets Rounded to The Nearest Integer

### Usage
 - Enter First Number
 - Choose Operator
 - Enter Second Number
 - View Result
 
## C++ Code

```cpp

#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
#include <fstream>
#include <filesystem>
#include <cmath>
#include <random>

using namespace std;

int validInput(){
  int number{};

  while(1)
    {
      if(!(cin >> number))
      {
        cout << "Enter a valid integer:\n";
        cin.clear();
        cin.ignore(1000,'\n');
        continue;
      }
      break;
    }
  cin.ignore(1000,'\n');
  return number; 
}

char validOperator()
{
    const string validOps = "+-*/";

    while (true) {
        string input;
        getline(cin, input);

        input.erase(0, input.find_first_not_of(" \t"));
        input.erase(input.find_last_not_of(" \t") + 1);

        if (input.length() == 1 && validOps.find(input[0]) != string::npos)
        {
            return input[0];
        }

        cout << "Invalid input. Please enter +, -, *, or / only.\n";
    }
}

int sum(int numOne, int numTwo){return numOne + numTwo;}
int subtract(int numOne, int numTwo){return numOne - numTwo;}
int multiply(int numOne, int numTwo){return numOne * numTwo;}
int division(int numerator, int &denominator, char op)
{
  while(1)
    {
      if(denominator == 0)
      {
        cout << "Cannot divide by zero. Try Again: \n";
        cin >> denominator;
        continue;
      }
      break;
    }

  double result = static_cast<double>(numerator) / denominator;

  return static_cast<int>(round(result));
}

int main()
{
  cout << "CALCULATOR\n";
  cout << "Usage: Provide two numbers and an operator\n";
  cout << "(+,-,*,/)\n";

  cout << "Enter Number One: ";
  int numOne{validInput()};

  cout << "Enter an Operator (+,-,*,/): ";
  char op{validOperator()};
  
  cout << "Enter Number Two: ";
  int numTwo{validInput()};


  int result{};

  switch(op)
    {
      case '+' : result = sum(numOne, numTwo); break;
      case '-' : result = subtract(numOne, numTwo); break;
      case '*' : result = multiply(numOne, numTwo); break;
      case '/' : result = division (numOne, numTwo, op); break;
    }

     cout << "\n";

    if(op == '/')
    {
      cout << "\n************************************************\n";
      cout << "**Result will be rounded up for decimal values**\n";
      cout << "************************************************\n\n";
    }
  
    cout << numOne << " " << op << " " << numTwo << " = " << result;

  return 0;
}

```