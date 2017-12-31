// Programming-and-Data-Structures
// Hangman Project
// hangman.h

// Last updated: 1/3/2017


#include <iostream>
#include <string>
#include <fstream>
#include <ctime>
#include <cstdlib>
#include <cctype>

using namespace std;

#ifndef HANGMAN_H
#define HANGMAN_H

class Hangman
{
 public:
  Hangman();
  bool initializeFile(string filename);
  bool newWord();
  bool guess(char letter, bool& done, bool& won);
  void displayStatistics();
  void displayGame();
  void revealWord();
  void welcomeStatement();
  void goodbyeStatement();


 private:

  static const int BODY_SIZE = 9;
  static const int MAX_WORDS = 100;
  static const int ALPHA_SIZE = 26;
  static const char YES = 'y';
  static const char BEGIN = 'A';
  static const int MAX_BAD_GUESSES = 7;
  static const int HEAD = 1;
  static const int NECK = 2;
  static const int RIGHT_ARM = 3;
  static const int LEFT_ARM = 4;
static const int TORSO = 5;
  static const int RIGHT_LEG = 6;
  static const int LEFT_LEG = 7;

  struct WordList{
    string word;
    bool used;
  };
  WordList list[MAX_WORDS];

  struct BodyPart{
    string firstLine;
    string secondLine;
    string thirdLine;
    bool displaySecond;
    bool displayThird;
  };
  BodyPart body[BODY_SIZE];

  struct AlphaList{
    char letter;
    bool used;
  };
  AlphaList alphabet[ALPHA_SIZE];

  string words[MAX_WORDS];
  char alpha[ALPHA_SIZE];
  int totalWords;
  int currentWord;
  int wins;
  int losses;
  unsigned correctChars;
  unsigned badGuesses;

  void displayBody();
  void displayWord();
  void displayAlpha();

};

#endif // HANGMAN_H
