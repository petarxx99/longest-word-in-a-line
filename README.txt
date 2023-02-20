I have developped a program which finds the longest word of each line in a file. While doing so I have built a StringBuilder struct and a VectorString struct. 
I have used dependency injection and inversion of control by passing function pointers that read/write from a file, read from user input, calculate the longest word of each line. I have separated the processes of reading user input, reading from a file and calculating the longest word in each line. I wanted to implement separation of concerns and hexagonal architecture. 
Here is the main function of the code which finds the longest word of each line. I have put almost everything on the heap even though that is not neccessary.

VectorString* getLongestWordsInEachLine(char *text){
    VectorString *wordsOfALine = VectorString_new();
    VectorString *longestWordsOfLines = VectorString_new();
    StringBuilder *currentWord = StringBuilder_new();

    bool previousCharWasALetter = false;
    char nextCharacter;
    int i=0;
    while((nextCharacter=text[i]) > 0){
        if(isALetter(nextCharacter)){
            StringBuilder_appendChar(currentWord, nextCharacter);
            previousCharWasALetter = true;
        } else if(previousCharWasALetter){ /* This character isn't a letter, but the previous one was, therefore we have reached the end of a word.*/
            VectorString_addString(wordsOfALine, currentWord);
            StringBuilder_reset(currentWord);
            previousCharWasALetter = false;
        }

        if(nextCharacter == END_OF_LINE_CHARACTER){
            StringBuilder *longestWordOfThisLine = VectorString_findTheLongestString(wordsOfALine);
            VectorString_addString(longestWordsOfLines, longestWordOfThisLine);
            StringBuilder_delete(&longestWordOfThisLine);
            VectorString_delete(&wordsOfALine);
            wordsOfALine = VectorString_new();
        }
        i++;
    }

    VectorString_delete(&wordsOfALine);
    return longestWordsOfLines;
}
