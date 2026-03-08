#  Emulador chip-8.

## Link do repositório utilizado para estudo: https://github.com/aquova/chip8-book/tree/master
## autor: aquova

![Rust](https://img.shields.io/badge/language-Rust-orange?style=for-the-badge&logo=rust)
![Firebase](https://img.shields.io/badge/backend-Firebase-yellow?style=for-the-badge&logo=firebase)
![License](https://img.shields.io/badge/license-MIT-blue?style=for-the-badge)

Um interpretador de CHIP-8 moderno desenvolvido em **Rust** para explorar conceitos de baixo nível, emulação de hardware e integração com serviços cloud via **Firebase**.

## Sobre o Projeto

Este projeto consiste na implementação de uma Máquina Virtual (VM) capaz de executar ROMs do sistema CHIP-8, uma linguagem interpretada dos anos 70. 

### Objetivos Técnicos:
* **Ciclo de CPU:** Implementação do ciclo *Fetch-Decode-Execute* para os 35 opcodes originais.
* **Memory Management:** Gerenciamento manual da memória (4KB) e registradores usando as garantias de segurança do Rust.
* **Persistência com Firebase:** Integração para armazenamento de metadados e progresso de execução.

## Tecnologias
* **Rust:** Escolhido pela performance e controle de memória sem a necessidade de um Garbage Collector.
* **Firebase:** Utilizado como camada de persistência e estatísticas.
* **SDL2 / Minifb:** (Escolha sua lib gráfica) para renderização dos gráficos de 64x32 pixels.

##  Estrutura do Código (CPU)

O núcleo do emulador está organizado na `struct CPU`, que reflete a arquitetura real do hardware:

```rust
pub struct Cpu {
    pub memory: [u8; 4096],     // 4KB de RAM
    pub v: [u8; 16],            // 16 Registradores de 8 bits (V0-VF)
    pub i: u16,                 // Registrador de índice
    pub pc: u16,                // Program Counter (aponta para 0x200)
    pub stack: [u16; 16],       // Pilha para sub-rotinas
    pub sp: u16,                // Stack Pointer
    // ... timers e display
}
