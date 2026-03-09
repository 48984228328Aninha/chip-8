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
pub const SCREEN_WIDTH: usize = 64;
pub const SCREEN_HEIGHT: usize = 32;

const RAM_SIZE: usize = 4096;
const NUM_REGS: usize = 16;
const STACK_SIZE: usize = 16;
const NUM_KEYS: usize = 16;
const START_ADDR: u16 = 0x200;

pub struct Emu {
    pc: u16, //registrador especial: program counter
    ram: [u8; RAM_SIZE],
    screen: [bool; SCREEN_WIDTH * SCREEN_HEIGHT],
    v_reg: [u8; NUM_REGS], //torna a busca + rápida
    i_reg: u16, //outro registrador
    sp: u16, //stack pointer, eh array de 16 valores que a cpu pode armazenar
    stack: [u16; STACK_SIZE],
    keys: [bool; NUM_KEYS], //serve p mapear os estados
    dt: u8, //eh um temporizador delay timer que vai até 0
    st: u8, //temporizador q ao chegar a 0, emite som
}

impl Emu {
    pub fn new() -> Self {
        Self {
            pc: START_ADDR,
            ram: [0; RAM_SIZE],
            screen: [false; SCREEN_WIDTH * SCREEN_HEIGHT],
            v_reg: [0; NUM_REGS],
            i_reg: 0,
            sp: 0,
            stack: [0; STACK_SIZE],
            keys: [false; NUM_KEYS],
            dt: 0,
            st: 0,
        }
    }
}
```
## Program Counter irá começar no endereço 0x200, o primeiro 512 será vazio e será armazenado os sprites. Sprites = gráficos bidimensionais

![SCREEN](https://github.com/aquova/chip8-book/raw/master/src/img/font_diagram.png)
## Cada pixel é um bit (branco/preto) e todo sprite no chip-8 tem 8 pixels de largura e cada linha tem 8 bits (1 byte). Os sprites foram escritos para todos os dígitos hexadecimais e precisam estar em algum lugar da RAM 
- Opcode são as instruções que estão prestes a ser executadas.
