# **Aula 3: Revisão de Orientação a Objetos com TypeScript**

## 🎯 **Objetivos da Aula**

- Revisar os conceitos fundamentais de **Orientação a Objetos (OO)**.
- Aprender como **aplicar OO no TypeScript**.
- Explorar **classes, herança, interfaces e polimorfismo**.
- Integrar conceitos de **OO ao desenvolvimento com Express**.

---

# 🏢 **1. O que é Orientação a Objetos?**

A **Programação Orientada a Objetos (POO)** é um **paradigma de programação** que organiza o código em **objetos**, permitindo **reutilização**, **modularidade** e **facilidade de manutenção**.

### ⚙️ **Pilares da OO**

| Pilar        | O que significa? |
|-------------|----------------|
| **Encapsulamento** | Protege os dados e controla seu acesso. |
| **Herança** | Permite criar novas classes baseadas em outras. |
| **Polimorfismo** | Um método pode ter diferentes comportamentos. |
| **Abstração** | Foca apenas nos detalhes essenciais do objeto. |

---

# 📚 **2. Criando Classes e Objetos no TypeScript**

Uma **classe** é um molde para criar objetos. Em TypeScript, podemos definir classes usando `class`:

```ts
class Pessoa {
  nome: string;
  idade: number;

  constructor(nome: string, idade: number) {
    this.nome = nome;
    this.idade = idade;
  }

  apresentar(): string {
    return `Olá, meu nome é ${this.nome} e tenho ${this.idade} anos.`;
  }
}

const pessoa1 = new Pessoa("Daniel", 28);
console.log(pessoa1.apresentar());
```

> **✅ Saída:** `Olá, meu nome é Daniel e tenho 28 anos.`

---

# 📦 **3. Encapsulamento: Modificadores de Acesso**

O **encapsulamento** protege os atributos e métodos da classe. O TypeScript fornece três níveis de acesso:

| Modificador | Acesso |
|------------|--------|
| **public** | Acessível de qualquer lugar. |
| **private** | Acessível apenas dentro da própria classe. |
| **protected** | Acessível dentro da classe e de classes filhas. |

### **Exemplo com encapsulamento**

```ts
class ContaBancaria {
  private saldo: number;

  constructor(saldoInicial: number) {
    this.saldo = saldoInicial;
  }

  depositar(valor: number): void {
    this.saldo += valor;
  }

  consultarSaldo(): number {
    return this.saldo;
  }
}

const minhaConta = new ContaBancaria(1000);
minhaConta.depositar(500);
console.log(minhaConta.consultarSaldo());
```

> **✅ Saída:** `1500`

> **⚠ Erro:** Se tentarmos acessar `minhaConta.saldo`, o TypeScript bloqueará o acesso, pois é **private**.

---

# 💸 **4. Herança: Reutilizando Código**

A **herança** permite que uma classe herde características de outra.

```ts
class Animal {
  constructor(public nome: string) {}

  fazerSom(): void {
    console.log("Som genérico de animal");
  }
}

class Cachorro extends Animal {
  latir(): void {
    console.log("Au Au!");
  }
}

const dog = new Cachorro("Rex");
dog.fazerSom(); // Herdado de Animal
dog.latir();
```

> **✅ Saída:**
>
> `Som genérico de animal`
>
> `Au Au!`

---

# 🔀 **5. Polimorfismo: Flexibilidade no Código**

O **polimorfismo** permite que classes filhas modifiquem comportamentos herdados da classe pai.

```ts
class Gato extends Animal {
  fazerSom(): void {
    console.log("Miau!");
  }
}

const gato = new Gato("Mingau");
gato.fazerSom();
```

> **✅ Saída:** `Miau!` (O comportamento foi alterado na classe filha)

---

# 👥 **6. Interfaces: Definindo Contratos**

Uma **interface** define um **modelo** que as classes devem seguir.

```ts
interface IUsuario {
  nome: string;
  email: string;
  exibirInfo(): void;
}

class Usuario implements IUsuario {
  constructor(public nome: string, public email: string) {}

  exibirInfo(): void {
    console.log(`Usuário: ${this.nome}, Email: ${this.email}`);
  }
}

const user = new Usuario("Daniel", "daniel@email.com");
user.exibirInfo();
```

> **✅ Saída:** `Usuário: Daniel, Email: daniel@email.com`

---

# 🛠 **7. Aplicando OO ao Express**

Vamos usar esses conceitos para organizar nosso código no Express!

### 📝 **Criando um Modelo para Usuários**

```ts
class Usuario {
  constructor(public id: number, public nome: string, public email: string) {}
}

const usuarios: Usuario[] = [];
```

### ✈️ **Criando Rotas com OO**

```ts
import express, { Request, Response } from 'express';

const app = express();
app.use(express.json());

app.post('/usuarios', (req: Request, res: Response) => {
  const { id, nome, email } = req.body;
  const novoUsuario = new Usuario(id, nome, email);
  usuarios.push(novoUsuario);
  res.status(201).json({ mensagem: "Usuário criado com sucesso!", usuario: novoUsuario });
});

app.get('/usuarios', (req: Request, res: Response) => {
  res.status(200).json(usuarios);
});

app.listen(3000, () => console.log('Servidor rodando na porta 3000!'));
```

Agora, temos um código mais organizado e seguindo os princípios de OO! 🚀

---

# 🎯 **8. Exercícios para Praticar**

1️⃣ **Crie uma classe `Produto` com os atributos `id`, `nome` e `preco`.**

2️⃣ **Implemente um CRUD para `Produto` com Express.**

3️⃣ **Crie uma interface `IVeiculo` e implemente uma classe `Carro` baseada nela.**

---

# ✅ **Resumo da Aula**

✅ Revisamos **Orientação a Objetos (OO)** e seus pilares.
✅ Aprendemos **como usar classes, herança, interfaces e polimorfismo** em TypeScript.
✅ Aplicamos **OO no desenvolvimento com Express**.
✅ Criamos **exercícios práticos para reforçar o aprendizado**.

Agora você está pronto para aplicar OO nos seus projetos Express! 🚀

