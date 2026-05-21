# 35 - Vulkan and Modern Graphics

# Introdução ao Vulkan e Gráficos Modernos em C

# Objetivo da Aula

Nesta aula iremos aprender:

```text
como funciona renderização gráfica moderna
```

Agora vamos entrar no mundo de:

- Vulkan
- GPUs modernas
- pipelines gráficos
- command buffers
- renderização low-level
- APIs modernas de GPU
- gráficos AAA

Isso é a base de:
- Unreal Engine 5
- id Tech
- Frostbite
- motores AAA modernos
- ray tracing

---

# O que iremos aprender

Nesta aula vamos estudar:

- Vulkan
- GPU pipeline moderno
- command buffers
- render passes
- swapchain
- renderização low-level
- paralelismo GPU
- arquitetura gráfica moderna

---

# O que é Vulkan?

Vulkan é:

```text
API gráfica moderna de baixo nível
```

---

# O que significa “baixo nível”?

Significa:

```text
controle quase direto da GPU
```

---

# Vulkan foi criado pela

:contentReference[oaicite:0]{index=0}

---

# Vulkan substitui APIs antigas

| API | Geração |
|---|---|
| OpenGL | antiga |
| DirectX 11 | antiga |
| Vulkan | moderna |
| DirectX 12 | moderna |
| Metal | moderna |

---

# O mais importante

OpenGL faz MUITA coisa automaticamente.

Vulkan faz:

```text
quase nada automaticamente
```

---

# Isso parece ruim…

Mas na verdade:
- dá MUITO mais performance
- usa melhor CPU
- permite paralelismo
- reduz overhead

---

# Jogos modernos usam Vulkan

| Jogo | Vulkan |
|---|---|
| Doom Eternal | ✔ |
| Red Dead Redemption 2 | ✔ |
| Rainbow Six Siege | ✔ |
| Valheim | ✔ |

---

# Engines modernas usam Vulkan

| Engine | Vulkan |
|---|---|
| Unreal Engine 5 | ✔ |
| Unity | ✔ |
| Godot 4 | ✔ |
| id Tech | ✔ |

---

# Arquitetura Moderna da GPU

```text
CPU
↓
Vulkan API
↓
Command Buffers
↓
GPU Driver
↓
GPU
↓
Shaders
↓
Framebuffer
↓
Monitor
```

---

# Pipeline Vulkan

```text
APPLICATION
↓
INSTANCE
↓
DEVICE
↓
SWAPCHAIN
↓
RENDER PASS
↓
PIPELINE
↓
COMMAND BUFFER
↓
GPU EXECUTION
```

---

# O que é Swapchain?

Swapchain é:

```text
fila de imagens da tela
```

---

# Simplificando

Enquanto:
- uma imagem aparece

a próxima:
- está sendo desenhada

---

# Isso evita flickering

---

# O que é Command Buffer?

Command Buffer é:

```text
lista de comandos enviada para GPU
```

---

# Exemplo

```text
desenhe cubo
↓
ative shader
↓
desenhe textura
↓
aplique iluminação
```

---

# O que é Render Pass?

Render Pass define:

```text
como renderização acontecerá
```

---

# Inclui

- cores
- profundidade
- attachments
- buffers

---

# O que é Pipeline?

Pipeline define:

- shaders
- rasterização
- blending
- profundidade
- estados gráficos

---

# Vulkan é MUITO diferente do OpenGL

---

# OpenGL

```c
DrawCube();
```

---

# Vulkan

Você precisa:
- criar pipeline
- criar buffers
- criar command buffers
- sincronizar GPU
- controlar memória

---

# Estrutura de Projeto Vulkan

```text
vulkan_project/
│
├── shaders/
│   ├── vertex.spv
│   └── fragment.spv
│
├── src/
│   ├── main.c
│   ├── renderer.c
│   ├── renderer.h
│   ├── vulkan_context.c
│   └── vulkan_context.h
```

---

# Exemplo Simplificado de Inicialização Vulkan

```c
// =========================================
// IMPORTA VULKAN
// =========================================

#include <vulkan/vulkan.h>

// =========================================
// INSTANCE
// =========================================

VkInstance instance;

// =========================================
// APPLICATION INFO
// =========================================

VkApplicationInfo appInfo = {};

appInfo.sType =
    VK_STRUCTURE_TYPE_APPLICATION_INFO;

appInfo.pApplicationName =
    "Mini Vulkan Engine";

appInfo.applicationVersion =
    VK_MAKE_VERSION(1, 0, 0);

appInfo.pEngineName =
    "Custom Engine";

appInfo.engineVersion =
    VK_MAKE_VERSION(1, 0, 0);

appInfo.apiVersion =
    VK_API_VERSION_1_0;

// =========================================
// INSTANCE CREATE INFO
// =========================================

VkInstanceCreateInfo createInfo = {};

createInfo.sType =
    VK_STRUCTURE_TYPE_INSTANCE_CREATE_INFO;

createInfo.pApplicationInfo =
    &appInfo;

// =========================================
// CRIA INSTANCE
// =========================================

vkCreateInstance(
    &createInfo,
    NULL,
    &instance
);
```

---

# O que esse código faz?

Ele cria:

```text
conexão entre aplicação e Vulkan
```

---

# Vulkan funciona por objetos

Tudo em Vulkan é:
- objeto
- handle
- estrutura

---

# Exemplo

