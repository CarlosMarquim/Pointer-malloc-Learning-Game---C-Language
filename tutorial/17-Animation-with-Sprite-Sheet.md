# 17 - Animação com Sprite Sheet

# Criando Animação com Sprite Sheet em Raylib (C)

# Objetivo da Aula

Nesta aula iremos aprender a criar:

```text
animações de sprites
```

usando:

```text
Sprite Sheet
```

Agora o personagem deixará de ser apenas:

```text
um quadrado parado
```

e passará a ter:

- animação
- movimento visual
- troca de frames
- sensação de vida
- aparência de jogo profissional

---

# O que iremos aprender

Nesta aula vamos estudar:

- o que é sprite sheet
- frames
- animação 2D
- recorte de textura
- Rectangle source
- Rectangle destination
- controle de frame atual
- velocidade da animação
- looping de animação

---

# O que é Sprite Sheet?

Uma Sprite Sheet é:

```text
uma imagem contendo vários frames de animação
```

Exemplo:

```text
┌────┬────┬────┬────┐
│ F1 │ F2 │ F3 │ F4 │
└────┴────┴────┴────┘
```

Cada quadrado representa:

```text
um frame da animação
```

---

# Exemplo real

Imagine um personagem andando.

Frame 1:

```text
perna esquerda
```

Frame 2:

```text
parado
```

Frame 3:

```text
perna direita
```

Frame 4:

```text
parado
```

Quando trocamos rapidamente os frames:

```text
F1 → F2 → F3 → F4
```

o cérebro interpreta como:

```text
movimento
```

---

# O que usaremos nesta aula

Precisaremos de:

```text
player.png
```

Uma sprite sheet simples.

---

# Estrutura da Sprite Sheet

Exemplo:

```text
┌────┬────┬────┬────┐
│ 64 │ 64 │ 64 │ 64 │
└────┴────┴────┴────┘
```

Cada frame possui:

