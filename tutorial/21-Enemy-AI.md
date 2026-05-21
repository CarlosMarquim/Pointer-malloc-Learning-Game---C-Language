# 21 - Enemy AI

# Criando Inteligência Artificial de Inimigos em Raylib (C)

# Objetivo da Aula

Nesta aula iremos aprender a criar:

```text
inteligência artificial básica para inimigos
```

Agora os inimigos deixarão de ser apenas objetos parados.

Vamos implementar:

- perseguição
- patrulha
- movimentação automática
- estados de comportamento
- distância entre entidades
- IA básica de jogos

Isso é a base de:

- inimigos em plataformas
- NPCs
- monstros
- bots
- sistemas de comportamento

---

# O que iremos aprender

Nesta aula vamos estudar:

- IA básica
- perseguição de jogador
- patrulha
- distância
- estados
- comportamento automático
- movimentação inteligente
- lógica de decisão

---

# O que é IA em jogos?

IA significa:

```text
Inteligência Artificial
```

Mas em jogos isso normalmente significa:

```text
comportamentos automáticos
```

Exemplos:

| Comportamento | IA |
|---|---|
| inimigo segue jogador | ✔ |
| inimigo patrulha mapa | ✔ |
| inimigo ataca | ✔ |
| inimigo foge | ✔ |

---

# IA não precisa ser “super inteligente”

Mesmo jogos famosos usam:

```text
regras simples
```

Exemplo:

```text
SE jogador estiver perto
↓
persiga
```

---

# Jogos que usam IA

Praticamente TODOS:

| Jogo | IA |
|---|---|
| Mario | ✔ |
| Doom | ✔ |
| Terraria | ✔ |
| Hollow Knight | ✔ |
| Zelda | ✔ |

---

# Código Completo

