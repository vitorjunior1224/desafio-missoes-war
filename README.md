# desafio-missoes-war
main.c
/*
    Implementação das funções do jogo de missões estratégicas
    Autor: Júnior Souza
*/

#include "funcoes.h"

// --------------------------------------------------------
// FUNÇÃO: atribuirMissao
// Descrição: Sorteia aleatoriamente uma missão e copia para o jogador
// --------------------------------------------------------
void atribuirMissao(char* destino, char* missoes[], int totalMissoes) {
    int indice = rand() % totalMissoes;
    strcpy(destino, missoes[indice]);
}

// --------------------------------------------------------
// FUNÇÃO: verificarMissao
// Descrição: Verifica se a missão foi cumprida (lógica simples inicial)
// --------------------------------------------------------
int verificarMissao(char* missao, Territorio* mapa, int tamanho) {

    // Exemplo de verificação simples
    if (strcmp(missao, "Eliminar todas as tropas da cor vermelha") == 0) {
        for (int i = 0; i < tamanho; i++) {
            if (strcmp(mapa[i].cor, "vermelho") == 0) {
                return 0; // Ainda há inimigos vermelhos
            }
        }
        return 1; // Missão concluída
    }

    if (strcmp(missao, "Conquistar todos os territorios verdes") == 0) {
        int conquistados = 0;
        for (int i = 0; i < tamanho; i++) {
            if (strcmp(mapa[i].cor, "verde") == 0)
                conquistados++;
        }
        if (conquistados == tamanho) return 1;
    }

    return 0; // Missão não concluída
}

// --------------------------------------------------------
// FUNÇÃO: exibirMissao
// Descrição: Mostra a missão ao jogador no início do jogo
// --------------------------------------------------------
void exibirMissao(char* missao) {
    printf("\n=============================\n");
    printf("SUA MISSAO: %s\n", missao);
    printf("=============================\n\n");
}

// --------------------------------------------------------
// FUNÇÃO: atacar
// Descrição: Simula uma batalha entre dois territórios
// --------------------------------------------------------
void atacar(Territorio* atacante, Territorio* defensor) {

    if (strcmp(atacante->cor, defensor->cor) == 0) {
        printf("Nao pode atacar territorios da mesma cor!\n");
        return;
    }

    int dadoAtacante = rand() % 6 + 1;
    int dadoDefensor = rand() % 6 + 1;

    printf("\nAtaque: %s (dado %d) VS %s (dado %d)\n",
           atacante->nome, dadoAtacante, defensor->nome, dadoDefensor);

    if (dadoAtacante > dadoDefensor) {
        printf("%s conquistou o territorio %s!\n", atacante->nome, defensor->nome);
        strcpy(defensor->cor, atacante->cor);
        defensor->tropas = atacante->tropas / 2;
    } else {
        atacante->tropas--;
        printf("%s perdeu uma tropa!\n", atacante->nome);
    }
}

// --------------------------------------------------------
// FUNÇÃO: exibirMapa
// Descrição: Mostra todos os territórios e seus estados
// --------------------------------------------------------
void exibirMapa(Territorio* mapa, int tamanho) {
    printf("\n--- MAPA ATUAL ---\n");
    for (int i = 0; i < tamanho; i++) {
        printf("%s | Cor: %s | Tropas: %d\n",
               mapa[i].nome, mapa[i].cor, mapa[i].tropas);
    }
    printf("------------------\n");
}

// --------------------------------------------------------
// FUNÇÃO: liberarMemoria
// Descrição: Libera a memória alocada dinamicamente
// --------------------------------------------------------
void liberarMemoria(char* missaoJogador, Territorio* mapa) {
    free(missaoJogador);
    free(mapa);
}