```text
64x64 pixels
```

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
    InitWindow(1000, 600, "Sprite Sheet");

    // Define FPS
    SetTargetFPS(60);

    // =========================================
    // CARREGA TEXTURA
    // =========================================

    Texture2D sprite =
        LoadTexture("player.png");

    // =========================================
    // CONFIGURAÇÃO DOS FRAMES
    // =========================================

    // Quantidade de frames
    int frames = 4;

    // Frame atual
    int frameAtual = 0;

    // Largura de cada frame
    int larguraFrame =
        sprite.width / frames;

    // Altura do frame
    int alturaFrame =
        sprite.height;

    // Controle do tempo da animação
    float tempoAnimacao = 0;

    // Velocidade da animação
    float velocidadeAnimacao = 0.15f;

    // =========================================
    // POSIÇÃO DO PERSONAGEM
    // =========================================

    Vector2 posicao = { 450, 250 };

    // =========================================
    // LOOP PRINCIPAL
    // =========================================

    while (!WindowShouldClose())
    {
        // =========================================
        // UPDATE
        // =========================================

        // Delta time
        float delta = GetFrameTime();

        // Atualiza tempo da animação
        tempoAnimacao += delta;

        // Troca frame
        if (tempoAnimacao >= velocidadeAnimacao)
        {
            // Próximo frame
            frameAtual++;

            // Reinicia animação
            if (frameAtual >= frames)
            {
                frameAtual = 0;
            }

            // Reinicia temporizador
            tempoAnimacao = 0;
        }

        // Movimento do personagem
        if (IsKeyDown(KEY_RIGHT))
            posicao.x += 200 * delta;

        if (IsKeyDown(KEY_LEFT))
            posicao.x -= 200 * delta;

        if (IsKeyDown(KEY_UP))
            posicao.y -= 200 * delta;

        if (IsKeyDown(KEY_DOWN))
            posicao.y += 200 * delta;

        // =========================================
        // SOURCE RECTANGLE
        // =========================================

        Rectangle source = {

            // Frame atual
            frameAtual * larguraFrame,

            // Linha da sprite sheet
            0,

            // Largura do frame
            larguraFrame,

            // Altura do frame
            alturaFrame
        };

        // =========================================
        // DESTINATION RECTANGLE
        // =========================================

        Rectangle dest = {

            // Posição X
            posicao.x,

            // Posição Y
            posicao.y,

            // Escala horizontal
            larguraFrame * 2,

            // Escala vertical
            alturaFrame * 2
        };

        // =========================================
        // DRAW
        // =========================================

        BeginDrawing();

        ClearBackground(RAYWHITE);

        // Título
        DrawText(
            "Animacao com Sprite Sheet",
            20,
            20,
            30,
            DARKBLUE
        );

        // Instruções
        DrawText(
            "Use as setas para mover",
            20,
            60,
            20,
            DARKGRAY
        );

        // Desenha sprite animado
        DrawTexturePro(
            sprite,
            source,
            dest,
            (Vector2){0, 0},
            0,
            WHITE
        );

        // Informações
        DrawText(
            TextFormat("Frame Atual: %d",
                       frameAtual),
            20,
            100,
            20,
            BLACK
        );

        DrawText(
            "Sprite Sheet = varios frames em uma unica imagem",
            20,
            560,
            18,
            MAROON
        );

        EndDrawing();
    }

    // =========================================
    // LIBERA MEMÓRIA
    // =========================================

    UnloadTexture(sprite);

    CloseWindow();

    return 0;
}
```

---

# Explicação COMPLETA da Arquitetura

# 1. O que é animação 2D?

Animação 2D funciona trocando imagens rapidamente.

Exemplo:

```text
frame 1
↓
frame 2
↓
frame 3
↓
frame 4
```

O cérebro interpreta isso como:

```text
movimento contínuo
```

---

# 2. O que é Texture2D?

```c
Texture2D sprite
```

Representa:

```text
uma textura carregada na GPU
```

---

# O que LoadTexture faz

```c
LoadTexture("player.png");
```

Carrega:

- imagem
- textura
- sprite sheet

para memória gráfica.

---

# 3. Quantidade de frames

```c
int frames = 4;
```

Nossa sprite sheet possui:

```text
4 frames
```

---

# 4. Largura do frame

```c
sprite.width / frames
```

Se a imagem possui:

```text
256 pixels
```

e temos:

```text
4 frames
```

então:

```text
256 / 4 = 64 pixels
```

Cada frame possui:

```text
64 pixels
```

---

# 5. Frame atual

```c
int frameAtual = 0;
```

Guarda:

```text
qual frame está sendo exibido
```

---

# Exemplo

| Frame Atual | Frame |
|---|---|
| 0 | primeiro |
| 1 | segundo |
| 2 | terceiro |
| 3 | quarto |

---

# 6. Tempo da animação

```c
float tempoAnimacao = 0;
```

Guarda:

```text
quanto tempo passou
```

---

# 7. Velocidade da animação

```c
float velocidadeAnimacao = 0.15f;
```

Significa:

```text
trocar frame a cada 0.15 segundos
```

---

# Quanto menor o valor

| Valor | Resultado |
|---|---|
| 0.05 | muito rápido |
| 0.15 | normal |
| 0.5 | lento |

---

# 8. Delta Time

```c
float delta = GetFrameTime();
```

Captura:

```text
tempo entre frames
```

Isso deixa a animação:
- suave
- independente do FPS

---

# 9. Atualizando tempo

```c
tempoAnimacao += delta;
```

O temporizador cresce:

```text
0.01
0.02
0.03
```

---

# 10. Trocando frame

```c
if (tempoAnimacao >= velocidadeAnimacao)
```

Quando o tempo chega no limite:

```text
troca para próximo frame
```

---

# Próximo frame

```c
frameAtual++;
```

---

# Reiniciando animação

```c
if (frameAtual >= frames)
{
    frameAtual = 0;
}
```

Quando chega no último frame:

```text
volta para o primeiro
```

---

# Isso cria looping

```text
0 → 1 → 2 → 3 → 0 → 1 → 2
```

---

# 11. Movimento do personagem

```c
posicao.x += 200 * delta;
```

Usamos:

```text
velocidade × delta time
```

Isso é movimento profissional.

---

# 12. Source Rectangle

```c
Rectangle source
```

Define:

```text
QUAL pedaço da imagem será usado
```

---

# O mais importante

```c
frameAtual * larguraFrame
```

Isso move o recorte da sprite sheet.

---

# Exemplo visual

Frame 0:

```text
X = 0
```

Frame 1:

```text
X = 64
```

Frame 2:

```text
X = 128
```

Frame 3:

```text
X = 192
```

---

# Visualização da Sprite Sheet

```text
┌────┬────┬────┬────┐
│ F0 │ F1 │ F2 │ F3 │
└────┴────┴────┴────┘
```

O source rectangle “anda” pela imagem.

---

# 13. Destination Rectangle

```c
Rectangle dest
```

Define:

```text
onde o sprite será desenhado
```

---

# Também define escala

```c
larguraFrame * 2
alturaFrame * 2
```

Isso faz o sprite aparecer maior.

---

# 14. DrawTexturePro

```c
DrawTexturePro()
```

É uma função MUITO poderosa.

Ela permite:
- recorte
- rotação
- escala
- origem
- animação

---

# Parâmetros do DrawTexturePro

| Parâmetro | Função |
|---|---|
| sprite | textura |
| source | pedaço da imagem |
| dest | onde desenhar |
| origem | ponto de origem |
| rotação | ângulo |
| WHITE | cor |

---

# 15. Fluxo completo da animação

```text
tempo aumenta
   ↓