```c
// =========================================
// IMPORTA RAYLIB
// =========================================

#include "raylib.h"
#include <math.h>

// =========================================
// ESTADOS DO INIMIGO
// =========================================

typedef enum EnemyState
{
    IDLE,
    PATROL,
    CHASE
} EnemyState;

// =========================================
// STRUCT DO INIMIGO
// =========================================

typedef struct Enemy
{
    // Posição
    Vector2 posicao;

    // Velocidade
    float velocidade;

    // Direção
    int direcao;

    // Estado atual
    EnemyState estado;

} Enemy;

// =========================================
// MAIN
// =========================================

int main(void)
{
    // Cria janela
    InitWindow(1200, 700, "Enemy AI");

    // Define FPS
    SetTargetFPS(60);

    // =========================================
    // JOGADOR
    // =========================================

    Vector2 jogador = { 300, 300 };

    float velocidadeJogador = 300;

    // =========================================
    // INIMIGO
    // =========================================

    Enemy inimigo;

    inimigo.posicao = (Vector2){ 700, 300 };

    inimigo.velocidade = 150;

    inimigo.direcao = 1;

    inimigo.estado = PATROL;

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
        // MOVIMENTO DO JOGADOR
        // =========================================

        if (IsKeyDown(KEY_RIGHT))
            jogador.x += velocidadeJogador * delta;

        if (IsKeyDown(KEY_LEFT))
            jogador.x -= velocidadeJogador * delta;

        if (IsKeyDown(KEY_UP))
            jogador.y -= velocidadeJogador * delta;

        if (IsKeyDown(KEY_DOWN))
            jogador.y += velocidadeJogador * delta;

        // =========================================
        // DISTÂNCIA ENTRE JOGADOR E INIMIGO
        // =========================================

        float distancia =
            Vector2Distance(
                jogador,
                inimigo.posicao
            );

        // =========================================
        // TROCA DE ESTADO
        // =========================================

        // Se jogador estiver perto
        if (distancia < 250)
        {
            inimigo.estado = CHASE;
        }
        else
        {
            inimigo.estado = PATROL;
        }

        // =========================================
        // IA - PATRULHA
        // =========================================

        if (inimigo.estado == PATROL)
        {
            // Movimento horizontal
            inimigo.posicao.x +=
                inimigo.velocidade *
                inimigo.direcao *
                delta;

            // Limites da patrulha
            if (inimigo.posicao.x > 900)
            {
                inimigo.direcao = -1;
            }

            if (inimigo.posicao.x < 500)
            {
                inimigo.direcao = 1;
            }
        }

        // =========================================
        // IA - PERSEGUIÇÃO
        // =========================================

        if (inimigo.estado == CHASE)
        {
            // Vetor direção
            Vector2 direcao =
                Vector2Subtract(
                    jogador,
                    inimigo.posicao
                );

            // Normaliza vetor
            direcao =
                Vector2Normalize(direcao);

            // Move inimigo
            inimigo.posicao.x +=
                direcao.x *
                inimigo.velocidade *
                1.5f *
                delta;

            inimigo.posicao.y +=
                direcao.y *
                inimigo.velocidade *
                1.5f *
                delta;
        }

        // =========================================
        // DRAW
        // =========================================

        BeginDrawing();

        ClearBackground(RAYWHITE);

        // =========================================
        // CHÃO
        // =========================================

        DrawRectangle(
            0,
            620,
            1200,
            80,
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
        // INIMIGO
        // =========================================

        Color corInimigo;

        // Cor muda conforme estado
        if (inimigo.estado == PATROL)
        {
            corInimigo = ORANGE;
        }
        else
        {
            corInimigo = RED;
        }

        DrawRectangleV(
            inimigo.posicao,
            (Vector2){50, 50},
            corInimigo
        );

        // =========================================
        // LINHA DE VISÃO
        // =========================================

        if (inimigo.estado == CHASE)
        {
            DrawLineV(
                jogador,
                inimigo.posicao,
                RED
            );
        }

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
            "Enemy AI em Raylib",
            20,
            20,
            30,
            DARKBLUE
        );

        DrawText(
            "Setas = mover jogador",
            20,
            60,
            20,
            DARKGRAY
        );

        DrawText(
            TextFormat("Distancia: %.2f",
                       distancia),
            20,
            90,
            20,
            BLACK
        );

        DrawText(
            TextFormat("Estado: %s",
                inimigo.estado == PATROL
                ? "PATROL"
                : "CHASE"),
            320,
            90,
            20,
            BLACK
        );

        DrawText(
            "Laranja = patrulha | Vermelho = perseguicao",
            650,
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

# 1. O que é IA básica?

IA básica em jogos significa:

```text
comportamentos automáticos
```

O inimigo:
- toma decisões
- reage ao jogador
- muda comportamento

---

# Exemplo simples

```text
SE jogador estiver perto
↓
perseguir
SENÃO
↓
patrulhar
```

---

# 2. Estados do inimigo

```c
typedef enum EnemyState
```

Criamos:

```text
estados de comportamento
```

---

# Estados usados

| Estado | Significado |
|---|---|
| IDLE | parado |
| PATROL | patrulhando |
| CHASE | perseguindo |

---

# O mais importante

IA em jogos normalmente funciona usando:

```text
máquinas de estado
```

---

# 3. Struct Enemy

```c
typedef struct Enemy
```

Representa:
- inimigo completo

---

# Campos do inimigo

| Campo | Função |
|---|---|
| posicao | posição no mundo |
| velocidade | velocidade |
| direcao | direção da patrulha |
| estado | comportamento atual |

---

# 4. Distância entre jogador e inimigo

```c
Vector2Distance()
```

Calcula:

```text
distância entre dois pontos
```

---

# Resultado

Quanto menor o valor:
- mais perto jogador está

---

# 5. Troca de estado

```c
if (distancia < 250)
```

Se jogador estiver perto:

```text
ativa perseguição
```

---

# Senão

```c
inimigo.estado = PATROL;
```

O inimigo:
- volta a patrulhar

---

# Isso é IA

```text
tomar decisão baseado em condições
```

---

# 6. Patrulha

```c
inimigo.posicao.x +=
    inimigo.velocidade *
    inimigo.direcao *
    delta;
