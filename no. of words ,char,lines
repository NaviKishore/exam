%{
#include <stdio.h>
int charCount = 0;
int lineCount = 0;
int wordCount = 0;
%}

%option noyywrap

%%

[ \t]+  ;  // Ignore spaces and tabs
\n      { lineCount++; }  // Count new lines

"/*"    {
            while(1) {
                char c = input();
                if (c == EOF) {
                    fprintf(stderr, "Error: Unterminated comment\n");
                    exit(1);
                }
                if (c == '*' && input() == '/') {
                    break;
                }
            }
        }

"//"    {
            while(input() != '\n');
            lineCount++;
        }

[a-zA-Z_][a-zA-Z0-9_]*    { wordCount++; }

.       { charCount++; }

%%

int main(int argc, char *argv[]) {
    if (argc != 2) {
        printf("Usage: %s <input_file>\n", argv[0]);
        return 1;
    }

    FILE *fp = fopen(argv[1], "r");
    if (fp == NULL) {
        perror("Error opening file");
        return 1;
    }

    yyin = fp;
    yylex();

    fclose(fp);

    printf("Number of characters: %d\n", charCount);
    printf("Number of lines: %d\n", lineCount);
    printf("Number of words: %d\n", wordCount);

    return 0;
}
