# 🏗️ Construindo uma API REST CRUD com MySQL em TypeScript usando TypeORM

## 🎯 Objetivo da Aula

Nesta aula, vamos aprender a criar uma API RESTful utilizando **TypeScript**, **Express**, **MySQL** e **TypeORM**. Ao final, você será capaz de:

- Configurar um projeto TypeScript com Express;
- Conectar o projeto a um banco de dados MySQL usando TypeORM;
- Criar um CRUD (Create, Read, Update, Delete);
- Estruturar corretamente uma aplicação seguindo boas práticas.

---

## 📦 Configurando o Projeto

Vamos começar criando nosso projeto Node.js com TypeScript:

```bash
mkdir minha-api && cd minha-api
npm init -y
```

Agora, instalamos as dependências necessárias:

```bash
npm install express mysql2 typeorm reflect-metadata dotenv cors
npm install --save-dev typescript ts-node-dev @types/node @types/express
```

Criamos o arquivo `tsconfig.json` com a seguinte configuração:

```json
{
  "compilerOptions": {
    "target": "ES6",
    "module": "CommonJS",
    "outDir": "dist",
    "rootDir": "src",
    "strict": true,
    "esModuleInterop": true,
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true
  }
}
```

Adicionamos um script no `package.json` para rodar o servidor:

```json
"scripts": {
  "dev": "ts-node-dev src/server.ts"
}
```

---

## 🏛️ Configurando o Banco de Dados com TypeORM

Criamos um arquivo `.env` para armazenar nossas credenciais:

```
DB_HOST=localhost
DB_PORT=3306
DB_USER=root
DB_PASSWORD=root
DB_NAME=meubanco
```

Agora, configuramos o TypeORM no arquivo `src/data-source.ts`:

```typescript
import "reflect-metadata";
import { DataSource } from "typeorm";
import dotenv from "dotenv";

dotenv.config();

export const AppDataSource = new DataSource({
  type: "mysql",
  host: process.env.DB_HOST,
  port: Number(process.env.DB_PORT),
  username: process.env.DB_USER,
  password: process.env.DB_PASSWORD,
  database: process.env.DB_NAME,
  entities: ["src/models/*.ts"],
  synchronize: true,
  logging: false,
});
```

---

## 📌 Criando a Entidade (Model)

Criamos uma entidade chamada `User` em `src/models/User.ts`:

```typescript
import { Entity, PrimaryGeneratedColumn, Column } from "typeorm";

@Entity()
export class User {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  name: string;

  @Column()
  email: string;
}
```

---

## 🔧 Criando o Repositório

Criamos o repositório `src/repositories/UserRepository.ts`:

```typescript
import { AppDataSource } from "../data-source";
import { User } from "../models/User";

export const UserRepository = AppDataSource.getRepository(User);
```

---

## 🚀 Criando as Rotas CRUD

Criamos um arquivo `src/routes/userRoutes.ts`:

```typescript
import { Router } from "express";
import { UserController } from "../controllers/UserController";

const router = Router();

router.post("/users", UserController.create);
router.get("/users", UserController.getAll);
router.get("/users/:id", UserController.getById);
router.put("/users/:id", UserController.update);
router.delete("/users/:id", UserController.delete);

export default router;
```

---

## 🛠️ Criando o Controller

Criamos o arquivo `src/controllers/UserController.ts`:

```typescript
import { Request, Response } from "express";
import { UserRepository } from "../repositories/UserRepository";

export class UserController {
  static async create(req: Request, res: Response) {
    const { name, email } = req.body;
    const user = UserRepository.create({ name, email });
    await UserRepository.save(user);
    return res.status(201).json(user);
  }

  static async getAll(req: Request, res: Response) {
    const users = await UserRepository.find();
    return res.json(users);
  }

  static async getById(req: Request, res: Response) {
    const { id } = req.params;
    const user = await UserRepository.findOneBy({ id: Number(id) });
    if (!user) return res.status(404).json({ message: "User not found" });
    return res.json(user);
  }

  static async update(req: Request, res: Response) {
    const { id } = req.params;
    const { name, email } = req.body;
    await UserRepository.update(id, { name, email });
    return res.json({ message: "User updated successfully" });
  }

  static async delete(req: Request, res: Response) {
    const { id } = req.params;
    await UserRepository.delete(id);
    return res.json({ message: "User deleted successfully" });
  }
}
```

---

## 🚦 Configurando o Servidor

Criamos o `src/server.ts`:

```typescript
import express from "express";
import cors from "cors";
import { AppDataSource } from "./data-source";
import userRoutes from "./routes/userRoutes";

const app = express();
app.use(cors());
app.use(express.json());
app.use("/api", userRoutes);

AppDataSource.initialize().then(() => {
  app.listen(3000, () => console.log("Server is running on port 3000"));
});
```

---

## 🏋️ Exercícios

1. Adicione um campo `age` à entidade `User` e implemente as mudanças no CRUD.
2. Adicione validações para garantir que `email` seja único.
3. Refatore o código para separar a lógica do banco de dados em um serviço `UserService.ts`.
