# 28 - Networking Multiplayer

# Criando Multiplayer em C com Raylib

# Objetivo da Aula

Nesta aula iremos aprender a criar:

```text
multiplayer em rede
```

Agora o jogo poderá possuir:

- múltiplos jogadores
- comunicação online
- cliente/servidor
- sincronização
- troca de mensagens
- multiplayer básico

Isso é a base de:

- jogos online
- MMOs
- FPS multiplayer
- jogos cooperativos
- servidores de jogos

---

# O que iremos aprender

Nesta aula vamos estudar:

- networking
- sockets
- cliente
- servidor
- IP
- porta
- TCP
- sincronização
- multiplayer básico

---

# O que é networking?

Networking é:

```text
comunicação entre computadores
```

---

# Em jogos isso significa

```text
trocar informações entre jogadores
```

Exemplo:

```text
jogador move
↓
posição enviada
↓
outro jogador recebe
↓
posição atualiza
```

---

# O que é multiplayer?

Multiplayer significa:

```text
mais de um jogador conectado
```

---

# Exemplos

| Jogo | Multiplayer |
|---|---|
| Minecraft | ✔ |
| Terraria | ✔ |
| Counter Strike | ✔ |
| GTA Online | ✔ |
| Fortnite | ✔ |

---

# Arquitetura Multiplayer

# Modelo Cliente/Servidor

```text
CLIENTE
↓
envia dados
↓
SERVIDOR
↓
redistribui dados
↓
OUTROS CLIENTES
```

---

# O que é Socket?

Socket é:

```text
canal de comunicação
```

---

# Pense como

```text
um “telefone” entre computadores
```

---

# Tipos principais

| Tipo | Característica |
|---|---|
| TCP | confiável |
| UDP | rápido |

---

# Nesta aula vamos usar

```text
TCP
```

Porque:
- é mais simples
- mais seguro
- melhor para aprender

---

# Estrutura do Projeto

```text
multiplayer/
│
├── server.c
├── client.c
└── README.md
```

---

# Código Completo

# server.c

```c
// =========================================
// SOCKETS WINDOWS
// =========================================

#ifdef _WIN32

#include <winsock2.h>

#pragma comment(lib, "ws2_32.lib")

#else

#include <sys/socket.h>
#include <arpa/inet.h>
#include <unistd.h>

#endif

// =========================================
// IMPORTS
// =========================================

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// =========================================
// MAIN
// =========================================

int main()
{
    // =========================================
    // SOCKET SERVER
    // =========================================

    int serverSocket;

    int clientSocket;

    struct sockaddr_in serverAddr;

    struct sockaddr_in clientAddr;

    int clientSize =
        sizeof(clientAddr);

    // =========================================
    // WINSOCK INIT
    // =========================================

#ifdef _WIN32

    WSADATA wsa;

    WSAStartup(
        MAKEWORD(2,2),
        &wsa
    );

#endif

    // =========================================
    // CRIA SOCKET
    // =========================================

    serverSocket =
        socket(
            AF_INET,
            SOCK_STREAM,
            0
        );

    // =========================================
    // CONFIGURA SERVER
    // =========================================

    serverAddr.sin_family = AF_INET;

    serverAddr.sin_port =
        htons(8080);

    serverAddr.sin_addr.s_addr =
        INADDR_ANY;

    // =========================================
    // BIND
    // =========================================

    bind(
        serverSocket,
        (struct sockaddr*)&serverAddr,
        sizeof(serverAddr)
    );

    // =========================================
    // LISTEN
    // =========================================

    listen(serverSocket, 1);

    printf("Servidor iniciado!\n");

    printf("Porta: 8080\n");

    // =========================================
    // ACCEPT
    // =========================================

    clientSocket =
        accept(
            serverSocket,
            (struct sockaddr*)&clientAddr,
            &clientSize
        );

    printf("Cliente conectado!\n");

    // =========================================
    // LOOP
    // =========================================

    while (1)
    {
        char buffer[1024];

        // Recebe mensagem
        recv(
            clientSocket,
            buffer,
            sizeof(buffer),
            0
        );

        printf(
            "Recebido: %s\n",
            buffer
        );

        // Responde cliente
        send(
            clientSocket,
            buffer,
            strlen(buffer),
            0
        );
    }

    return 0;
}
```

---

# client.c

