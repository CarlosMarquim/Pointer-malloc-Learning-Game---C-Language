# 01 - Basic Window

# Código Básico em Raylib (C)

```c
#include "raylib.h"

int main()
{
    InitWindow(800, 600, "Meu Primeiro Jogo");

    while (!WindowShouldClose())
    {
        BeginDrawing();

        ClearBackground(RAYWHITE);

        DrawText("Hello World", 10, 10, 20, BLACK);

        EndDrawing();
    }

    CloseWindow();

    return 0;
}
```

---

# O que foi adicionado nesta etapa

Nesta primeira etapa aprendemos:

* criar uma janela gráfica
* criar o loop principal do jogo
* desenhar na tela
* limpar o frame
* renderizar texto
* fechar corretamente o programa

Esse é o menor programa gráfico funcional em Raylib.

---

# Explicação linha por linha

## Importar a Raylib

```c
#include "raylib.h"
```

Importa a biblioteca Raylib.

Ela possui:

* gráficos
* áudio
* teclado
* mouse
* texturas
* câmera
* colisão
* matemática 2D

Tudo que um jogo precisa.

---

# Função principal

```c
int main()
```

Todo programa em C começa aqui.

O sistema operacional chama a função `main()` quando o programa inicia.

---

# Criar a janela

```c
InitWindow(800, 600, "Meu Primeiro Jogo");
```

Cria a janela do jogo.

Parâmetros:

| Valor               | Significado       |
| ------------------- | ----------------- |
| 800                 | largura da janela |
| 600                 | altura da janela  |
| "Meu Primeiro Jogo" | título da janela  |

---

# Loop principal do jogo

```c
while (!WindowShouldClose())
```

Esse é o coração do jogo.

Significa:

> “continue executando enquanto o jogador não fechar a janela”.

Todo jogo moderno possui um loop parecido.

Esse loop:

* atualiza lógica
* desenha gráficos
* toca sons
* processa teclado
* processa mouse

---

# Estrutura do frame

## Começar desenho

```c
BeginDrawing();
```

Informa para a GPU:

> “vou começar a desenhar um novo frame”.

---

## Limpar o fundo

```c
ClearBackground(RAYWHITE);
```

Pinta toda a tela de branco.

Sem isso:

* os desenhos antigos ficam acumulados
* aparecem rastros na tela
* gera efeito fantasma

---

## Desenhar texto

```c
DrawText("Hello World", 10, 10, 20, BLACK);
```

Desenha texto na tela.

Parâmetros:

| Valor         | Significado      |
| ------------- | ---------------- |
| "Hello World" | texto            |
| 10            | posição X        |
| 10            | posição Y        |
| 20            | tamanho da fonte |
| BLACK         | cor do texto     |

---

## Finalizar desenho

```c
EndDrawing();
```

Entrega o frame pronto para a GPU mostrar no monitor.

É como dizer:

> “GPU, pode exibir a imagem”.

---

# Fechar janela

```c
CloseWindow();
```

Libera recursos e encerra corretamente o programa.

Isso evita:

* vazamento de memória
* travamentos
* recursos presos no sistema

---

# Fluxo visual do programa

```text
InitWindow()

↓ LOOP PRINCIPAL

BeginDrawing()
    ↓
ClearBackground()
    ↓
DrawText()
    ↓
EndDrawing()

↓
CloseWindow()
```

---

# Conceito novo aprendido

Nesta etapa você aprendeu:

| Conceito         | Explicação                     |
| ---------------- | ------------------------------ |
| Janela gráfica   | área visual do jogo            |
| Game Loop        | repetição contínua do jogo     |
| Frame            | imagem desenhada na tela       |
| Renderização     | processo de desenhar           |
| GPU              | processador gráfico            |
| Double Buffering | técnica para evitar flickering |

---

# O que acontece internamente

Quando você executa:

```c
DrawText()
```

acontece algo parecido com:

```text
CPU
 ↓
Raylib
 ↓
OpenGL
 ↓
Driver da GPU
 ↓
Placa de vídeo
 ↓
Monitor
```

A CPU envia comandos.

A GPU desenha os pixels.

O monitor exibe o resultado.

---

# Tecnologias usadas internamente

A Raylib utiliza várias tecnologias por baixo dos panos:

| Tecnologia       | Função                           |
| ---------------- | -------------------------------- |
| OpenGL           | renderização gráfica             |
| GLFW             | criação da janela                |
| GPU Driver       | comunicação com a placa de vídeo |
| VRAM             | memória gráfica                  |
| Double Buffering | renderização suave               |

---

# O que é Double Buffering

A tela possui:

* um buffer sendo mostrado
* outro buffer sendo desenhado

Quando o frame termina:

```text
buffer desenhado ↔ buffer exibido
```

Isso evita:

* flickering
* tremedeira
* atualização incompleta

---

# Resultado esperado

Você verá:

* uma janela branca
* texto preto escrito:

```text
Hello World
```

no canto superior esquerdo.

---

# Por que essa etapa é importante

Esse programa é o equivalente gráfico ao:

```c
printf("Hello World");
```

Ele é:

* a base de praticamente qualquer jogo
* o início de engines gráficas
* a estrutura fundamental de renderização

---

# Desafio

Tente modificar:

## Desafio 1

Troque o texto:

```c
"Hello World"
```

por:

```c
"Meu Primeiro Game"
```

---

## Desafio 2

Mude a cor do fundo:

```c
RAYWHITE
```

para:

```c
SKYBLUE
```

---

## Desafio 3

Aumente o tamanho da fonte:

```c
20
```

para:

```c
40
```

---

# Próximo passo

Na próxima etapa iremos aprender:

* coordenadas
* desenho de formas
* quadrados
* retângulos
* posicionamento na tela

usando:

```c
DrawRectangle()
```