```

O inimigo:
- anda sozinho

---

# 7. Direção

```c
inimigo.direcao
```

Pode ser:

| Valor | Direção |
|---|---|
| 1 | direita |
| -1 | esquerda |

---

# 8. Mudança de direção

```c
if (inimigo.posicao.x > 900)
```

Quando chega no limite:
- vira direção

---

# Isso cria patrulha

```text
→ → → →
← ← ← ←
→ → → →
```

---

# 9. Perseguição

```c
Vector2Subtract()
```

Cria vetor direção:

```text
jogador - inimigo
```

---

# Exemplo visual

```text
INIMIGO --------→ JOGADOR
```

---

# 10. Normalize

```c
Vector2Normalize()
```

Transforma vetor em:

```text
direção pura
```

Sem normalize:
- velocidade mudaria conforme distância

---

# 11. Movimento da perseguição

```c
direcao.x *
velocidade *
delta
```

Isso faz:
- inimigo ir até jogador

---

# 12. Cor do inimigo

```c
if (inimigo.estado == PATROL)
```

Muda:
- cor conforme comportamento

---

# Resultado

| Cor | Estado |
|---|---|
| laranja | patrulha |
| vermelho | perseguição |

---

# 13. Linha de visão

```c
DrawLineV()
```

Desenha:
- linha entre inimigo e jogador

---

# Isso ajuda visualizar IA

Você consegue enxergar:
- alvo
- perseguição
- direção

---

# 14. Fluxo completo da IA

```text
calcula distância
↓
decide estado
↓
executa comportamento
↓
move inimigo
↓
renderiza IA
```

---

# Visualização mental

```text
PATRULHA
   ↓
jogador aproxima
   ↓
CHASE
   ↓
jogador foge
   ↓
PATRULHA
```

---

# Conceitos profissionais aprendidos

| Conceito | Foi usado |
|---|---|
| IA básica | ✔ |
| Estados | ✔ |
| Patrulha | ✔ |
| Perseguição | ✔ |
| Distância | ✔ |
| Vetores | ✔ |
| Normalize | ✔ |
| Comportamento | ✔ |

---

# O que você aprende de verdade

# você entende:

```text
IA em jogos NÃO é mágica
```

Ela normalmente funciona usando:

```text
regras
↓
condições
↓
estados
↓
comportamentos
```

E entende:
- como inimigos perseguem
- como NPCs patrulham
- como bots “pensam”

---

# Curiosidade MUITO importante

Mesmo jogos AAA usam:
- estados
- comportamento baseado em regras
- árvores de decisão
- máquinas de estado

Inclusive:
- Doom
- Mario
- Zelda
- Hollow Knight
- Terraria

---

# Resultado esperado

Você verá:

✅ inimigo patrulhando  
✅ inimigo perseguindo jogador  
✅ troca de estados  
✅ linha de visão  
✅ IA funcionando  
✅ comportamento automático  

---

# Atividade da Aula

## Exercício 1

Aumente distância de perseguição:

```c
distancia < 400
```

---

## Exercício 2

Faça inimigo mais rápido.

---

## Exercício 3

Adicione múltiplos inimigos.

---

## Exercício 4

Crie estado:

```text
IDLE
```

onde inimigo fica parado.

---

# Desafio Extra

Crie:

```text
inimigo que foge do jogador
```

---

# Super Desafio

Crie:
- boss
- múltiplos estados
- ataque
- cooldown
- inimigos diferentes
- sistema completo de IA

---

# Próximo passo

Na próxima aula podemos evoluir para:

```text
22 - Save System.md
```

onde iremos aprender:

- salvar jogo
- carregar progresso
- arquivos
- persistência
- salvar score
- salvar posição
