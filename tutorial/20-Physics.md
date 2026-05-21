# 20 - Physics

# Criando Física Básica em Raylib (C)

# Objetivo da Aula

Nesta aula iremos aprender a criar:

```text
física básica para jogos
```

Agora o personagem deixará de “flutuar”.

Vamos implementar:

- gravidade
- velocidade vertical
- pulo
- chão
- aceleração
- queda
- colisão com solo

Isso é a base de praticamente qualquer:

- jogo de plataforma
- Mario-like
- Metroidvania
- side scrolling
- engine 2D

---

# O que iremos aprender

Nesta aula vamos estudar:

- gravidade
- velocidade
- aceleração
- física frame a frame
- delta time
- colisão com chão
- estado de pulo
- velocidade vertical
- movimento realista

---

# O que é física em jogos?

Física em jogos é:

```text
simular movimento natural
```

Exemplos:

| Situação | Física |
|---|---|
| personagem cai | gravidade |
| personagem pula | velocidade vertical |
| objeto acelera | aceleração |
| objeto para no chão | colisão |

---

# Sem física

Sem física:

```text
personagem “teleporta”
```

---

# Com física

Com física:

```text
personagem acelera
↓
cai
↓
ganha velocidade
↓
encosta no chão
```

---

# Jogos que usam física

Praticamente TODOS:

| Jogo | Física |
|---|---|
| Mario | ✔ |
| Celeste | ✔ |
| Hollow Knight | ✔ |
| Terraria | ✔ |
| Sonic | ✔ |

---

# Código Completo

```c
// =========================================
// IMPORTA RAYLIB
// =========================================

#include "raylib.h"

// =========================================
// MAIN
// =========================================

int main(void)
{
    // Cria janela
    InitWindow(1200, 700, "Physics em Raylib");

    // Define FPS
    SetTargetFPS(60);

    // =========================================
    // JOGADOR
    // =========================================

    // Posição do jogador
    Vector2 jogador = { 200, 300 };

    // Velocidade vertical
    float velocidadeY = 0;

    // Velocidade horizontal
    float velocidadeX = 300;

    // Gravidade
    float gravidade = 900;

    // Força do pulo
    float forcaPulo = -500;

    // Verifica se está no chão
    bool noChao = false;

    // =========================================
    // CHÃO
    // =========================================

    Rectangle chao = {
        0,
        600,
        1200,
        100
    };

    // =========================================
    // LOOP PRINCIPAL
    // =========================================

    while (!WindowShouldClose())
    {
        // =========================================
        // DELTA TIME
        // =========================================

        float delta = GetFrameTime();

        // =========================================
        // MOVIMENTO HORIZONTAL
        // =========================================

        // Direita
        if (IsKeyDown(KEY_RIGHT))
        {
            jogador.x += velocidadeX * delta;
        }

        // Esquerda
        if (IsKeyDown(KEY_LEFT))
        {
            jogador.x -= velocidadeX * delta;
        }

        // =========================================
        // PULO
        // =========================================

        // Só pula se estiver no chão
        if (IsKeyPressed(KEY_SPACE) && noChao)
        {
            // Aplica força para cima
            velocidadeY = forcaPulo;

            // Sai do chão
            noChao = false;
        }

        // =========================================
        // GRAVIDADE
        // =========================================

        // Gravidade aumenta velocidade vertical
        velocidadeY += gravidade * delta;

        // Movimento vertical
        jogador.y += velocidadeY * delta;

        // =========================================
        // HITBOX DO JOGADOR
        // =========================================

        Rectangle jogadorRect = {
            jogador.x,
            jogador.y,
            50,
            50
        };

        // =========================================
        // COLISÃO COM CHÃO
        // =========================================

        if (CheckCollisionRecs(jogadorRect, chao))
        {
            // Encosta jogador no chão
            jogador.y = chao.y - 50;

            // Para velocidade vertical
            velocidadeY = 0;

            // Marca que está no chão
            noChao = true;
        }

        // =========================================
        // DRAW
        // =========================================

        BeginDrawing();

        // Fundo
        ClearBackground(RAYWHITE);

        // =========================================
        // CHÃO
        // =========================================

        DrawRectangleRec(
            chao,
            DARKGREEN
        );

        // =========================================
        // JOGADOR
        // =========================================

        DrawRectangleV(
            jogador,
            (Vector2){50, 50},
            BLUE
        );

        // =========================================
        // HUD
        // =========================================

        DrawRectangle(
            0,
            0,
            1200,
            120,
            Fade(LIGHTGRAY, 0.9f)
        );

        DrawText(
            "Physics em Raylib",
            20,
            20,
            30,
            DARKBLUE
        );

        DrawText(
            "Setas = mover | ESPACO = pular",
            20,
            60,
            20,
            DARKGRAY
        );

        DrawText(
            TextFormat("Velocidade Y: %.2f",
                       velocidadeY),
            20,
            90,
            20,
            BLACK
        );

        DrawText(
            TextFormat("Posicao Y: %.2f",
                       jogador.y),
            320,
            90,
            20,
            BLACK
        );

        DrawText(
            TextFormat("No Chao: %s",
                       noChao ? "SIM" : "NAO"),
            620,
            90,
            20,
            BLACK
        );

        DrawText(
            "Fisica = aceleracao + velocidade + colisao",
            720,
            90,
            20,
            MAROON
        );

        EndDrawing();
    }

    // Fecha janela
    CloseWindow();

    return 0;
}
```

---

# Explicação COMPLETA da Arquitetura

# 1. O que é física básica?

A física básica simula:

```text
gravidade
↓
velocidade
↓
aceleração
↓
colisão
```

