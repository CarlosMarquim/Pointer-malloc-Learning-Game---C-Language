# 13 - Complete Game

# Jogo dos Ponteiros, malloc, realloc e Áudio em Raylib (C)

# Objetivo da Aula

Nesta aula iremos juntar TODOS os principais conceitos aprendidos até agora:

- janela
- FPS
- input
- colisão
- structs
- ponteiros
- heap
- malloc
- realloc
- free
- música
- efeitos sonoros
- gameplay loop

Agora estamos criando:

```text
um jogo completo com memória dinâmica e áudio
```

---

# O que esse jogo representa

Este projeto simula:

```text
um sistema real de entidades dinâmicas
```

Além disso:
- possui música de fundo
- possui efeitos sonoros
- aumenta memória dinamicamente
- cria gameplay completo

Esse tipo de arquitetura já se aproxima de:
- mini engines
- jogos arcade
- sistemas ECS básicos

---

# Arquivos necessários

Coloque estes arquivos na mesma pasta do projeto:

```text
fundo.ogg
coleta.wav
```

---

# Código Completo

```c
// =========================================
// IMPORTS
// =========================================

// Biblioteca gráfica Raylib
#include "raylib.h"

// Biblioteca de memória dinâmica
#include <stdlib.h>

// Biblioteca de printf
#include <stdio.h>

// =========================================
// STRUCT BLOCO DE MEMÓRIA
// =========================================

// Estrutura de cada bloco
typedef struct BlocoMemoria {

    // Guarda posição X e Y
    Vector2 posicao;

    // Guarda tamanho do bloco
    float tamanho;

    // Define se bloco está ativo
    bool ativo;

} BlocoMemoria;

// =========================================
// MAIN
// =========================================

int main(void)
{
    // =========================================
    // JANELA E ÁUDIO
    // =========================================

    // Cria janela
    InitWindow(800, 450, "Jogo dos Ponteiros e malloc");

    // Inicializa sistema de áudio
    InitAudioDevice();

    // Define FPS
    SetTargetFPS(60);

    // =========================================
    // MÚSICA E EFEITOS SONOROS
    // =========================================

    // Carrega música de fundo
    Music musica =
        LoadMusicStream("fundo.ogg");

    // Carrega efeito sonoro
    Sound coleta =
        LoadSound("coleta.wav");

    // Inicia música
    PlayMusicStream(musica);

    // =========================================
    // JOGADOR
    // =========================================

    // Posição inicial do jogador
    Vector2 jogador = { 400, 225 };

    // Velocidade do jogador
    float velocidade = 4.0f;

    // =========================================
    // CONTROLE DE MEMÓRIA
    // =========================================

    // Capacidade máxima inicial
    int capacidade = 5;

    // Quantidade atual
    int quantidade = 5;

    // Pontuação
    int pontos = 0;

    // =========================================
    // MALLOC
    // =========================================

    // malloc:
    // cria vetor dinâmico na heap
    BlocoMemoria *blocos =
        malloc(capacidade *
               sizeof(BlocoMemoria));

    // Verifica erro de memória
    if (blocos == NULL)
    {
        // Mostra erro
        printf("Erro ao alocar memoria!\n");

        // Fecha áudio
        CloseAudioDevice();

        // Fecha janela
        CloseWindow();

        return 1;
    }

    // =========================================
    // INICIALIZA BLOCOS
    // =========================================

    for (int i = 0; i < quantidade; i++)
    {
        // Posição X aleatória
        blocos[i].posicao.x =
            GetRandomValue(50, 750);

        // Posição Y aleatória
        blocos[i].posicao.y =
            GetRandomValue(50, 400);

        // Tamanho
        blocos[i].tamanho = 25;

        // Ativa bloco
        blocos[i].ativo = true;
    }

    // =========================================
    // GAME LOOP
    // =========================================

    while (!WindowShouldClose())
    {
        // =========================================
        // UPDATE DA MÚSICA
        // =========================================

        // Atualiza streaming da música
        UpdateMusicStream(musica);

        // =========================================
        // INPUT
        // =========================================

        // Move direita
        if (IsKeyDown(KEY_RIGHT))
            jogador.x += velocidade;

        // Move esquerda
        if (IsKeyDown(KEY_LEFT))
            jogador.x -= velocidade;

        // Move cima
        if (IsKeyDown(KEY_UP))
            jogador.y -= velocidade;

        // Move baixo
        if (IsKeyDown(KEY_DOWN))
            jogador.y += velocidade;

        // =========================================
        // HITBOX DO JOGADOR
        // =========================================

        Rectangle rectJogador =
        {
            jogador.x,
            jogador.y,
            30,
            30
        };

        // =========================================
        // COLISÃO
        // =========================================

        for (int i = 0; i < quantidade; i++)
        {
            // Verifica se bloco está ativo
            if (blocos[i].ativo)
            {
                // Hitbox do bloco
                Rectangle rectBloco =
                {
                    blocos[i].posicao.x,
                    blocos[i].posicao.y,
                    blocos[i].tamanho,
                    blocos[i].tamanho
                };

                // Detecta colisão
                if (CheckCollisionRecs(
                        rectJogador,
                        rectBloco))
                {
                    // Desativa bloco
                    blocos[i].ativo = false;

                    // Aumenta pontuação
                    pontos++;

                    // Toca som de coleta
                    PlaySound(coleta);
                }
            }
        }

        // =========================================
        // REALLOCAÇÃO DINÂMICA
        // =========================================

        // Quando todos forem coletados
        if (pontos == quantidade)
        {
            // Aumenta capacidade
            capacidade += 5;

            // Aumenta quantidade
            quantidade += 5;

            // realloc:
            // aumenta memória dinamicamente
            BlocoMemoria *novoEndereco =
                realloc(blocos,
                        capacidade *
                        sizeof(BlocoMemoria));

            // Verifica erro
            if (novoEndereco == NULL)
            {
                printf("Erro ao realocar memoria!\n");

                // Libera memória antiga
                free(blocos);

                // Libera som
                UnloadSound(coleta);

                // Libera música
                UnloadMusicStream(musica);

                // Fecha áudio
                CloseAudioDevice();

                // Fecha janela
                CloseWindow();

                return 1;
            }

            // Atualiza ponteiro
            blocos = novoEndereco;

            // Inicializa novos blocos
            for (int i = pontos;
                 i < quantidade;
                 i++)
            {
                // Nova posição X
                blocos[i].posicao.x =
                    GetRandomValue(50, 750);

                // Nova posição Y
                blocos[i].posicao.y =
                    GetRandomValue(50, 400);

                // Tamanho
                blocos[i].tamanho = 25;

                // Ativa bloco
                blocos[i].ativo = true;
            }
        }

        // =========================================
        // RENDERIZAÇÃO
        // =========================================

        // Inicia renderização
        BeginDrawing();

        // Limpa fundo
        ClearBackground(RAYWHITE);

        // Título
        DrawText("Jogo dos Ponteiros e malloc",
                 20,
                 20,
                 24,
                 DARKBLUE);

        // Instruções
        DrawText(
            "Use as setas para coletar blocos de memoria",
            20,
            55,
            18,
            DARKGRAY
        );

        // Mostra pontos
        DrawText(
            TextFormat("Pontos: %d", pontos),
            20,
            90,
            20,
            BLACK
        );

        // Mostra quantidade
        DrawText(
            TextFormat("Blocos alocados: %d",
                       quantidade),
            20,
            115,
            20,
            BLACK
        );

        // Mostra capacidade
        DrawText(
            TextFormat("Capacidade da memoria: %d",
                       capacidade),
            20,
            140,
            20,
            BLACK
        );

        // Desenha jogador
        DrawRectangleV(
            jogador,
            (Vector2){30, 30},
            BLUE
        );

        // =========================================
        // DESENHA BLOCOS
        // =========================================

        for (int i = 0; i < quantidade; i++)
        {
            // Verifica se bloco está ativo
            if (blocos[i].ativo)
            {
                // Desenha bloco verde
                DrawRectangleV(
                    blocos[i].posicao,
                    (Vector2)
                    {
                        blocos[i].tamanho,
                        blocos[i].tamanho
                    },
                    GREEN
                );
            }
        }

        // Explicação visual
        DrawText(
            "malloc cria | realloc aumenta | free libera",
            20,
            410,
            18,
            MAROON
        );

        // Finaliza renderização
        EndDrawing();
    }

    // =========================================
    // FREE
    // =========================================

    // Libera memória dos blocos
    free(blocos);

    // Libera efeito sonoro
    UnloadSound(coleta);

    // Libera música
    UnloadMusicStream(musica);

    // Fecha sistema de áudio
    CloseAudioDevice();

    // Fecha janela
    CloseWindow();

    return 0;
}
```