```c
// =========================================
// SOCKETS WINDOWS
// =========================================

#ifdef _WIN32

#include <winsock2.h>

#pragma comment(lib, "ws2_32.lib")

#else

#include <sys/socket.h>
#include <arpa/inet.h>
#include <unistd.h>

#endif

// =========================================
// IMPORTA RAYLIB
// =========================================

#include "raylib.h"

// =========================================
// IMPORTS
// =========================================

#include <stdio.h>
#include <string.h>

// =========================================
// MAIN
// =========================================

int main()
{
    // =========================================
    // SOCKET
    // =========================================

    int clientSocket;

    struct sockaddr_in serverAddr;

#ifdef _WIN32

    WSADATA wsa;

    WSAStartup(
        MAKEWORD(2,2),
        &wsa
    );

#endif

    // =========================================
    // CRIA SOCKET
    // =========================================

    clientSocket =
        socket(
            AF_INET,
            SOCK_STREAM,
            0
        );

    // =========================================
    // CONFIGURA SERVER
    // =========================================

    serverAddr.sin_family = AF_INET;

    serverAddr.sin_port =
        htons(8080);

    serverAddr.sin_addr.s_addr =
        inet_addr("127.0.0.1");

    // =========================================
    // CONNECT
    // =========================================

    connect(
        clientSocket,
        (struct sockaddr*)&serverAddr,
        sizeof(serverAddr)
    );

    // =========================================
    // RAYLIB
    // =========================================

    InitWindow(
        1200,
        700,
        "Networking Multiplayer"
    );

    SetTargetFPS(60);

    // =========================================
    // PLAYER
    // =========================================

    Vector2 player =
        {300, 300};

    Vector2 remotePlayer =
        {700, 300};

    float velocidade = 300;

    // =========================================
    // LOOP
    // =========================================

    while (!WindowShouldClose())
    {
        // =========================================
        // DELTA TIME
        // =========================================

        float delta =
            GetFrameTime();

        // =========================================
        // MOVIMENTO
        // =========================================

        if (IsKeyDown(KEY_RIGHT))
            player.x += velocidade * delta;

        if (IsKeyDown(KEY_LEFT))
            player.x -= velocidade * delta;

        if (IsKeyDown(KEY_UP))
            player.y -= velocidade * delta;

        if (IsKeyDown(KEY_DOWN))
            player.y += velocidade * delta;

        // =========================================
        // SERIALIZA POSIÇÃO
        // =========================================

        char buffer[1024];

        sprintf(
            buffer,
            "%.2f %.2f",
            player.x,
            player.y
        );

        // =========================================
        // ENVIA DADOS
        // =========================================

        send(
            clientSocket,
            buffer,
            strlen(buffer),
            0
        );

        // =========================================
        // RECEBE DADOS
        // =========================================

        recv(
            clientSocket,
            buffer,
            sizeof(buffer),
            0
        );

        // =========================================
        // DESERIALIZA
        // =========================================

        sscanf(
            buffer,
            "%f %f",
            &remotePlayer.x,
            &remotePlayer.y
        );

        // =========================================
        // DRAW
        // =========================================

        BeginDrawing();

        ClearBackground(RAYWHITE);

        // =========================================
        // PLAYER LOCAL
        // =========================================

        DrawCircleV(
            player,
            30,
            BLUE
        );

        // =========================================
        // PLAYER REMOTO
        // =========================================

        DrawCircleV(
            remotePlayer,
            30,
            RED
        );

        // =========================================
        // HUD
        // =========================================

        DrawRectangle(
            0,
            0,
            1200,
            140,
            Fade(LIGHTGRAY, 0.9f)
        );

        DrawText(
            "Networking Multiplayer",
            20,
            20,
            40,
            DARKBLUE
        );

        DrawText(
            "Setas = mover jogador",
            20,
            70,
            20,
            DARKGRAY
        );

        DrawText(
            "Azul = player local",
            20,
            100,
            20,
            BLUE
        );

        DrawText(
            "Vermelho = player remoto",
            320,
            100,
            20,
            RED
        );

        DrawText(
            "TCP Socket Multiplayer",
            820,
            100,
            20,
            MAROON
        );

        EndDrawing();
    }

    CloseWindow();

    return 0;
}
```

---

# Como Compilar

# Windows (MinGW)

## Servidor

```bash
gcc server.c -o server -lws2_32
```

---

## Cliente

```bash
gcc client.c -o client -lraylib -lopengl32 -lgdi32 -lwinmm -lws2_32
```

---

# Linux

## Servidor

```bash
gcc server.c -o server
```

---

## Cliente

```bash
gcc client.c -o client -lraylib -lm -ldl -lpthread
```

---

# Como Executar