---

# O mais importante

O personagem agora:

```text
não se move instantaneamente
```

Ele:
- acelera
- cai
- ganha velocidade

---

# 2. Gravidade

```c
float gravidade = 900;
```

Representa:

```text
força puxando jogador para baixo
```

---

# Quanto maior a gravidade

| Valor | Resultado |
|---|---|
| 200 | queda lenta |
| 900 | normal |
| 2000 | queda pesada |

---

# 3. Velocidade Vertical

```c
float velocidadeY = 0;
```

Guarda:

```text
velocidade no eixo Y
```

---

# Importante

Em jogos:
- eixo Y positivo = para baixo
- eixo Y negativo = para cima

---

# Então:

| Valor | Movimento |
|---|---|
| positivo | cai |
| negativo | sobe |

---

# 4. Força do pulo

```c
float forcaPulo = -500;
```

O valor é negativo porque:

```text
pulo sobe
```

E subir significa:

```text
Y negativo
```

---

# 5. Delta Time

```c
float delta = GetFrameTime();
```

Usamos:
- movimento baseado no tempo
- física consistente

---

# 6. Movimento Horizontal

```c
jogador.x += velocidadeX * delta;
```

Movimento profissional:

```text
velocidade × tempo
```

---

# 7. Sistema de Pulo

```c
if (IsKeyPressed(KEY_SPACE) && noChao)
```

Só permite pular:
- se estiver no chão

---

# O que significa &&

```c
&&
```

significa:

```text
E
```

Então:

```c
ESPACO && noChao
```

quer dizer:

```text
pressionou espaço E está no chão
```

---

# 8. Aplicando força de pulo

```c
velocidadeY = forcaPulo;
```

Isso faz:

```text
velocidade vertical negativa
```

Então o personagem:
- sobe

---

# 9. Gravidade frame a frame

```c
velocidadeY += gravidade * delta;
```

A cada frame:
- gravidade aumenta velocidade

---

# Isso cria aceleração

Exemplo:

```text
Frame 1 → velocidade 5
Frame 2 → velocidade 10
Frame 3 → velocidade 15
```

---

# Isso é MUITO importante

Sem isso:
- queda pareceria falsa

Com isso:
- queda ganha peso

---

# 10. Movimento Vertical

```c
jogador.y += velocidadeY * delta;
```

Aplica:
- velocidade vertical no jogador

---

# Fluxo do pulo

```text
pulo
↓
velocidade negativa
↓
personagem sobe
↓
gravidade aumenta
↓
velocidade vira positiva
↓
personagem cai
```

---

# 11. Hitbox do jogador

```c
Rectangle jogadorRect
```

Representa:
- área de colisão

---

# 12. Colisão com chão

```c
CheckCollisionRecs()
```

Detecta:
- colisão entre jogador e chão

---

# 13. Encostando no chão

```c
jogador.y = chao.y - 50;
```

Coloca jogador:
- exatamente em cima do chão

---

# Por que -50?

Porque jogador possui:

```text
50 pixels de altura
```

---

# 14. Zerando velocidade

```c
velocidadeY = 0;
```

Sem isso:
- jogador atravessaria chão

---

# 15. Estado noChao

```c
noChao = true;
```

Permite:
- novo pulo

---

# 16. Operador ternário

```c
noChao ? "SIM" : "NAO"
```

Significa:

```text
se noChao for verdadeiro
↓
mostra "SIM"
↓
senão mostra "NAO"
```

---

# 17. Fluxo completo

```text
gravidade
↓
aumenta velocidade
↓
move jogador
↓
colisão detecta chão
↓
zera velocidade
↓
permite novo pulo
```

---

# Visualização mental

```text
PULO
  ↑
SUBIDA
  ↑
gravidade desacelera
  ↓
PARA NO AR
  ↓
QUEDA
  ↓
COLISÃO CHÃO
```

---

# Conceitos profissionais aprendidos

| Conceito | Foi usado |
|---|---|
| Gravidade | ✔ |
| Física | ✔ |
| Velocidade Vertical | ✔ |
| Pulo | ✔ |
| Colisão | ✔ |
| Delta Time | ✔ |
| Aceleração | ✔ |
| Hitbox | ✔ |

---

# O que o aluno aprende de verdade

O aluno entende:

```text
física em jogos é matemática acontecendo frame a frame
```

E aprende:
- aceleração
- velocidade
- gravidade
- colisão

---

# Curiosidade MUITO importante

Praticamente todos os jogos de plataforma usam:
- gravidade
- velocidade vertical
- aceleração

Inclusive:
- Mario
- Celeste
- Hollow Knight
- Sonic
- Terraria

---

# Resultado esperado

Você verá:

✅ jogador azul  
✅ gravidade funcionando  
✅ personagem caindo  
✅ pulo realista  
✅ colisão com chão  
✅ HUD mostrando física  

---

# Atividade da Aula

## Exercício 1

Aumente gravidade:

```c
float gravidade = 1500;
```

---

## Exercício 2

Faça pulo mais forte:

```c
float forcaPulo = -800;
```

---

## Exercício 3

Faça jogador mais pesado.

---

## Exercício 4

Crie múltiplas plataformas.

---

# Desafio Extra

Crie:

```text
pulo duplo
```

---

# Super Desafio

Transforme em:

- plataforma estilo Mario
- parkour
- metroidvania
- physics sandbox

---

# Próximo passo

Na próxima aula podemos evoluir para:

```text
21 - Enemy AI.md
```

onde iremos aprender:

- inteligência artificial
- perseguição
- patrulha
- estados de inimigo
- comportamento
- IA básica para jogos
