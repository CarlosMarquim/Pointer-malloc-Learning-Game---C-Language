# 11 - Enemies

# Criando Inimigos e IA Básica em Raylib (C)

```c
#include "raylib.h"

int main()
{
    InitWindow(800, 600, "Enemies");

    int enemyX = 700;
    int direction = -1;

    while (!WindowShouldClose())
    {
        enemyX += direction * 5;

        if (enemyX <= 0 || enemyX >= 700)
        {
            direction *= -1;
        }

        BeginDrawing();

        ClearBackground(RAYWHITE);

        DrawRectangle(enemyX, 200, 100, 100, RED);

        DrawText("Enemy AI", 10, 10, 20, BLACK);

        EndDrawing();
    }

    CloseWindow();

    return 0;
}
```

---

# O que foi adicionado nesta etapa

Nesta etapa aprendemos:

- inimigos
- IA básica
- movimentação automática
- direção
- lógica procedural
- comportamento automático

Agora o jogo começa a ter “vida própria”.

---

# Explicação linha por linha

# Importando a Raylib

```c
#include "raylib.h"
```

Importa:
- renderização
- gráficos
- sistema de janela
- desenho de formas

---

# Função principal

```c
int main()
```

Ponto inicial do programa.

---

# Criando janela

```c
InitWindow(800, 600, "Enemies");
```

Cria:
- janela do jogo
- contexto gráfico
- sistema de renderização

---

# Variável de posição do inimigo

```c
int enemyX = 700;
```

Guarda:
- posição horizontal do inimigo

O inimigo começa:
- perto da direita da tela

---

# Variável de direção

```c
int direction = -1;
```

Essa variável é MUITO importante.

Ela controla:
- para onde o inimigo vai

---

# Como direção funciona

| Valor | Significado |
|---|---|
| 1 | direita |
| -1 | esquerda |

Neste caso:

```c
-1
```

faz o inimigo:
- começar indo para esquerda

---

# Loop principal

```c
while (!WindowShouldClose())
```

Executa continuamente:
- lógica
- movimentação
- IA
- renderização

---

# Movimento automático

```c
enemyX += direction * 5;
```

Isso significa:

```c
enemyX = enemyX + (direction * 5);
```

---

# O que acontece na prática

## Quando:

```c
direction = -1
```

fica:

```c
enemyX += -5;
```

O inimigo:
- anda para esquerda

---

## Quando:

```c
direction = 1
```

fica:

```c
enemyX += 5;
```

O inimigo:
- anda para direita

---

# Isso já é IA?

Sim.

Mesmo simples, isso já é:

```text
comportamento automático
```

O objeto:
- toma decisões sozinho
- muda comportamento
- reage ao ambiente

Isso já é:
- inteligência artificial básica

---

# Detectando limites

```c
if (enemyX <= 0 || enemyX >= 700)
```

Verifica:

```text
“o inimigo chegou na borda?”
```

---

# Operador ||

```c
||
```

significa:

```text
OU lógico
```

Então:

```c
enemyX <= 0 || enemyX >= 700
```

significa:

```text
“chegou na esquerda OU na direita?”
```

---

# Invertendo direção

```c
direction *= -1;
```

Isso é MUITO importante.

---

# O que significa *=

```c
direction *= -1;
```

é igual a:

```c
direction = direction * -1;
```

---

# Resultado da multiplicação

| Antes | Depois |
|---|---|
| 1 | -1 |
| -1 | 1 |

Isso faz:
- inimigo inverter direção

---

# Esse padrão aparece em jogos reais

Esse tipo de lógica é usado em:
- patrulha de inimigos
- NPCs simples
- plataformas
- shooters
- jogos arcade

---

# Iniciando renderização

```c
BeginDrawing();
```

Começa frame atual.

---

# Limpando fundo

```c
ClearBackground(RAYWHITE);
```

Apaga frame anterior.

---

# Desenhando inimigo

```c
DrawRectangle(enemyX, 200, 100, 100, RED);
```

Desenha:
- inimigo vermelho

---

# O mais importante aqui

Observe:

```c
enemyX
```

está mudando sozinho.

Isso significa:

```text
o objeto possui comportamento automático
```

---

# Desenhando texto

```c
DrawText("Enemy AI", 10, 10, 20, BLACK);
```

Mostra:
- informação na tela

---

# Finalizando renderização

```c
EndDrawing();
```

Entrega frame para GPU.

---

# Fechando janela

```c
CloseWindow();
```

Libera recursos corretamente.

---

# Fluxo visual do programa

```text
direction = -1
    ↓
enemyX += direction * 5
    ↓
Inimigo move
    ↓
Chega borda?
    ↓
Sim
    ↓
direction *= -1
    ↓
Inverte direção
```

---

# O que acontece internamente

A cada frame:

1. CPU atualiza posição
2. verifica bordas
3. altera direção
4. GPU desenha nova posição
5. monitor mostra movimento

Tudo isso acontece:
- dezenas de vezes por segundo

---

# Conceito novo aprendido

| Conceito | Explicação |
|---|---|
| AI | inteligência artificial |
| Direction | direção |
| Movimento automático | sem input |
| Patrol | patrulha |
| Lógica procedural | comportamento programado |

---

# Curiosidade MUITO importante

Muitos jogos antigos funcionavam assim:

```text
regras simples + atualização contínua
```

E mesmo jogos modernos ainda usam:
- máquinas de estado
- patrulha
- lógica procedural
- direção automática

---

# Resultado esperado

Você verá:

- um inimigo vermelho
- andando sozinho
- mudando direção nas bordas

---

# Desafio

## Desafio 1

Aumente velocidade:

```c
enemyX += direction * 10;
```

---

## Desafio 2

Crie:
- dois inimigos

---

## Desafio 3

Faça:
- inimigo vertical

Dica:

```c
enemyY += direction * 5;
```

---

## Desafio 4

Use:
- cores diferentes
- tamanhos diferentes

---

## Super Desafio

Faça:
- inimigo perseguir jogador

Dica:

```c
if (enemyX < playerX)
{
    enemyX += 2;
}
```

---

# Curiosidade Avançada

Observe:

```c
enemyX
direction
```

são apenas:
- UMA entidade

Jogos reais possuem:
- dezenas
- centenas
- milhares de inimigos

Por isso engines usam:
- arrays
- structs
- ponteiros
- memória dinâmica

Isso prepara o cérebro para:
- malloc
- free
- entity systems
- ECS
- game engines reais

---

# Próximo passo

Na próxima etapa iremos aprender:

- ponteiros
- heap
- malloc
- free
- memória dinâmica

usando:

```c
Enemy *enemies = malloc(...);
```