---

# Explicação COMPLETA da Arquitetura

# 1. O que esse jogo faz

O jogador:
- coleta blocos verdes

Quando todos são coletados:
- mais blocos aparecem

E a memória:
- cresce dinamicamente

---

# 2. O que malloc faz

```c
malloc()
```

Reserva:
- memória dinâmica na heap

---

# Visualização da Heap

```text
HEAP
┌──────────────┐
│ Bloco 0      │
├──────────────┤
│ Bloco 1      │
├──────────────┤
│ Bloco 2      │
├──────────────┤
│ Bloco 3      │
├──────────────┤
│ Bloco 4      │
└──────────────┘
```

---

# 3. O que realloc faz

```c
realloc()
```

Aumenta:
- espaço do vetor dinâmico

---

# Antes

```text
5 blocos
```

---

# Depois

```text
10 blocos
```

---

# 4. O que free faz

```c
free()
```

Libera:
- memória heap

Sem isso:
- memory leak
- vazamento de RAM

---

# 5. O que a música ensina

```c
Music musica
```

usa:
- streaming de áudio

---

# Por que UpdateMusicStream é necessário

```c
UpdateMusicStream()
```

atualiza:
- buffer da música

Sem isso:
- música trava

---

# 6. Diferença entre Sound e Music

| Tipo | Uso |
|---|---|
| Sound | áudio curto |
| Music | áudio longo |

