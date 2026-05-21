# 26 - Asset Manager

# Criando Asset Manager em Raylib (C)

# Objetivo da Aula

Nesta aula iremos aprender a criar:

```text
Asset Manager
```

Agora o jogo passará a possuir:

- gerenciamento de texturas
- gerenciamento de áudio
- cache de assets
- carregamento centralizado
- descarregamento correto
- organização profissional de recursos

Isso é a base de qualquer:

- engine
- jogo profissional
- framework
- sistema gráfico moderno

---

# O que iremos aprender

Nesta aula vamos estudar:

- gerenciamento de assets
- cache
- Texture2D
- Sound
- Music
- carregamento centralizado
- descarregamento de memória
- organização profissional

---

# O que é um Asset?

Asset significa:

```text
recurso do jogo
```

Exemplos:

| Asset | Tipo |
|---|---|
| imagem | textura |
| música | áudio |
| efeito sonoro | sound |
| fonte | font |
| animação | sprite |

---

# O que é Asset Manager?

Asset Manager é:

```text
sistema responsável por carregar e controlar recursos
```

---

# Sem Asset Manager

Sem organização:

```text
LoadTexture()
LoadTexture()
LoadTexture()
LoadTexture()
```

Problemas:
- duplicação
- vazamento de memória
- desorganização
- difícil manutenção

---

# Com Asset Manager

```text
Asset Manager
↓
centraliza recursos
↓
organiza memória
↓
evita duplicação
```

---

# Jogos usam isso o tempo todo

| Engine | Asset Manager |
|---|---|
| Unity | ✔ |
| Unreal | ✔ |
| Godot | ✔ |
| Source Engine | ✔ |

---

# Estrutura do Projeto

```text
meu_jogo/
│
├── assets/
│   ├── textures/
│   │   └── player.png
│   │
│   ├── audio/
│   │   ├── music.ogg
│   │   └── shot.wav
│   │
│   └── fonts/
│
├── src/
│   ├── main.c
│   ├── asset_manager.c
│   └── asset_manager.h
```

---

# Código Completo

# asset_manager.h

```c
#ifndef ASSET_MANAGER_H
#define ASSET_MANAGER_H

// =========================================
// IMPORTA RAYLIB
// =========================================

#include "raylib.h"

// =========================================
// TEXTURAS
// =========================================

extern Texture2D playerTexture;

// =========================================
// ÁUDIOS
// =========================================

extern Sound shotSound;

extern Music backgroundMusic;

// =========================================
// FUNÇÕES
// =========================================

void LoadAssets();

void UnloadAssets();

void UpdateAssets();

#endif
```

---

# asset_manager.c

```c
// =========================================
// IMPORTA HEADER
// =========================================

#include "asset_manager.h"

// =========================================
// ASSETS GLOBAIS
// =========================================

// Textura do jogador
Texture2D playerTexture;

// Som de tiro
Sound shotSound;

// Música de fundo
Music backgroundMusic;

// =========================================
// LOAD ASSETS
// =========================================

void LoadAssets()
{
    // Inicializa sistema de áudio
    InitAudioDevice();

    // =========================================
    // CARREGA TEXTURAS
    // =========================================

    playerTexture =
        LoadTexture(
            "assets/textures/player.png"
        );

    // =========================================
    // CARREGA SONS
    // =========================================

    shotSound =
        LoadSound(
            "assets/audio/shot.wav"
        );

    // =========================================
    // CARREGA MÚSICA
    // =========================================

    backgroundMusic =
        LoadMusicStream(
            "assets/audio/music.ogg"
        );

    // Toca música
    PlayMusicStream(backgroundMusic);
}

// =========================================
// UPDATE ASSETS
// =========================================

void UpdateAssets()
{
    // Atualiza streaming da música
    UpdateMusicStream(backgroundMusic);
}

// =========================================
// UNLOAD ASSETS
// =========================================

void UnloadAssets()
{
    // Libera textura
    UnloadTexture(playerTexture);

    // Libera som
    UnloadSound(shotSound);

    // Libera música
    UnloadMusicStream(backgroundMusic);

    // Fecha sistema de áudio
    CloseAudioDevice();
}
```

---

# main.c

```c
// =========================================
// IMPORTA RAYLIB
// =========================================

#include "raylib.h"

// =========================================
// IMPORTA ASSET MANAGER
// =========================================

#include "asset_manager.h"

// =========================================
// MAIN
// =========================================

int main(void)
{
    // Cria janela
    InitWindow(
        1200,
        700,
        "Asset Manager"
    );

    // FPS
    SetTargetFPS(60);

    // =========================================
    // LOAD ASSETS
    // =========================================

    LoadAssets();

    // =========================================
    // JOGADOR
    // =========================================

    Vector2 jogador = {300, 300};

    float velocidade = 300;

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
        // UPDATE ASSETS
        // =========================================

        UpdateAssets();

        // =========================================
        // MOVIMENTO
        // =========================================

        if (IsKeyDown(KEY_RIGHT))
            jogador.x += velocidade * delta;

        if (IsKeyDown(KEY_LEFT))
            jogador.x -= velocidade * delta;

        if (IsKeyDown(KEY_UP))
            jogador.y -= velocidade * delta;

        if (IsKeyDown(KEY_DOWN))
            jogador.y += velocidade * delta;

        // =========================================
        // TOCA SOM
        // =========================================

        if (IsKeyPressed(KEY_SPACE))
        {
            PlaySound(shotSound);
        }

        // =========================================
        // DRAW
        // =========================================

        BeginDrawing();

        ClearBackground(RAYWHITE);

        // =========================================
        // DESENHA TEXTURA
        // =========================================

        DrawTexture(
            playerTexture,
            jogador.x,
            jogador.y,
            WHITE
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
            "Asset Manager",
            20,
            20,
            40,
            DARKBLUE
        );

        DrawText(
            "Setas = mover",
            20,
            70,
            20,
            DARKGRAY
        );

        DrawText(
            "ESPACO = tocar som",
            220,
            70,
            20,
            DARKGRAY
        );

        DrawText(
            "Assets carregados centralmente",
            700,
            70,
            20,
            MAROON
        );

        EndDrawing();
    }

    // =========================================
    // UNLOAD ASSETS
    // =========================================

    UnloadAssets();

    // Fecha janela
    CloseWindow();

    return 0;
}
```

