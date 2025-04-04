# 🛡️ Segurança de Senhas com Bcrypt, TypeORM & Express  
## 🌍 *Desde a Terra Média até o continente de Westeros...*

## 🎬 Introdução

> "Guardar uma senha sem hash é como esconder o Um Anel no próprio dedo."  
> – Frodo Baggins _(provavelmente não)_

<div align="center">
    <img src="https://media1.tenor.com/m/9oT8tCjmcIIAAAAd/lotr-frodo.gif">
    <p>Fonte: <em><a href="https://media1.tenor.com/m/9oT8tCjmcIIAAAAd/lotr-frodo.gif" target="_blank">https://media1.tenor.com/m/9oT8tCjmcIIAAAAd/lotr-frodo.gif</a></em></p>
</div>

Salvar senhas **sem criptografia** é como deixar a chave da sua conta bancária em cima da mesa — ou pior, como o Frodo desfilando com o Um Anel no dedo. 👁️🔥  

Se um hacker invadir o sistema, ele vai encontrar as senhas ali, de bandeja. 😱  
Nós não queremos que informações **preciosas** venham a parar nas mãos de outra pessoa.

<div align="center">
    <img src="https://media1.tenor.com/m/A0R9p3ygQw0AAAAC/smeagol-my.gif">
    <p>Fonte: <em><a href="https://media1.tenor.com/m/A0R9p3ygQw0AAAAC/smeagol-my.gif" target="_blank">https://media1.tenor.com/m/A0R9p3ygQw0AAAAC/smeagol-my.gif</a></em></p>
</div>


É por isso que usamos funções de **hashing**, como o poderoso **bcrypt** 🔐 – nosso feitiço de proteção contra o lado sombrio da força (ou da internet).

---

## 🧠 O Que É Hash?  

**Hashing** é um processo que transforma dados de qualquer tamanho em uma sequência fixa de caracteres, geralmente um número hexadecimal, usando uma função matemática chamada **função hash**. Essa sequência gerada é chamada de **hash** ou **resumo criptográfico**.  

### 📌 Características principais do hashing:

1. **Tamanho fixo** → Independentemente do tamanho da entrada, o hash sempre terá um comprimento fixo (ex: `SHA-256` sempre gera um hash de 256 bits).
2. **Determinístico** → A mesma entrada sempre gera o mesmo hash.
3. **Irreversível** → Não dá para obter a entrada original a partir do hash (hashing é um processo unidirecional).
4. **Efeito avalanche** → Pequenas mudanças na entrada geram um hash completamente diferente.
5. **Rápido de calcular** → A função hash é projetada para ser eficiente em termos computacionais.

### 🛠️ Exemplos de funções hash:

- **MD5** (128 bits) → Antigo, mas vulnerável a ataques de colisão.
- **SHA-1** (160 bits) → Também considerado inseguro para criptografia.
- **SHA-256** (256 bits) → Muito usado em segurança e blockchain.
- **bcrypt, scrypt, Argon2** → Hashes usados para armazenar senhas com maior segurança.

### 🔒 Aplicações do hashing:
✅ **Armazenamento seguro de senhas** (com salting para evitar ataques de dicionário).  
✅ **Verificação de integridade de arquivos** (ex: checksums em downloads).  
✅ **Índices em bancos de dados** (ex: buscas mais rápidas).  
✅ **Estruturas de dados como tabelas hash** (ex: dicionários em Python).  
✅ **Criptografia e blockchain** (ex: Bitcoin usa SHA-256).  

### 🔍 Hashing ≠ Criptografia

| Hashing                       | Criptografia                             |
| ----------------------------- | ---------------------------------------- |
| Unidirecional 🔒               | Bidirecional 🔄                           |
| Não se reverte (senha → hash) | Pode-se descriptografar                  |
| Ideal para senhas             | Ideal para dados sigilosos (ex: cartões) |

### 💍 Analogia:

> A senha original é como o **Um Anel**.  
> Quando você faz o hash, é como jogá-lo no **Fogo da Montanha da Perdição**:  
> ele se transforma em algo **irrecuperável**. 💥

<div align="center">
    <img src="https://media1.tenor.com/m/EpOhEGBTLwAAAAAC/the-one-ring.gif">
    <p>Fonte: <em><a href="https://media1.tenor.com/m/EpOhEGBTLwAAAAAC/the-one-ring.gif" target="_blank">https://media1.tenor.com/m/EpOhEGBTLwAAAAAC/the-one-ring.gif</a></em></p>
</div>

---

## 🧊 O Bcrypt – A Muralha de Westeros

**Bcrypt é a muralha gelada que impede os White Walkers (hackers) de atravessar.**  
Ele garante 3 coisas:

1. **Salt**: cada hash é temperado com um valor aleatório (evita ataques por dicionário). 🍚  
2. **Hash único**, mesmo para senhas iguais  
3. **Lentidão proposital**, dificultando ataques de força bruta 🐢


<div align="center">
    <img src="https://static.wikia.nocookie.net/ttgot/images/f/fc/Ttgotthewall.png/revision/latest/scale-to-width-down/1000?cb=20150204062137">
    <p>Fonte: <em><a href="https://static.wikia.nocookie.net/ttgot/images/f/fc/Ttgotthewall.png/revision/latest/scale-to-width-down/1000?cb=20150204062137" target="_blank">https://static.wikia.nocookie.net/ttgot/images/f/fc/Ttgotthewall.png/revision/latest/scale-to-width-down/1000?cb=20150204062137</a></em></p>
</div>

---

## ⚙️ Setup do Projeto

```bash
npm install bcryptjs
npm install @types/bcryptjs -D
```

---

## 🧱 Model User (com magia @BeforeInsert/@BeforeUpdate)