| Objeto | Função |
|---|---|
| VkInstance | conexão Vulkan |
| VkDevice | GPU lógica |
| VkBuffer | memória GPU |
| VkImage | textura |
| VkPipeline | pipeline gráfico |

---

# O que é VkDevice?

Representa:

```text
GPU lógica usada pela aplicação
```

---

# Vulkan usa GPUs explicitamente

Você escolhe:
- qual GPU usar
- como memória funciona
- sincronização

---

# Isso é MUITO poderoso

---

# O problema

Também é:
- extremamente complexo

---

# OpenGL vs Vulkan

| OpenGL | Vulkan |
|---|---|
| simples | complexo |
| automático | manual |
| menos performance | mais performance |
| single-thread | multithread |
| driver faz muito | aplicação controla tudo |

---

# O que é Overhead?

Overhead significa:

```text
trabalho extra da CPU
```

---

# Vulkan reduz overhead

Por isso:
- engines AAA usam Vulkan

---

# Paralelismo moderno

Vulkan foi feito para:

```text
múltiplas CPUs
```

---

# Isso permite

- múltiplos command buffers
- render paralelo
- melhor uso multicore

---

# O que é SPIR-V?

SPIR-V é:

```text
bytecode dos shaders Vulkan
```

---

# Fluxo dos shaders

```text
GLSL
↓
compilador
↓
SPIR-V
↓
GPU
```

---

# Exemplo compilação shader

```bash
glslangValidator -V shader.vert -o vert.spv
```

---

# Vulkan e Ray Tracing

Vulkan moderno suporta:
- ray tracing
- mesh shaders
- compute shaders

---

# O que é Compute Shader?

Compute Shader permite:

```text
usar GPU para computação geral
```

---

# Exemplo

GPU pode calcular:
- IA
- física
- partículas
- fluidos

---

# Isso é GPGPU

General Purpose GPU.

---

# GPU moderna

GPU moderna possui:
- milhares de núcleos

---

# Exemplo

Uma GPU RTX moderna possui:
- milhares de shaders cores

---

# Por isso GPUs são absurdamente rápidas

---

# Pipeline gráfico moderno

```text
VERTEX
↓
TESSELLATION
↓
GEOMETRY
↓
RASTERIZATION
↓
FRAGMENT
↓
POST PROCESS
```

---

# Rasterização

Converte:
- triângulos
- em pixels

---

# Tudo em 3D vira triângulo

---

# Isso é MUITO importante

Até:
- personagens
- carros
- mundos

viram:
- triângulos

---

# Fórmula da projeção 3D

:contentReference[oaicite:1]{index=1}

---

# Quanto mais distante

Maior Z:
- menor objeto

---

# Matrizes 3D modernas

Motores usam:

| Matriz | Função |
|---|---|
| Model | objeto |
| View | câmera |
| Projection | perspectiva |

---

# MVP Matrix

:contentReference[oaicite:2]{index=2}

---

# Renderização moderna

Hoje engines usam:
- deferred rendering
- clustered rendering
- PBR
- SSAO
- SSR
- ray tracing

---

# O que é PBR?

PBR significa:

```text
Physically Based Rendering
```

---

# Simula luz realista

---

# Ray Tracing

Ray tracing calcula:
- raios de luz reais

---

# Isso gera

- reflexos
- sombras
- iluminação realista

---

# Fluxo visual moderno

```text
CPU
↓
Vulkan
↓
Command Buffers
↓
GPU
↓
Shaders
↓
Ray Tracing
↓
Framebuffer
↓
Monitor
```

---

# Conceitos profissionais aprendidos

| Conceito | Foi usado |
|---|---|
| Vulkan | ✔ |
| GPU Pipeline | ✔ |
| Swapchain | ✔ |
| Command Buffer | ✔ |
| Render Pass | ✔ |
| Pipeline | ✔ |
| SPIR-V | ✔ |
| GPU Programming | ✔ |

---

# O que você aprende de verdade

# você entende:

```text
gráficos modernos são sistemas extremamente complexos de programação paralela para GPU
```

E entende:
- Vulkan
- renderização moderna
- GPU pipeline
- command buffers
- gráficos AAA
- arquitetura gráfica moderna

---

# Curiosidade MUITO importante

Engines modernas possuem:
- MILHÕES de linhas de código
- renderizadores gigantescos
- pipelines extremamente complexos

Inclusive:
- Unreal Engine 5
- Frostbite
- id Tech
- RE Engine

---

# Resultado esperado

Você verá:

✅ entendimento de Vulkan  
✅ arquitetura gráfica moderna  
✅ pipeline GPU  
✅ command buffers  
✅ renderização AAA  
✅ fundamentos de engines modernas  

---

# Atividade da Aula

## Exercício 1

Pesquise:
- SPIR-V

---

## Exercício 2

Instale:
- Vulkan SDK

---

## Exercício 3

Compile:
- shader Vulkan

---

## Exercício 4

Crie:
- janela Vulkan básica

---

# Desafio Extra

Crie:
- renderer Vulkan minimalista

---

# Super Desafio

Transforme isso em:
- mini Unreal Engine
- Vulkan renderer
- ray tracing engine
- deferred renderer
- compute engine
- graphics engine AAA

---

# Próximo passo

Na próxima aula podemos evoluir para:

```text
36 - Build Your Own Operating System.md
```

onde iremos aprender:

- bootloader
- kernel
- memória
- interrupções
- hardware
- sistemas operacionais em C
