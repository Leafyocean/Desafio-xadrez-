#include <stdio.h>
#include <stdlib.h>

#define TAM 8

// Função para limpar o tabuleiro
void limparTabuleiro(int tabuleiro[TAM][TAM]) {
    for (int i = 0; i < TAM; i++) {
        for (int j = 0; j < TAM; j++) {
            tabuleiro[i][j] = 0;
        }
    }
}

// Função para exibir o tabuleiro
void mostrarTabuleiro(int tabuleiro[TAM][TAM], int x, int y) {
    printf("\nTabuleiro (posição da peça: %d,%d):\n", x, y);
    for (int i = 0; i < TAM; i++) {
        for (int j = 0; j < TAM; j++) {
            if (i == x && j == y) {
                printf(" P ");
            } else if (tabuleiro[i][j] == 1) {
                printf(" * ");
            } else {
                printf(" . ");
            }
        }
        printf("\n");
    }
}

// Torre: movimentos em linha reta (for)
void movimentoTorre(int tabuleiro[TAM][TAM], int x, int y) {
    for (int i = 0; i < TAM; i++) {
        if (i != x) tabuleiro[i][y] = 1;
        if (i != y) tabuleiro[x][i] = 1;
    }
}

// Bispo: movimentos em diagonal (while)
void movimentoBispo(int tabuleiro[TAM][TAM], int x, int y) {
    int i, j;
    
    i = x; j = y;
    while (++i < TAM && ++j < TAM) tabuleiro[i][j] = 1;

    i = x; j = y;
    while (--i >= 0 && --j >= 0) tabuleiro[i][j] = 1;

    i = x; j = y;
    while (++i < TAM && --j >= 0) tabuleiro[i][j] = 1;

    i = x; j = y;
    while (--i >= 0 && ++j < TAM) tabuleiro[i][j] = 1;
}

// Cavalo: movimento em L (loops aninhados)
void movimentoCavalo(int tabuleiro[TAM][TAM], int x, int y) {
    int movX[] = {2, 2, -2, -2, 1, 1, -1, -1};
    int movY[] = {1, -1, 1, -1, 2, -2, 2, -2};

    for (int i = 0; i < 8; i++) {
        int nx = x + movX[i];
        int ny = y + movY[i];
        if (nx >= 0 && nx < TAM && ny >= 0 && ny < TAM) {
            tabuleiro[nx][ny] = 1;
        }
    }
}

// Rainha: combina torre + bispo (recursivo)
void movimentoRainhaRecursivo(int tabuleiro[TAM][TAM], int x, int y, int dx, int dy) {
    int nx = x + dx;
    int ny = y + dy;

    if (nx >= 0 && nx < TAM && ny >= 0 && ny < TAM) {
        tabuleiro[nx][ny] = 1;
        movimentoRainhaRecursivo(tabuleiro, nx, ny, dx, dy);
    }
}

void movimentoRainha(int tabuleiro[TAM][TAM], int x, int y) {
    int direcoes[8][2] = {
        {1, 0}, {-1, 0}, {0, 1}, {0, -1},
        {1, 1}, {-1, -1}, {1, -1}, {-1, 1}
    };

    for (int i = 0; i < 8; i++) {
        movimentoRainhaRecursivo(tabuleiro, x, y, direcoes[i][0], direcoes[i][1]);
    }
}

int main() {
    int tabuleiro[TAM][TAM];
    int linha, coluna, opcao;

    do {
        printf("\n=== SIMULADOR DE PEÇAS DE XADREZ ===\n");
        printf("Informe a posição da peça (linha e coluna de 0 a 7): ");
        scanf("%d %d", &linha, &coluna);

        if (linha < 0 || linha >= TAM || coluna < 0 || coluna >= TAM) {
            printf("Posição inválida!\n");
            continue;
        }

        printf("\nEscolha a peça:\n");
        printf("1. Torre\n");
        printf("2. Bispo\n");
        printf("3. Cavalo\n");
        printf("4. Rainha\n");
        printf("0. Sair\n");
        printf("Opção: ");
        scanf("%d", &opcao);

        limparTabuleiro(tabuleiro);

        switch (opcao) {
            case 1:
                movimentoTorre(tabuleiro, linha, coluna);
                mostrarTabuleiro(tabuleiro, linha, coluna);
                break;
            case 2:
                movimentoBispo(tabuleiro, linha, coluna);
                mostrarTabuleiro(tabuleiro, linha, coluna);
                break;
            case 3:
                movimentoCavalo(tabuleiro, linha, coluna);
                mostrarTabuleiro(tabuleiro, linha, coluna);
                break;
            case 4:
                movimentoRainha(tabuleiro, linha, coluna);
                mostrarTabuleiro(tabuleiro, linha, coluna);
                break;
            case 0:
                printf("Encerrando o programa...\n");
                break;
            default:
                printf("Opção inválida!\n");
        }
    } while (opcao != 0);

    return 0;
}