```ts
// src/models/User.ts
import {
  Entity, PrimaryGeneratedColumn, Column, OneToMany,
  BeforeInsert, BeforeUpdate
} from "typeorm";
import bcrypt from "bcryptjs";
import { Order } from "./Order";
import { FavoriteDish } from "./FavoriteDish";

@Entity()
export class User {
  @PrimaryGeneratedColumn()
  id!: number;

  @Column({ length: 100 })
  name!: string;

  @Column({ unique: true })
  email!: string;

  @Column()
  password!: string;

  @Column({ type: "varchar", length: 15 })
  phone!: string;

  @Column({ default: "customer" })
  role!: string;

  @OneToMany(() => Order, order => order.user)
  orders!: Order[];

  @OneToMany(() => FavoriteDish, fav => fav.user)
  favoriteDishes!: FavoriteDish[];

  constructor(name: string, email: string, password: string, phone: string) {
    this.name = name;
    this.email = email;
    this.password = password;
    this.phone = phone;
  }

  @BeforeInsert()
  @BeforeUpdate()
  async hashPassword() {
    const salt = await bcrypt.genSalt(10);
    this.password = await bcrypt.hash(this.password, salt);
  }
}
```

Antes de inserir ou atualizar um usuário, será feito o hash de sua senha.

---

## 🧰 Capítulo 5: Repository do Usuário

```ts
// src/repositories/UserRepository.ts
import { AppDataSource } from "../data-source";
import { User } from "../models/User";

export class UserRepository {
  private repo = AppDataSource.getRepository(User);

  async createUser(name: string, email: string, password: string, phone: string, role?: string) {
    const user = new User(name, email, password, phone);
    if (role) user.role = role;
    return await this.repo.save(user);
  }

  async findUserByEmail(email: string) {
    return await this.repo.findOne({ where: { email } });
  }

  async findUserById(id: number) {
    return await this.repo.findOne({ where: { id }, relations: ["orders", "favoriteDishes"] });
  }

  async updateUser(id: number, fields: Partial<User>) {
    const user = await this.findUserById(id);
    if (!user) return null;
    Object.assign(user, fields);
    return await this.repo.save(user);
  }

  async deleteUser(id: number) {
    const user = await this.findUserById(id);
    if (!user) return null;
    return await this.repo.remove(user);
  }

  async findAllUsers() {
    return await this.repo.find({ relations: ["orders", "favoriteDishes"] });
  }
}
```

---

## 📡 Capítulo 6: Controller do Usuário

```ts
import { Request, Response } from "express";
import { UserRepository } from "../repositories/UserRepository";
import bcrypt from "bcryptjs";

const repo = new UserRepository();

export class UserController {

  // 👤 Registro de novo usuário
  static async register(req: Request, res: Response) {
    try {
      const { name, email, password, phone, role } = req.body;

      const existing = await repo.findUserByEmail(email);
      if (existing) return res.status(400).json({ message: "Email já em uso." });

      const user = await repo.createUser(name, email, password, phone, role);
      res.status(201).json(user);
    } catch (error) {
      res.status(500).json({ error: "Erro ao registrar usuário", details: error });
    }
  }

  // 🔐 Login
  static async login(req: Request, res: Response) {
    try {
      const { email, password } = req.body;

      const user = await repo.findUserByEmail(email);
      if (!user) return res.status(404).json({ message: "Usuário não encontrado." });

      const isValid = await bcrypt.compare(password, user.password);
      if (!isValid) return res.status(401).json({ message: "Senha inválida." });

      res.json({ message: "Login autorizado" });
    } catch (error) {
      res.status(500).json({ message: "Erro ao fazer login", details: error });
    }
  }

  // 📜 Buscar todos os usuários
  static async getAll(req: Request, res: Response) {
    try {
      const users = await repo.findAllUsers();
      res.json(users);
    } catch (error) {
      res.status(500).json({ message: "Erro ao buscar usuários", details: error });
    }
  }

  // 🔍 Buscar usuário por ID
  static async getById(req: Request, res: Response) {
    try {
      const id = parseInt(req.params.id);
      const user = await repo.findUserById(id);
      if (!user) return res.status(404).json({ message: "Usuário não encontrado." });

      res.json(user);
    } catch (error) {
      res.status(500).json({ message: "Erro ao buscar usuário", details: error });
    }
  }

  // ✏️ Atualizar usuário
  static async update(req: Request, res: Response) {
    try {
      const id = parseInt(req.params.id);
      const { name, email, password, phone, role } = req.body;

      const fieldsToUpdate = { name, email, password, phone, role };
      const updated = await repo.updateUser(id, fieldsToUpdate);

      if (!updated) return res.status(404).json({ message: "Usuário não encontrado." });

      res.json({ message: "Usuário atualizado com sucesso.", updated });
    } catch (error) {
      res.status(500).json({ message: "Erro ao atualizar usuário", details: error });
    }
  }

  // ❌ Deletar usuário
  static async delete(req: Request, res: Response) {
    try {
      const id = parseInt(req.params.id);
      const deleted = await repo.deleteUser(id);

      if (!deleted) return res.status(404).json({ message: "Usuário não encontrado." });

      res.json({ message: "Usuário deletado com sucesso." });
    } catch (error) {
      res.status(500).json({ message: "Erro ao deletar usuário", details: error });
    }
  }

}
```

---

## 🎯 Conclusão

✅ Você aprendeu:

- O que é **hash**, **sal** e por que o `bcrypt` é uma muralha de proteção.  
- Como implementar tudo usando `TypeORM`, `Express`.  
- Como lidar com registro e login de forma segura.  
- E ainda viajou pela Terra Média e Westreos no processo. 😎
