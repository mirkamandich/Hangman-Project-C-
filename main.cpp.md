// Programming-and-Data-Structures
// Hangman Project
// main.cpp

// Last updated: 1/3/2017


#include <iostream>
#include <string>
#include <cctype>

#include "hangman.h"

using namespace std;

int main()
{
  Hangman game;
  char letter;
  char response;
  bool gameOver = false;
  bool gameWon = false;
  // string filename = "/home/fac/sreeder/class/cs1430/p1input.dat";
  string filename;

  //  if(!game.initializeFile(filename)){
  //    cout << "There was a problem with the file. Exiting..." << endl;
  //    return 0;
  // }

  game.welcomeStatement();

  cout << "Would you like to play? (y/n)" << endl;
  cin >> response;
  response = tolower(response);

  if(response == 'y'){
    cout << "\nPlease enter the filename:" << endl;
    cin >> filename;

    if (!game.initializeFile(filename)){
      cout << "There was a problem with the file. Exiting..." << endl;
      return 0;
    }
  }
while(response == 'y' && game.newWord() ){
    game.displayStatistics();
    cout << endl;
    game.displayGame();
    gameOver = false;
    gameWon = false;
    while (!gameOver){
      cout << "Which letter? ";
      cin >> letter;
      game.guess(toupper(letter), gameOver, gameWon);
      game.displayGame();
    }
    game.revealWord();
    if (gameWon){
      cout << "You won!\n";
      game.displayStatistics();
      cout << endl << endl;
    }
    cout << "Would you like to try again? (y/n)" << endl;
    cin >> response;
  }
  game.goodbyeStatement();
  return 0;
}	