# 1. Inicie servidor

```bash
./server
```

---

# 2. Inicie cliente

```bash
./client
```

---

# Fluxo Multiplayer

```text
CLIENTE
↓
envia posição
↓
SERVIDOR
↓
recebe dados
↓
reenvia posição
↓
CLIENTE recebe
```

---

# Explicação COMPLETA da Arquitetura

# 1. O que é socket?

Socket é:

```text
conexão entre computadores
```

---

# Pense como

```text
um cabo virtual
```

---

# 2. socket()

```c
socket(
    AF_INET,
    SOCK_STREAM,
    0
);
```

Cria:
- socket TCP

---

# AF_INET

```text
IPv4
```

---

# SOCK_STREAM

```text
TCP
```

---

# 3. bind()

```c
bind()
```

Liga:
- socket
- porta

---

# Porta

```text
8080
```

funciona como:
- endereço do serviço

---

# 4. listen()

```c
listen()
```

Servidor começa:
- esperar conexões

---

# 5. accept()

```c
accept()
```

Aceita:
- cliente conectado

---

# Fluxo

```text
cliente conecta
↓
servidor aceita
↓
comunicação inicia
```

---

# 6. connect()

```c
connect()
```

Cliente:
- conecta servidor

---

# 127.0.0.1

```text
localhost
```

Significa:
- próprio computador

---

# 7. send()

```c
send()
```

Envia:
- dados pela rede

---

# 8. recv()

```c
recv()
```

Recebe:
- dados da rede

---

# 9. Serialização

```c
sprintf()
```

Transforma:
- dados em texto

---

# Exemplo

```text
300.00 250.00
```

---

# 10. Deserialização

```c
sscanf()
```

Transforma:
- texto
- em números novamente

---

# Isso é MUITO importante

Networking depende de:

```text
serializar
↓
enviar
↓
deserializar
```

---

# 11. Multiplayer básico

Agora:
- jogadores trocam posições

---

# Isso já é multiplayer

Mesmo que simples.

---

# 12. TCP

TCP garante:
- entrega correta
- ordem correta

---

# Problema do TCP

É mais:
- lento
- pesado

---

# Jogos competitivos usam

```text
UDP
```

---

# 13. Cliente/Servidor

Cliente:
- envia input

Servidor:
- controla jogo

---

# Isso evita cheats

Muito importante.

---

# 14. Arquitetura profissional

Jogos modernos usam:

```text
cliente
↓
gateway
↓
servidores
↓
database
```

---

# 15. Fluxo completo

```text
INPUT
↓
SEND
↓
NETWORK
↓
SERVER
↓
RECEIVE
↓
UPDATE
```

---

# Visualização mental

```text
PLAYER A
   ↓
 INTERNET
   ↓
 SERVER
   ↓
 INTERNET
   ↓
 PLAYER B
```

---

# Conceitos profissionais aprendidos

| Conceito | Foi usado |
|---|---|
| Networking | ✔ |
| Multiplayer | ✔ |
| Socket | ✔ |
| TCP | ✔ |
| Cliente | ✔ |
| Servidor | ✔ |
| Serialização | ✔ |
| Sync | ✔ |

---

# O que você aprende de verdade

# você entende:

```text
multiplayer é troca de dados entre computadores
```

E entende:
- networking
- sockets
- cliente/servidor
- sincronização
- arquitetura online
- multiplayer básico

---

# Curiosidade MUITO importante

Jogos online modernos usam:
- UDP
- prediction
- rollback netcode
- interpolation
- lag compensation

Inclusive:
- Fortnite
- Valorant
- CS2
- League of Legends

---

# Resultado esperado

Você verá:

✅ cliente conectado  
✅ servidor funcionando  
✅ troca de dados  
✅ multiplayer básico  
✅ sincronização de posições  
✅ arquitetura online  

---

# Atividade da Aula

## Exercício 1

Adicione:
- múltiplos jogadores

---

## Exercício 2

Adicione:
- chat online

---

## Exercício 3

Sincronize:
- tiros

---

## Exercício 4

Adicione:
- nickname

---

# Desafio Extra

Crie:
- servidor dedicado

---

# Super Desafio

Transforme isso em:
- MMO
- rollback netcode
- prediction
- authoritative server
- matchmaking
- multiplayer profissional

---

# Próximo passo

Na próxima aula podemos evoluir para:

```text
29 - Game Engine.md
```

onde iremos aprender:

- criar mini engine
- loop profissional
- renderer
- scene graph
- systems
- arquitetura de engine real
