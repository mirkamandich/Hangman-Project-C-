// Programming-and-Data-Structures
// Hangman Project
// hangman.cpp

// Last updated: 1/3/2017


#include "hangman.h"

Hangman::Hangman()
{
  totalWords = 0;

  // Noose pieces
  body[0].firstLine = "    ----\n";
  body[1].firstLine = "    |  |\n";
  body[2].firstLine = "       |\n";
  body[2].secondLine = "    O  |\n";
  body[3].firstLine = "       |\n";
  body[3].secondLine = "    |  |\n";
  body[4].firstLine = "       |\n";
  body[4].secondLine = "   -|  |\n";
  body[4].thirdLine = "   -|- |\n";
  body[5].firstLine = "       |\n";
  body[5].secondLine = "    |  |\n";
  body[6].firstLine = "       |\n";
  body[6].secondLine = "   /   |\n";
  body[6].thirdLine = "   / \\ |\n";
  body[7].firstLine = "       |\n";
  body[8].firstLine = "_______|___\n";

  // Set up noose
  for (int i = 0; i < BODY_SIZE; i++){
    body[i].displaySecond = false;
    body[i].displayThird = false;
  }

  char letter = BEGIN;
  for (int i = 0; i < ALPHA_SIZE; i++){
    alphabet[i].letter = letter;
    letter++;
    alphabet[i].used = false;
  }

  currentWord = -1;
currentWord = -1;
  totalWords = 0;
  wins = 0;
  losses = 0;
  correctChars = 0;
  badGuesses = 0;

  srand(time(0)); // Initialize random
}

bool Hangman::initializeFile(string filename)
{
  ifstream inFile;
  inFile.open(filename.c_str());

  if (inFile.fail())
    return false;

  while (inFile >> list[totalWords].word){
    list[totalWords].used = false;
    totalWords++;
  }

  inFile.close();
  return true;
}

void Hangman::displayStatistics()
{
  cout << "GAME STATISTICS" << endl;;
  cout << "Words read from file: " << totalWords << endl;
  cout << "Words available: " << totalWords - wins - losses << endl;
  cout << "Games Won: " << wins << endl;
  cout << "Games Lost: " << losses << endl;
}

bool Hangman::newWord() // pick a new word
{
  bool done = false;
  int newIndex = 0;

  if (wins + losses == totalWords)
   return false;

  while (!done){
    newIndex = rand() % totalWords;
    if (!list[newIndex].used){
      currentWord = newIndex;
      list[newIndex].used = true;
      done = true;
    }
  }

  // reset hangman
  for (int i = 0; i < BODY_SIZE; i++){
    body[i].displaySecond = false;
    body[i].displayThird = false;
  }

  // reset alphabet
  for (int i = 0; i < ALPHA_SIZE; i++){
    alphabet[i].used = false;
  }

  // reset character count
  correctChars = 0;
  badGuesses = 0;

  return true;
}

void Hangman::displayBody()
{
  for(int i = 0; i<9; i++){
    if(body[i].displayThird)
      cout << body[i].thirdLine;
    else if(body[i].displaySecond)
      cout << body[i].secondLine;
    else
      cout << body[i].firstLine;
  }
  cout << endl;
}

void Hangman::displayWord()
{
  for(unsigned i = 0; i < list[currentWord].word.length(); i++){
    if(alphabet[list[currentWord].word[i] - BEGIN].used)
      cout << list[currentWord].word[i];
    else
      cout << "_ ";
  }
  cout << endl;
}

void Hangman::displayAlpha()
{
  for(int i = 0; i<26 ; i++){
  if(alphabet[i].used) // if letter has been used, remove from display
    cout << "_ ";
  else
    cout << alphabet[i].letter << " ";
  }
void Hangman::displayGame()
{
  cout << endl;
  displayBody();
  cout << endl;
  displayWord();
  cout << endl;
  displayAlpha();
  cout << endl;
}

bool Hangman::guess(char letter, bool& done, bool& won)
{
  int letterIndex = letter - BEGIN;
  bool letterFound = false;
  if (!isalpha(letter))
    return false;

  letter = toupper(letter);

  if(alphabet[letterIndex].used)
    return false;

  alphabet[letterIndex].used = true;

  for(unsigned i = 0; i<list[currentWord].word.length(); i++ ){
    if(list[currentWord].word[i] == letter){
      letterFound = true;
      correctChars++;
    }
  }
  if(!letterFound)
    badGuesses++;

  if (correctChars==list[currentWord].word.length()){
    done = true;
    won = true;
    wins++;
  }

  if (badGuesses == MAX_BAD_GUESSES){
    done = true;
    losses++;
  }

  switch(badGuesses){
  case HEAD:
    body[2].displaySecond = true;
    break;
  case NECK:
    body[3].displaySecond = true;
    break;
  case RIGHT_ARM:
    body[4].displaySecond = true;
    break;
  case LEFT_ARM:
    body[4].displayThird = true;
    break;
case TORSO:
    body[5].displaySecond = true;
    break;
  case RIGHT_LEG:
    body[6].displaySecond = true;
    break;
  case LEFT_LEG:
    body[6].displayThird = true;
    break;
  }
  return true;
}

void Hangman::revealWord()
{
  cout << "The word is: " << list[currentWord].word << endl;
  cout << "Thank you for playing!" << endl << endl << endl;
}
void Hangman::welcomeStatement()
{
  cout << "\n\n\nWelcome to Hangman!\n\n"
       << "In this game, you will be given a mystery word \n"
       << "that you will try to guess letter by letter.\n"
       << "If you make " << MAX_BAD_GUESSES << " bad guesses, you "
       << "lose and your \nstickman gets hanged." << endl << endl;
}

void Hangman::goodbyeStatement()
{
  cout << "\nGoodbye!" << endl << endl << endl;
}
