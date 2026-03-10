#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>

#define N=9

void printBoard(int board[N][N]) {
    printf("\n");
    for (int r = 0; r < N; r++) {
        for (int c = 0; c < N; c++) {
            if (board[r][c] == 0) {
                printf(" ."); 
            }
            else {
                printf("%2d", board[r][c]);
            }

            if ((c + 1) % 3 == 0 && c < 8) printf(" |");
        }
        printf("\n");

        if ((r + 1) % 3 == 0 && r < 8) {
            printf("-----------------------\n");
        }
    }
    printf("\n");
}

int isSafe(int board[N][N], int row, int col, int num) {
    for (int x = 0; x < N; x++) {
        if (board[row][x] == num || board[x][col] == num) {
            return 0; 
        }
    }

    int startRow = row - row % 3;
    int startCol = col - col % 3;
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            if (board[i + startRow][j + startCol] == num) {
                return 0; 
            }
        }
    }
    return 1;
}

int isGameOver(int board[N][N]) {
    for (int r = 0; r < N; r++) {
        for (int c = 0; c < N; c++) {
            if (board[r][c] == 0) return 0; 
        }
    }
    return 1; 
}

int main() {
    // Startowa plansza (0 oznacza puste pole)
    int board[N][N] = {
        {5, 3, 0, 0, 7, 0, 0, 0, 0},
        {6, 0, 0, 1, 9, 5, 0, 0, 0},
        {0, 9, 8, 0, 0, 0, 0, 6, 0},
        {8, 0, 0, 0, 6, 0, 0, 0, 3},
        {4, 0, 0, 8, 0, 3, 0, 0, 1},
        {7, 0, 0, 0, 2, 0, 0, 0, 6},
        {0, 6, 0, 0, 0, 0, 2, 8, 0},
        {0, 0, 0, 4, 1, 9, 0, 0, 5},
        {0, 0, 0, 0, 8, 0, 0, 7, 9}
    };

    int initialBoard[N][N];
    for (int r = 0; r < N; r++) {
        for (int c = 0; c < N; c++) {
            initialBoard[r][c] = board[r][c];
        }
    }

    int row, col, num;

    printf("---SUDOKU---\n");
    printf("Zasady: Wypelnij plansze cyframi 1-9. Uzyj 0, aby wyczyscic pole.\n");

    // Główna pętla gry
    while (!isGameOver(board)) {
        printBoard(board);

        printf("Podaj wiersz (1-9), kolumne (1-9) i cyfre (0-9) oddzielone spacja (np. 1 5 3): ");
        if (scanf("%d %d %d", &row, &col, &num) != 3) {
            printf("[!] Blad formatu. Sprobuj ponownie.\n");
            while (getchar() != '\n');
            continue;
        }

        row--;
        col--;

        if (row < 0 || row >= N || col < 0 || col >= N || num < 0 || num > 9) {
            printf("[!] Nieprawidlowe wartosci! Uzyj liczb z odpowiednich zakresow.\n");
            continue;
        }

        if (initialBoard[row][col] != 0) {
            printf("[!] Tego pola nie mozna nadpisac, jest czescia startowej planszy.\n");
            continue;
        }

        if (num == 0) {
            board[row][col] = 0;
            printf("[+] Pole [%d, %d] wyczyszczone.\n", row + 1, col + 1);
        }
        else {
            int temp = board[row][col];
            board[row][col] = 0;

            if (isSafe(board, row, col, num)) {
                board[row][col] = num;
                printf("[+] Liczba %d dodana na pozycje [%d, %d].\n", num, row + 1, col + 1);
            }
            else {
                board[row][col] = temp; 
                printf("[-] Zly ruch! Liczba %d koliduje z inna w wierszu, kolumnie lub kwadracie.\n", num);
            }
        }
    }


    printBoard(board);
    printf("Gratulacje! \n");

    return 0;
}
