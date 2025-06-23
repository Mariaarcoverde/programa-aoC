#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// Definições das peças
#define EMPTY ' '
#define PAWN 'P'
#define ROOK 'R'
#define KNIGHT 'N'
#define BISHOP 'B'
#define QUEEN 'Q'
#define KING 'K'

typedef struct {
    char piece;
    char color; // 'W' ou 'B'
} Square;

Square board[8][8];

void initBoard() {
    int i;

    // Limpar o tabuleiro
    for (int y = 2; y < 6; y++)
        for (int x = 0; x < 8; x++)
            board[y][x].piece = EMPTY;

    // Peões
    for (i = 0; i < 8; i++) {
        board[1][i].piece = PAWN;
        board[1][i].color = 'W';
        board[6][i].piece = PAWN;
        board[6][i].color = 'B';
    }

    // Primeira e última fileira
    char pieces[] = {ROOK, KNIGHT, BISHOP, QUEEN, KING, BISHOP, KNIGHT, ROOK};
    for (i = 0; i < 8; i++) {
        board[0][i].piece = pieces[i];
        board[0][i].color = 'W';
        board[7][i].piece = pieces[i];
        board[7][i].color = 'B';
    }
}

void printBoard() {
    printf("  a b c d e f g h\n");
    for (int y = 7; y >= 0; y--) {
        printf("%d ", y + 1);
        for (int x = 0; x < 8; x++) {
            if (board[y][x].piece == EMPTY)
                printf(". ");
            else if (board[y][x].color == 'W')
                printf("%c ", board[y][x].piece);
            else
                printf("%c ", board[y][x].piece + 32); // minúsculas para pretas
        }
        printf("%d\n", y + 1);
    }
    printf("  a b c d e f g h\n");
}

int movePiece(char from[3], char to[3]) {
    int fx = from[0] - 'a';
    int fy = from[1] - '1';
    int tx = to[0] - 'a';
    int ty = to[1] - '1';

    if (fx < 0 || fx > 7 || fy < 0 || fy > 7 || tx < 0 || tx > 7 || ty < 0 || ty > 7)
        return 0;

    Square fromSquare = board[fy][fx];
    if (fromSquare.piece == EMPTY)
        return 0;

    board[ty][tx] = fromSquare;
    board[fy][fx].piece = EMPTY;
    board[fy][fx].color = 0;

    return 1;
}

int main() {
    char from[3], to[3];
    initBoard();

    while (1) {
        printBoard();
        printf("Digite o movimento (ex: e2 e4): ");
        scanf("%s %s", from, to);

        if (!movePiece(from, to)) {
            printf("Movimento inválido!\n");
        }
    }

    return 0;
}