---

# 7. O que PlaySound faz

```c
PlaySound(coleta);
```

Toca:
- efeito sonoro imediato

Quando:
- colisão acontece

---

# Fluxo completo do jogo

```text
malloc()
    ↓
blocos aparecem
    ↓
jogador move
    ↓
colisão detecta
    ↓
som toca
    ↓
pontuação aumenta
    ↓
realloc()
    ↓
mais blocos aparecem
    ↓
free()
    ↓
memória liberada
```

---

# Conceitos profissionais aprendidos

| Conceito | Foi usado |
|---|---|
| Gameplay Loop | ✔ |
| Heap | ✔ |
| malloc | ✔ |
| realloc | ✔ |
| free | ✔ |
| Structs | ✔ |
| Collision | ✔ |
| Sound Effects | ✔ |
| Music Streaming | ✔ |
| Dynamic Entities | ✔ |

---

# Resultado esperado

Você verá:

✅ jogador azul  
✅ blocos verdes  
✅ música de fundo  
✅ som ao coletar  
✅ memória crescendo dinamicamente  
✅ score aumentando  
✅ gameplay completo  

---

# O mais importante pedagogicamente

Agora o aluno entende VISUALMENTE:

```text
malloc cria entidades
```

```text
realloc aumenta entidades
```

```text
free libera entidades
```

E ainda:
- áudio reage ao gameplay
- colisão dispara eventos
- memória dinâmica cria o mundo do jogo

---

# Curiosidade MUITO importante

Esse projeto já possui conceitos usados em:
- ECS
- Unity Internals
- Unreal Internals
- Engines 2D
- Sistemas de Entidades
- Arquiteturas de Jogos AAA

---

# Desafios

## Desafio 1

Adicione:
- partículas

---

## Desafio 2

Adicione:
- inimigos perseguindo jogador

---

## Desafio 3

Adicione:
- vidas

---

## Desafio 4

Adicione:
- game over

---

## Super Desafio

Transforme em:

- survival game
- memory collector
- shooter
- mini ECS engine
