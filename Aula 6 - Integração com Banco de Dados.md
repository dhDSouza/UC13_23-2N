# **Aula: Integração do MySQL com TypeScript usando TypeORM e POO**  

## **Introdução**  

Hoje vamos aprender a integrar um banco de dados MySQL com um projeto em TypeScript utilizando TypeORM e POO. Vamos criar uma API com as operações CRUD (Create, Read, Update, Delete).  

---

## **1. Configuração do Projeto**

Antes de começar, precisamos configurar nosso ambiente.  

### **Passo 1: Criar um projeto TypeScript**

```sh
npm init -y
npm install express mysql2 typeorm reflect-metadata
npm install typescript ts-node-dev @types/node @types/express -D
```

### **Passo 2: Configurar o TypeORM**

Crie um arquivo `ormconfig.json`:  

```json
{
  "type": "mysql",
  "host": "localhost",
  "port": 3306,
  "username": "root",
  "password": "root",
  "database": "mydb",
  "synchronize": true,
  "logging": true,
  "entities": ["src/models/*.ts"]
}
```

---

## **2. Criando a Entidade "User"**

Crie a pasta `src/models` e dentro dela o arquivo `User.ts`:  

```ts
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

## **3. Criando o Repositório de Usuários**  
Agora vamos criar o repositório para manipular os usuários no banco.  

Crie a pasta `src/repositories` e dentro dela o arquivo `UserRepository.ts`:  

```ts
import { getConnection } from "typeorm";
import { User } from "../models/User";

export class UserRepository {
  async createUser(name: string, email: string) {
    const query = `INSERT INTO user (name, email) VALUES ('${name}', '${email}')`;
    await getConnection().query(query);
  }

  async getUsers() {
    const query = `SELECT * FROM user`;
    return await getConnection().query(query);
  }

  async getUserById(id: number) {
    const query = `SELECT * FROM user WHERE id = ${id}`;
    return await getConnection().query(query);
  }

  async updateUser(id: number, name: string, email: string) {
    const query = `UPDATE user SET name = '${name}', email = '${email}' WHERE id = ${id}`;
    await getConnection().query(query);
  }

  async deleteUser(id: number) {
    const query = `DELETE FROM user WHERE id = ${id}`;
    await getConnection().query(query);
  }
}
```

>[!DANGER]
>Estamos concatenando as variáveis diretamente nas queries SQL, o que deixa nossa aplicação vulnerável a **SQL Injection**!  

---

## **4. Criando as Rotas**
Agora, criaremos as rotas para expor as operações CRUD.  

Crie a pasta `src/routes` e dentro dela o arquivo `userRoutes.ts`:  

```ts
import { Router } from "express";
import { UserRepository } from "../repositories/UserRepository";

const router = Router();
const userRepository = new UserRepository();

router.post("/users", async (req, res) => {
  const { name, email } = req.body;
  await userRepository.createUser(name, email);
  res.status(201).send("User created");
});

router.get("/users", async (req, res) => {
  const users = await userRepository.getUsers();
  res.json(users);
});

router.get("/users/:id", async (req, res) => {
  const { id } = req.params;
  const user = await userRepository.getUserById(Number(id));
  res.json(user);
});

router.put("/users/:id", async (req, res) => {
  const { id } = req.params;
  const { name, email } = req.body;
  await userRepository.updateUser(Number(id), name, email);
  res.send("User updated");
});

router.delete("/users/:id", async (req, res) => {
  const { id } = req.params;
  await userRepository.deleteUser(Number(id));
  res.send("User deleted");
});

export default router;
```

---

## **5. Criando o Servidor**
Agora, vamos criar o servidor Express para rodar nossa API.  

Crie o arquivo `src/server.ts`:  

```ts
import "reflect-metadata";
import express, { Application } from "express";
import { createConnection } from "typeorm";
import userRoutes from "./routes/userRoutes";

const app: Application = express();
app.use(express.json());
app.use(userRoutes);

createConnection()
  .then(() => {
    app.listen(3000, () => console.log("Server running on port 3000"));
  })
  .catch((error) => console.log(error));
```

---

## **6. Testando a Vulnerabilidade**
Se rodarmos a API e executarmos a seguinte requisição maliciosa no endpoint `/users/:id`, conseguimos **deletar todos os usuários do banco**:  

```
GET /users/1 OR 1=1
```

Isso ocorre porque a consulta fica assim:  

```sql
SELECT * FROM user WHERE id = 1 OR 1=1
```

O que sempre retorna todos os usuários!  

Outro exemplo perigoso seria:  

```
DELETE /users/1; DELETE FROM user;
```

Se nosso banco permitisse múltiplas consultas na mesma query, poderíamos apagar tudo!

---

## **7. Como Corrigir?**
Agora, vamos corrigir a vulnerabilidade utilizando **query builders seguros** do TypeORM.  

### **Correção no Repositório**
Substituímos `getConnection().query()` por `getRepository(User).createQueryBuilder()`:  

```ts
import { getRepository } from "typeorm";
import { User } from "../models/User";

export class UserRepository {
  async createUser(name: string, email: string) {
    await getRepository(User)
      .createQueryBuilder()
      .insert()
      .into(User)
      .values({ name, email })
      .execute();
  }

  async getUsers() {
    return await getRepository(User).find();
  }

  async getUserById(id: number) {
    return await getRepository(User)
      .createQueryBuilder("user")
      .where("user.id = :id", { id })
      .getOne();
  }

  async updateUser(id: number, name: string, email: string) {
    await getRepository(User)
      .createQueryBuilder()
      .update(User)
      .set({ name, email })
      .where("id = :id", { id })
      .execute();
  }

  async deleteUser(id: number) {
    await getRepository(User)
      .createQueryBuilder()
      .delete()
      .from(User)
      .where("id = :id", { id })
      .execute();
  }
}
```

### **O que mudou?**
1. **Uso de parâmetros nomeados (`:id`)** → Impede a concatenação direta de strings.  
2. **Métodos seguros do TypeORM** → Garantem que os dados sejam tratados corretamente antes da execução da query.  

---

## **8. Reflexão e Debate**
1. **Por que concatenar strings em queries é um problema?**  
2. **Como ataques de SQL Injection podem comprometer sistemas reais?**  
3. **Quais outras práticas de segurança devemos adotar ao lidar com bancos de dados?**  

---

### **Conclusão**
Seguir boas práticas é essencial para proteger sistemas contra ataques. A segurança deve ser uma preocupação desde a construção do código. Se negligenciarmos isso, nossa aplicação pode ser comprometida.
