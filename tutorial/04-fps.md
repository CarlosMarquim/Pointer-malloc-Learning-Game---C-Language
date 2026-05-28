# 04 - FPS

# Trabalhando com FPS em Raylib (C)

```c
#include "raylib.h"

int main()
{
    InitWindow(800, 600, "FPS Example");

    SetTargetFPS(120);

    int x = 0;

    while (!WindowShouldClose())
    {
        x += 10;

        a += 5;

        BeginDrawing();

        ClearBackground(RAYWHITE);

        DrawRectangle(x, 250, 100, 100, BLUE);

        DrawRectangle(a, 400, 100, 100, RED);

        DrawText("FPS Example", 10, 10, 20, BLACK);

        DrawFPS(700, 10);

        EndDrawing();
    }

    CloseWindow();

    return 0;
}
```

---

# O que foi adicionado nesta etapa

Nesta etapa aprendemos:

* FPS
* sincronização do jogo
* velocidade de atualização
* taxa de renderização
* controle do loop
* desempenho gráfico

Agora começamos a controlar o tempo do jogo.

---

# Explicação linha por linha

## Criando a janela

```c
InitWindow(800, 600, "FPS Example");
```

Cria a janela principal.

---

# Definindo FPS

```c
SetTargetFPS(60);
```

Define que o jogo tentará rodar em:

```text
60 FPS
```

FPS significa:

```text
Frames Per Second
```

ou:

```text
Frames por Segundo
```

---

# O que é um frame

Um frame é:

> uma imagem completa desenhada na tela.

Jogos são:

```text
milhares de imagens mostradas rapidamente
```

criando ilusão de movimento.

---

# Variável de posição

```c
int x = 0;
```

Guarda a posição horizontal do quadrado.

---

# Loop principal

```c
while (!WindowShouldClose())
```

Executa continuamente:

* lógica
* renderização
* atualização da tela

---

# Atualização de movimento

```c
x += 2;
```

Move o quadrado para direita.

Agora ele anda:

```text
2 pixels por frame
```

---

# Início do frame

```c
BeginDrawing();
```

Começa a renderização.

---

# Limpar fundo

```c
ClearBackground(RAYWHITE);
```

Apaga o frame anterior.

---

# Desenhando o quadrado

```c
DrawRectangle(x, 250, 100, 100, BLUE);
```

Desenha o objeto na posição atual.

---

# Desenhando texto

```c
DrawText("FPS Example", 10, 10, 20, BLACK);
```

Mostra texto na tela.

---

# Mostrar FPS atual

```c
DrawFPS(700, 10);
```

A Raylib possui uma função pronta para mostrar FPS.

Parâmetros:

| Valor | Significado |
| ----- | ----------- |
| 700   | posição X   |
| 10    | posição Y   |

---

# Finalizar renderização

```c
EndDrawing();
```

Entrega o frame pronto para GPU.

---

# Fechar janela

```c
CloseWindow();
```

Libera os recursos corretamente.

---

# Fluxo visual do programa

```text
SetTargetFPS(60)

↓ LOOP

Atualiza lógica
    ↓
Move quadrado
    ↓
Desenha frame
    ↓
Mostra FPS
    ↓
Exibe monitor
```

---

# Conceito novo aprendido

| Conceito      | Explicação         |
| ------------- | ------------------ |
| FPS           | frames por segundo |
| Renderização  | desenho da tela    |
| Atualização   | lógica do jogo     |
| Sincronização | controle do tempo  |
| Performance   | velocidade do jogo |

---

# O que acontece internamente

Quando você usa:

```c
SetTargetFPS(60);
```

A Raylib:

1. mede o tempo do frame
2. calcula velocidade atual
3. espera o tempo necessário
4. mantém estabilidade visual

---

# O que aconteceria sem FPS

Sem:

```c
SetTargetFPS()
```

cada computador executaria em velocidades diferentes.

Exemplo:

| Computador | FPS     |
| ---------- | ------- |
| Fraco      | 30 FPS  |
| Médio      | 120 FPS |
| Gamer      | 500 FPS |

Resultado:

```text
o jogo mudaria de velocidade em cada PC
```

---

# Por que FPS é importante

FPS influencia:

* suavidade
* gameplay
* responsividade
* física
* animação
* experiência visual

---

# Faixas comuns de FPS

| FPS | Sensação     |
| --- | ------------ |
| 15  | travando     |
| 30  | aceitável    |
| 60  | suave        |
| 120 | muito suave  |
| 240 | ultra fluido |

---

# Monitores e FPS

Monitores possuem taxa de atualização.

Exemplos:

| Monitor     | Taxa  |
| ----------- | ----- |
| comum       | 60Hz  |
| gamer       | 144Hz |
| competitivo | 240Hz |

Hz significa:

```text
quantas vezes o monitor atualiza por segundo
```

---

# GPU e renderização

A GPU trabalha:

```text
frame por frame
```

Cada frame:

* desenha pixels
* aplica cores
* calcula texturas
* envia imagem para monitor

---

# Resultado esperado

Você verá:

* quadrado azul andando
* texto na tela
* contador de FPS no canto superior direito

---

# Curiosidade

Jogos modernos possuem:

* sistemas complexos de FPS
* sincronização vertical
* frame pacing
* interpolação
* renderização assíncrona

Tudo para manter fluidez.

---

# Problema importante

Mesmo usando:

```c
SetTargetFPS(60)
```

nosso movimento ainda depende do FPS.

Isso significa:

```text
o jogo ainda pode variar entre computadores
```

A solução profissional será:

```text
DELTA TIME
```

que veremos na próxima etapa.

---

# Desafio

## Desafio 1

Mude FPS:

```c
SetTargetFPS(30);
```

Veja diferença.

---

## Desafio 2

Teste:

```c
SetTargetFPS(120);
```

---

## Desafio 3

Aumente velocidade:

```c
x += 10;
```

---

## Desafio 4

Crie dois quadrados:

* velocidades diferentes
* cores diferentes

---

## Desafio 5

Observe:

```text
como FPS muda quando o computador trabalha mais
```

---

# Próximo passo

Na próxima etapa iremos aprender:

* delta time
* tempo real
* movimento independente de FPS
* velocidade profissional

usando:

```c
GetFrameTime();
```