---

# Explicação COMPLETA da Arquitetura

# 1. O que é Asset Manager?

Asset Manager controla:

```text
todos os recursos do jogo
```

---

# Exemplos de assets

| Tipo | Exemplo |
|---|---|
| textura | player.png |
| som | shot.wav |
| música | music.ogg |
| fonte | font.ttf |

---

# 2. Por que Asset Manager existe?

Porque jogos possuem:

```text
MUITOS assets
```

Sem gerenciamento:
- bagunça
- memória desperdiçada
- duplicação

---

# 3. extern

```c
extern Texture2D playerTexture;
```

Significa:

```text
variável existe em outro arquivo
```

---

# Isso é MUITO importante

Permite:
- compartilhar assets entre módulos

---

# 4. Asset global

```c
Texture2D playerTexture;
```

Agora:
- textura pode ser usada no jogo inteiro

---

# 5. LoadAssets()

```c
LoadAssets();
```

Centraliza:
- carregamento de tudo

---

# Isso melhora organização

Em vez de:

```text
cada arquivo carregando assets
```

temos:

```text
um sistema central
```

---

# 6. InitAudioDevice()

```c
InitAudioDevice();
```

Inicializa:
- sistema de áudio da Raylib

---

# Sem isso

Sons:
- não funcionam

---

# 7. LoadTexture()

```c
LoadTexture()
```

Carrega:
- imagem para GPU

---

# Isso cria Texture2D

A textura fica:
- armazenada na VRAM

---

# 8. LoadSound()

```c
LoadSound()
```

Carrega:
- efeito sonoro curto

---

# Exemplo

| Tipo | Uso |
|---|---|
| Sound | tiros |
| Sound | explosões |
| Sound | clique |

---

# 9. LoadMusicStream()

```c
LoadMusicStream()
```

Carrega:
- música longa

---

# Diferença importante

| Tipo | Característica |
|---|---|
| Sound | pequeno |
| Music | streaming |

---

# 10. PlayMusicStream()

```c
PlayMusicStream()
```

Começa:
- tocar música

---

# 11. UpdateMusicStream()

```c
UpdateMusicStream()
```

MUITO importante.

Música em streaming precisa:
- atualização contínua

---

# Sem isso

A música:
- trava
- para
- falha

---

# 12. PlaySound()

```c
PlaySound(shotSound);
```

Toca:
- efeito sonoro

---

# 13. UnloadTexture()

```c
UnloadTexture()
```

Libera:
- memória da GPU

---

# Isso é MUITO importante

Sem unload:
- memory leak
- consumo excessivo

---

# 14. UnloadAssets()

```c
UnloadAssets();
```

Centraliza:
- liberação de recursos

---

# Fluxo profissional

```text
Load
↓
Use
↓
Unload
```

---

# 15. Estrutura profissional

Agora:
- assets ficam separados
- código organizado
- recursos centralizados

---

# Fluxo completo

```text
LoadAssets()
↓
carrega texturas
↓
carrega sons
↓
carrega músicas
↓
jogo usa recursos
↓
UnloadAssets()
↓
memória liberada
```

---

# Visualização mental

```text
ASSET MANAGER
│
├── TEXTURES
├── AUDIO
├── MUSIC
└── FONTS
```

---

# 16. Cache de assets

Asset Manager também evita:

```text
carregar mesma textura várias vezes
```

---

# Isso melhora performance

Muito importante em:
- engines
- jogos grandes
- multiplayer

---

# Conceitos profissionais aprendidos

| Conceito | Foi usado |
|---|---|
| Asset Manager | ✔ |
| LoadTexture | ✔ |
| LoadSound | ✔ |
| Music Stream | ✔ |
| Cache | ✔ |
| VRAM | ✔ |
| Unload | ✔ |
| Organização Profissional | ✔ |

---

# O que você aprende de verdade

# você entende:

```text
jogos profissionais possuem sistemas centrais de recursos
```

E entende:
- gerenciamento de memória
- organização de assets
- carregamento profissional
- descarregamento correto
- arquitetura de engines

---

# Curiosidade MUITO importante

Grandes engines possuem:
- asset databases
- streaming
- cache
- virtual assets
- pipelines de importação

Inclusive:
- Unreal Engine
- Unity
- Frostbite
- Source Engine

---

# Resultado esperado

Você verá:

✅ textura carregada  
✅ som funcionando  
✅ música tocando  
✅ assets centralizados  
✅ organização profissional  
✅ gerenciamento correto de memória  

---

# Atividade da Aula

## Exercício 1

Adicione:
- fonte personalizada

---

## Exercício 2

Adicione:
- múltiplas músicas

---

## Exercício 3

Adicione:
- animações

---

## Exercício 4

Crie:
- texture atlas

---

# Desafio Extra

Crie:
- sistema de preload

---

# Super Desafio

Transforme isso em:
- mini engine
- asset database
- hot reload
- streaming de assets
- resource manager completo

---

# Próximo passo

Na próxima aula podemos evoluir para:

```text
27 - Entity Component System (ECS).md
```

onde iremos aprender:

- ECS
- entidades
- componentes
- systems
- arquitetura moderna de games
- organização usada em engines AAA
