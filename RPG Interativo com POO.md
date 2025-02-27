# **🧙‍♂️ RPG Interativo com POO em TypeScript 🎮📚**

**🎯Contexto:**

Vocês foram contratados por uma empresa de desenvolvimento de jogos para criar um **RPG Interativo** em que o jogador pode tomar decisões que afetam diretamente a história, os personagens, o inventário e até mesmo os combates. A proposta é que o jogo seja baseado no conceito de **livro-jogo** (onde o jogador escolhe os caminhos e suas escolhas influenciam os eventos do jogo).

O jogo será escrito em **TypeScript** e deve implementar os principais conceitos de **Programação Orientada a Objetos (POO)** que vocês aprenderam, como **classes**, **herança**, **polimorfismo**, **encapsulamento**, **abstração** e **interfaces**.

### Objetivo 🏆

Desenvolver um **RPG interativo** em que o jogador possa:

1. Escolher um personagem com habilidades e atributos únicos.
2. Tomar decisões que mudam o rumo da história.
3. Enfrentar inimigos e participar de batalhas com o uso de itens do inventário.
4. Explorar diferentes caminhos e ver as consequências de suas escolhas.
5. Ter um sistema de combate baseado em atributos de força, defesa, vida, etc.

### O Jogo 💻🎮

O jogo será estruturado em uma **história de aventura** com várias escolhas. O jogador deve interagir com personagens e objetos dentro do mundo do jogo, fazer escolhas, lutar contra inimigos e explorar diferentes cenários. Para criar uma experiência imersiva, vocês devem implementar os seguintes componentes:

---

### 1. **Personagens (Classes e Herança) 👤⚔️**

- **Classe `Personagem`:** Crie uma classe base chamada `Personagem` com os atributos principais que um personagem de RPG teria. Por exemplo:

  - `nome`: o nome do personagem.
  - `vida`: a quantidade de vida do personagem.
  - `força`: quanto dano o personagem pode causar.
  - `defesa`: a resistência do personagem.
  - `itens`: um inventário de itens que o personagem possui.
  
- **Classes de Personagem:** Crie pelo menos duas classes filhas de `Personagem`:

  - **`Guerreiro`:** com habilidades de combate, maior força, mas vida média.
  - **`Mago`:** com habilidades mágicas, maior inteligência, mas vida baixa.
  
Essas classes devem ter métodos que permitam a manipulação dos atributos, como:

- **`atacar(inimigo: Personagem): void`** — Realiza um ataque ao inimigo.
- **`usarItem(item: Item): void`** — Usa um item do inventário.
- **`receberDano(dano: number): void`** — Reduz a vida do personagem com base no dano recebido.

---

### 2. **Inventário e Itens 🛠️📦**

- **Classe `Item`:** Crie uma classe `Item` que representa os itens do jogo. Cada item pode ter:
  - `nome`: o nome do item.
  - `efeito`: o efeito que o item tem (cura, buff de força, etc.).
  
- **Inventário:** O personagem pode coletar itens ao longo do jogo. Eles devem ser armazenados em um **array** ou **lista** de itens dentro da classe `Personagem`. O jogador deve ser capaz de:
  - **Adicionar um item ao inventário**.
  - **Usar um item do inventário**, o que pode afetar atributos como vida, força ou defesa.

---

### 3. **Sistema de Combate ⚔️🛡️**

Implemente um sistema de combate simples, onde o personagem pode enfrentar inimigos. O combate deve levar em consideração:
- **Ataques:** O personagem pode atacar o inimigo e causar dano com base na sua força.
- **Defesa:** O inimigo pode ter uma defesa que diminui o dano recebido.
- **Morte:** Quando a vida de um personagem (ou inimigo) chega a 0, ele deve ser considerado morto.
- **Escolhas de batalha:** O jogador pode escolher entre atacar, usar item ou fugir durante a batalha.

---

### 4. **História e Escolhas 🗺️📖**

A parte mais importante é a **história interativa**. O jogo precisa ter uma narrativa onde o jogador faz escolhas que afetam o rumo da história. 
A história pode ser dividida em **cenas**, cada uma com um conjunto de escolhas e consequências:

- **Cena de Introdução:** O jogo começa com uma introdução e o jogador escolhe qual personagem quer jogar.
- **Cenas de Decisão:** Ao longo do jogo, o jogador deve tomar decisões. Por exemplo:
  - **"Você encontra um inimigo. O que fazer?"**
    - **(A)** Atacar com a espada.
    - **(B)** Usar feitiço.
    - **(C)** Fugir.
  - Cada escolha terá um impacto direto na história, como ganhar ou perder uma batalha, ou até mesmo mudar o final do jogo.
  
Implemente uma função para **navegar entre as cenas**, de acordo com as escolhas feitas pelo jogador.

---

### 5. **Desafios e Consequências 🔄💥**

- **Decisões com Consequências:** As escolhas feitas pelo jogador devem impactar a história e o estado do jogo. Por exemplo, ao atacar um inimigo, o jogador pode perder pontos de vida, mas também pode obter um item ou habilidade nova. 
- **Múltiplos Finais:** Dependendo das decisões do jogador, o jogo pode ter diferentes finais. Crie pelo menos dois finais possíveis: um bom e um ruim.

---

### Requisitos Técnicos 💡

1. **POO em TypeScript:** Use classes, herança, interfaces e métodos. A lógica do jogo e a manipulação dos personagens devem estar bem encapsuladas em suas respectivas classes.
2. **Interface `Inimigo`:** Defina uma interface para os inimigos, que pode ser implementada por várias classes de inimigos, como **Goblin**, **Dragão**, etc.
3. **Exceções:** Adicione um tratamento de erros, por exemplo, ao tentar usar um item inexistente no inventário ou atacar quando o personagem está sem vida.
4. **Código Modularizado:** Divida o código em arquivos e classes para que ele seja fácil de entender e manter.
5. **Testes (opcional):** Teste a integridade das suas classes e a lógica do jogo com alguns exemplos de uso.

---

### Entregáveis 📝

1. **Código fonte:** Todo o código-fonte do jogo, organizado em arquivos TypeScript.
2. **Documentação:** Um documento explicando como o jogo funciona, como implementar novos personagens ou itens, e como as classes estão organizadas.
3. **Descrição do Jogo:** Um resumo da história, personagens, escolhas e possíveis finais.

---

### Dicas para Implementação 💡

- Não sobrecarregue o código com complexidade desnecessária, mas tente utilizar ao máximo os conceitos de POO que aprendemos.
- Pense na história e nas escolhas do jogador enquanto implementa as classes. O jogo precisa ser interessante e dinâmico.
- Lembre-se de que os itens e o inventário devem ser bem integrados à mecânica de combate.

Boa sorte e que vençam as batalhas! 🏆🕹️
