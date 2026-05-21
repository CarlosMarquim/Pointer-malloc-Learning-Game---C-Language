# 13 - Complete Game

# Construindo um Jogo Completo em Raylib (C)

```c
#include "raylib.h"
#include <stdbool.h>

typedef struct Enemy
{
    int x;
    int y;
    int active;
} Enemy;

int main()
{
    InitWindow(800, 600, "Complete Game");

    SetTargetFPS(60);

    int playerX = 350;
    int playerY = 500;
    int speed = 5;

    int score = 0;

    bool gameOver = false;

    int bulletX = 0;
    int bulletY = 0;

    bool shooting = false;

    Enemy enemy;

    enemy.x = 350;
    enemy.y = 100;
    enemy.active = true;

    while (!WindowShouldClose())
    {
        if (!gameOver)
        {
            // INPUT
            if (IsKeyDown(KEY_RIGHT)) playerX += speed;
            if (IsKeyDown(KEY_LEFT)) playerX -= speed;

            // SHOOTING
            if (IsKeyPressed(KEY_SPACE) && !shooting)
            {
                shooting = true;

                bulletX = playerX + 45;
                bulletY = playerY;
            }

            // UPDATE BULLET
            if (shooting)
            {
                bulletY -= 10;

                if (bulletY < 0)
                {
                    shooting = false;
                }
            }

            // COLLISION
            Rectangle bullet =
            {
                bulletX,
                bulletY,
                10,
                20
            };

            Rectangle enemyRect =
            {
                enemy.x,
                enemy.y,
                80,
                80
            };

            if (shooting &&
                enemy.active &&
                CheckCollisionRecs(bullet, enemyRect))
            {
                score += 1;

                shooting = false;

                enemy.x = GetRandomValue(50, 700);

                enemy.y = 100;
            }

            // GAME OVER
            enemy.y += 2;

            if (enemy.y > 600)
            {
                gameOver = true;
            }
        }

        BeginDrawing();

        ClearBackground(RAYWHITE);

        // PLAYER
        DrawRectangle(playerX,
                      playerY,
                      100,
                      50,
                      BLUE);

        // BULLET
        if (shooting)
        {
            DrawRectangle(bulletX,
                          bulletY,
                          10,
                          20,
                          RED);
        }

        // ENEMY
        if (enemy.active)
        {
            DrawRectangle(enemy.x,
                          enemy.y,
                          80,
                          80,
                          GREEN);
        }

        // SCORE
        DrawText(TextFormat("Score: %i", score),
                 10,
                 10,
                 20,
                 BLACK);

        // GAME OVER
        if (gameOver)
        {
            DrawText("GAME OVER",
                     250,
                     250,
                     50,
                     RED);
        }

        EndDrawing();
    }

    CloseWindow();

    return 0;
}
```

---

# O que foi adicionado nesta etapa

Agora estamos integrando TUDO que aprendemos:

- player
- input
- movimento
- shooting
- enemy
- collision
- score
- game over
- gameplay completo

Essa é a primeira vez que criamos:

```text
um jogo realmente jogável
```

---

# O objetivo desta aula

Até agora aprendemos:
- peças isoladas

Agora:
- tudo será conectado

---

# Fluxo completo do jogo

```text
PLAYER
   ↓
INPUT
   ↓
MOVIMENTO
   ↓
SHOOTING
   ↓
BULLET
   ↓
COLLISION
   ↓
ENEMY
   ↓
SCORE
   ↓
GAME OVER
```

---

# Explicação linha por linha

# Importando Raylib

```c
#include "raylib.h"
```

Importa:
- gráficos
- teclado
- colisão
- renderização

---

# Importando bool

```c
#include <stdbool.h>
```

Permite usar:

```c
bool
true
false
```

---

# Criando Enemy

```c
typedef struct Enemy
```

Criamos:
- estrutura do inimigo

---

# Campo active

```c
int active;
```

Representa:

```text
se inimigo está ativo
```

---

# FPS

```c
SetTargetFPS(60);
```

Mantém:
- jogo suave
- velocidade consistente

---

# Jogador

```c
int playerX = 350;
int playerY = 500;
```

Guarda:
- posição da nave

---

# Sistema de pontuação

```c
int score = 0;
```

Conta:
- pontos do jogador

---

# Estado de Game Over

```c
bool gameOver = false;
```

Representa:
- fim do jogo

---

# Sistema de tiro

```c
bool shooting = false;
```

Controla:
- existência do projétil

---

# Criando inimigo

```c
Enemy enemy;
```

Cria:
- entidade inimiga

---

# Movimento do jogador

```c
if (IsKeyDown(KEY_RIGHT)) playerX += speed;
```

Move:
- nave

---

# Criando tiro