troca frame
   ↓
source rectangle muda
   ↓
novo frame aparece
   ↓
personagem parece animado
```

---

# Conceitos profissionais aprendidos

| Conceito | Foi usado |
|---|---|
| Sprite Sheet | ✔ |
| Frame Animation | ✔ |
| Delta Time | ✔ |
| Texture2D | ✔ |
| Rectangle Source | ✔ |
| Rectangle Destination | ✔ |
| Looping | ✔ |
| Renderização 2D | ✔ |

---

# O que voce aprende de verdade

voce entende:

```text
animação 2D = trocar frames rapidamente
```

E aprende:
- como engines reais fazem animação
- como jogos 2D funcionam
- como sprite sheets funcionam

---

# Curiosidade MUITO importante

Praticamente todos os jogos 2D usam:
- sprite sheets
- troca de frames
- animação procedural

Inclusive:
- Mario
- Sonic
- Street Fighter
- Metal Slug
- Terraria

---

# Resultado esperado

Você verá:

✅ personagem animado  
✅ troca de frames  
✅ movimento usando setas  
✅ sprite sheet funcionando  
✅ animação em looping  

---

# Atividade da Aula

## Exercício 1

Aumente quantidade de frames:

```c
int frames = 6;
```

---

## Exercício 2

Aumente velocidade da animação:

```c
float velocidadeAnimacao = 0.05f;
```

---

## Exercício 3

Faça animação mais lenta:

```c
float velocidadeAnimacao = 0.4f;
```

---

## Exercício 4

Mude escala do personagem:

```c
larguraFrame * 3
```

---

# Desafio Extra

Faça o personagem:

- virar para esquerda
- virar para direita

Dica:

```c
source.width = -larguraFrame;
```

---

# Super Desafio

Crie animações diferentes:

| Ação | Frames |
|---|---|
| Idle | parado |
| Walk | andando |
| Attack | ataque |
| Jump | pulo |

---

# Próximo passo

Na próxima aula podemos evoluir para:

```text
18 - Camera2D.md
```

onde iremos aprender:

- câmera
- scrolling
- mundo maior
- seguir jogador
- mapas grandes
- visão de jogo profissional