```c
if (IsKeyPressed(KEY_SPACE) && !shooting)
```

Dispara:
- projétil

---

# Atualizando projétil

```c
bulletY -= 10;
```

Move tiro:
- para cima

---

# Criando hitbox do tiro

```c
Rectangle bullet
```

Cria:
- área de colisão

---

# Criando hitbox do inimigo

```c
Rectangle enemyRect
```

Representa:
- área física do inimigo

---

# Detectando colisão

```c
CheckCollisionRecs()
```

Verifica:
- se tiro atingiu inimigo

---

# Sistema de score

```c
score += 1;
```

Aumenta:
- pontuação

---

# Respawn do inimigo

```c
enemy.x = GetRandomValue(50, 700);
```

Move inimigo:
- para posição aleatória

---

# O que é gameplay loop

Observe:

```text
atirar
↓
colidir
↓
ganhar ponto
↓
novo inimigo
↓
repetir
```

Isso é:

```text
CORE GAMEPLAY LOOP
```

A base de praticamente todos os jogos.

---

# Sistema de Game Over

```c
if (enemy.y > 600)
```

Verifica:
- inimigo chegou ao final

---

# Encerrando gameplay

```c
gameOver = true;
```

Finaliza:
- partida

---

# Renderização do player

```c
DrawRectangle(...)
```

Desenha:
- nave azul

---

# Renderização do tiro

```c
if (shooting)
```

Desenha:
- projétil vermelho

---

# Renderização do inimigo

```c
DrawRectangle(enemy.x,
              enemy.y,
              80,
              80,
              GREEN);
```

Desenha:
- inimigo verde

---

# Sistema de Score visual

```c
DrawText(TextFormat("Score: %i", score))
```

Mostra:
- pontuação em tempo real

---

# O que é TextFormat

```c
TextFormat()
```

permite:
- inserir variáveis dentro de texto

---

# Tela de Game Over

```c
DrawText("GAME OVER")
```

Mostra:
- fim da partida

---

# O que acontece internamente

A cada frame:

1. teclado é lido
2. jogador move
3. tiros atualizam
4. colisões são verificadas
5. score atualiza
6. GPU renderiza
7. monitor mostra gameplay

---

# Conceitos integrados nesta aula

| Sistema | Foi usado |
|---|---|
| Window | ✔ |
| Renderização | ✔ |
| FPS | ✔ |
| Input | ✔ |
| Shooting | ✔ |
| Enemy | ✔ |
| Collision | ✔ |
| Score | ✔ |
| Game Over | ✔ |
| Struct | ✔ |
| Bool | ✔ |

---

# O mais importante desta aula

Agora o aluno já consegue entender:

```text
como um jogo inteiro funciona
```

---

# Curiosidade MUITO importante

Quase todos os jogos possuem:

```text
Gameplay Loop
```

Exemplo:

## Mario

```text
andar
↓
pular
↓
desviar
↓
ganhar moeda
↓
repetir
```

---

## FPS Games

```text
mover
↓
atirar
↓
acertar
↓
ganhar ponto
↓
repetir
```

---

# Resultado esperado

Você verá:

- jogador azul
- inimigo verde
- tiro vermelho
- score
- game over

Tudo funcionando junto.

---

# Desafio

## Desafio 1

Adicione:
- múltiplos inimigos

---

## Desafio 2

Adicione:
- áudio

---

## Desafio 3

Adicione:
- música

---

## Desafio 4

Adicione:
- vidas

---

## Desafio 5

Adicione:
- animação

---

## Super Desafio

Transforme em:

- Space Shooter
- Asteroids
- Survival Game
- Boss Fight

---

# Curiosidade Avançada

Observe isso:

```text
player
enemy
bullet
score
gameOver
```

Isso já é praticamente:

```text
ARQUITETURA DE GAME ENGINE
```

Você acabou de construir:
- gameplay loop
- entity system básico
- renderização
- lógica de jogo
- sistema de estados

---

# Próximos passos profissionais

Agora você pode evoluir para:

| Tema | Nível |
|---|---|
| Sprite Animation | Intermediário |
| Tilemap | Intermediário |
| Camera2D | Intermediário |
| Particle Systems | Avançado |
| ECS | Avançado |
| OpenGL | Avançado |
| Multiplayer | Avançado |
| Physics Engine | Avançado |

---

# Parabéns

Você agora já consegue:

✅ criar jogos completos em C  
✅ entender gameplay loop  
✅ criar inimigos  
✅ fazer shooting  
✅ usar colisão  
✅ trabalhar com estados  
✅ estruturar entidades  
✅ construir mini engines 2D  

Isso já é:
- desenvolvimento de jogos real
- programação gráfica real
- base para engines profissionais
- introdução sólida à arquitetura de jogos
