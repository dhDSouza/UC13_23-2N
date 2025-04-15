# 🧠 Autenticação com JWT (JSON Web Token)

### 📌 Objetivo da aula

Ao final desta aula, o aluno será capaz de:

- Entender o que é o JWT e por que usá-lo
- Compreender como funciona a autenticação baseada em token
- Implementar o JWT em uma aplicação Node.js + TypeScript
- Usar o token de forma segura para proteger rotas

---

## 🧩 1. O que é JWT?

**JWT (JSON Web Token)** é um padrão **seguro e leve** para realizar **autenticação e troca de informações entre sistemas**.

> 💡 JWT é como um **crachá digital com informações codificadas**, que o sistema confia e valida.

---

### 🔐 Analogia com a vida real:

Imagine que você vai em um evento. Na entrada você faz seu **cadastro** (login), e recebe um **crachá** com seu nome, papel e um código único.

- Esse crachá é o **token JWT**.
- Ele serve pra você **acessar áreas restritas** sem precisar se identificar de novo.
- O sistema de segurança **verifica seu crachá** antes de te deixar passar.

---

## 📦 2. Estrutura de um JWT

Um token JWT é composto por **3 partes**, separadas por pontos (`.`):

```
HEADER.PAYLOAD.SIGNATURE
```

### 🔍 Vamos entender cada parte:

#### 1️⃣ **Header** (Cabeçalho):
Define o tipo de token (JWT) e o algoritmo de criptografia usado:

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

#### 2️⃣ **Payload** (Carga útil):
Contém os **dados (claims)** que queremos transmitir, como `id`, `email`, `role`, etc.

```json
{
  "id": 123,
  "email": "daniel@email.com",
  "role": "admin"
}
```

#### 3️⃣ **Signature** (Assinatura):
Serve para **validar** se o token não foi **alterado**. É gerado com um segredo que **só o servidor conhece**.

```
HMACSHA256(
  base64UrlEncode(header) + "." + base64UrlEncode(payload),
  SECRET_KEY
)
```

---

## 🚀 3. Quando usar JWT?

✅ Situações ideais:

- APIs RESTful onde o cliente e o servidor estão separados
- SPAs (Single Page Applications) com frontend em React/Vue/Angular
- Aplicações mobile (React Native, Flutter, etc.)
- Sistemas com múltiplos serviços (microserviços)

❌ Não é recomendado:

- Quando você precisa invalidar sessões com frequência (JWT não é armazenado no servidor)
- Quando há necessidade de *logout forçado global*

---

## 🛠️ 4. Instalando o JWT na aplicação

```bash
npm install jsonwebtoken
npm install @types/jsonwebtoken -D
```

---

## 🔨 5. Implementando JWT no login

### 📁 `auth.ts` – Funções de geração e verificação do token:

```ts
import jwt, { Secret, JwtPayload } from "jsonwebtoken";
import dotenv from "dotenv";

dotenv.config();

const { JWT_SECRET } = process.env;

if(!JWT_SECRET) {
    throw new Error('JWT_SECRET is not defined in .env file');
}

const secret: Secret = JWT_SECRET;

export function generateToken(payload: JwtPayload): string {
    return jwt.sign(payload, secret, { expiresIn: "1h" })
}

export function verifyToken(token: string) {
    return jwt.verify(token, secret);
}
```

---

### 🔐 Exemplo de controller (`UserController`):

```ts
import { Request, Response } from "express";
import { UserRepository } from "../repositories/UserRepository";
import bcrypt from "bcryptjs";
import { generateToken } from "../auth";

const repo = new UserRepository();

export class UserController {

  // 👤 Registro de novo usuário
  static async register(req: Request, res: Response) {
    try {
      const { name, email, password, phone, role } = req.body;

      const existing = await repo.findUserByEmail(email);
      if (existing) {
        res.status(400).json({ message: "Email já em uso." });
        return;
      }

      const user = await repo.createUser(name, email, password, phone, role);
      res.status(201).json(user);
      return;
    } catch (error) {
      res.status(500).json({ error: "Erro ao registrar usuário", details: error });
      return;
    }
  }

  // 🔐 Login
  static async login(req: Request, res: Response) {
    try {
      const { email, password } = req.body;

      const user = await repo.findUserByEmail(email);
      if (!user) {
        res.status(404).json({ message: "Usuário não encontrado." });
        return;
      }

      const isValid = await bcrypt.compare(password, user.password);
      if (!isValid) {
        res.status(401).json({ message: "Senha inválida." });
        return;
      }

      const token = generateToken({ id: user.id, email: user.email, role: user.role });

      res.json({ message: "Login autorizado", token });
    } catch (error) {
      res.status(500).json({ message: "Erro ao fazer login", details: error });
    }
  }

  // 📜 Buscar todos os usuários
  static async getAll(req: Request, res: Response) {
    try {
      const users = await repo.findAllUsers();
      res.json(users);
      return;
    } catch (error) {
      res.status(500).json({ message: "Erro ao buscar usuários", details: error });
      return;
    }
  }

  // 🔍 Buscar usuário por ID
  static async getById(req: Request, res: Response) {
    try {
      const id = parseInt(req.params.id);
      const user = await repo.findUserById(id);
      if (!user) {
        res.status(404).json({ message: "Usuário não encontrado." });
        return;
      }

      res.json(user);
    } catch (error) {
      res.status(500).json({ message: "Erro ao buscar usuário", details: error });
      return;
    }
  }

  // ✏️ Atualizar usuário
  static async update(req: Request, res: Response) {
    try {
      const id = parseInt(req.params.id);
      const { name, email, password, phone, role } = req.body;

      const fieldsToUpdate = { name, email, password, phone, role };
      const updated = await repo.updateUser(id, fieldsToUpdate);

      if (!updated) {
        res.status(404).json({ message: "Usuário não encontrado." });
        return;
      }

      res.json({ message: "Usuário atualizado com sucesso.", updated });
    } catch (error) {
      res.status(500).json({ message: "Erro ao atualizar usuário", details: error });
      return;
    }
  }

  // ❌ Deletar usuário
  static async delete(req: Request, res: Response) {
    try {
      const id = parseInt(req.params.id);
      const deleted = await repo.deleteUser(id);

      if (!deleted) {
        res.status(404).json({ message: "Usuário não encontrado." });
        return;
      }

      res.json({ message: "Usuário deletado com sucesso." });
      return;
    } catch (error) {
      res.status(500).json({ message: "Erro ao deletar usuário", details: error });
      return;
    }
  }
}
```

---

## 🛡️ 6. Protegendo rotas com JWT (Middleware)

### 📁 `middlewares/authMiddleware.ts`

```ts
import { Request, Response, NextFunction } from "express";
import { verifyToken } from "../auth";

declare global {
    namespace Express {
        interface Request {
            user?: any;
        }
    }
}

export class AuthMiddleware {
    async authenticateToken(req: Request, res: Response, next: NextFunction) {
        const authHeader = req.headers.authorization;

        const token = authHeader?.split(" ")[1]; // Bearer <token>
        if (!token) {
            res.status(401).json({ message: "Token não fornecido" });
            return;
        }
        
        try {
            const user = verifyToken(token);
            req.user = user; // Você pode usar isso em rotas depois
            next();
        } catch (error) {
            res.status(403).json({ message: "Token inválido ou expirado" });
            return;
        }
    }
}
```

---

### 🔐 Exemplo de rota protegida:

```ts
import { Router } from "express";
import { UserController } from "../controllers/UserController";
import { AuthMiddleware } from "../middlewares/AuthMiddleware";

const middleware = new AuthMiddleware()

// Instanciando o roteador
const router = Router();

router.post('/register', UserController.register);
router.post('/login', UserController.login);
router.get('/users/', middleware.authenticateToken, UserController.getAll);

export default router;
```

---

## 🧯 7. Boas práticas com JWT

✅ Dicas valiosas:

- **Nunca envie o token como query string**
- Armazene no **localStorage** (web) ou **SecureStorage** (mobile)
- Sempre use **HTTPS**
- Utilize **expiração curta** (ex: 15min, 1h)
- Se precisar "deslogar", implemente **blacklist** de tokens ou use refresh tokens

---

## 🤓 Dica de ouro (com analogia final):

> Pense no JWT como um **ingresso de cinema** 🎟️.  
> Uma vez que você o tem, você pode entrar na sala (rota protegida) sem precisar comprar de novo (refazer login).  
> Mas se o ingresso expirar ou for falsificado, o segurança (middleware) vai te barrar.

## Exercícios

### 🧪 **Exercício 1: Setup do Projeto + Criação do Usuário**

**Objetivo**: Criar uma API básica que permite registrar usuários com senha criptografada.

**Passos sugeridos**:

1. Inicialize o projeto com TypeScript (`tsc --init`).
2. Configure o Express.
3. Configure o TypeORM com MySQL.
4. Crie uma entidade `User` com os campos:
   - `id`
   - `name`
   - `email` (único)
   - `password` (criptografado com bcrypt)

6. Crie um endpoint `POST /register` para cadastrar o usuário.

**Dica**: use `bcrypt.hash(password, salt)`.

📌 **Desafio extra**:
- Validar se o email já está cadastrado.

---

### 🔐 **Exercício 2: Login com JWT**

**Objetivo**: Criar um endpoint de login que retorna um token JWT.

**Passos sugeridos**:
1. Crie o endpoint `POST /login`.
2. No login:
   - Verifique se o usuário existe pelo email.
   - Compare a senha com `bcrypt.compare`.
   - Gere um JWT com `jsonwebtoken`.

📌 **Desafio extra**:
- Permitir login com username ou email.
- Armazenar o token no client e simular um consumo com `Insomnia` ou `Postman`.

---

### ✅ **Exercício 3: Rota protegida com JWT**

**Objetivo**: Criar um middleware de autenticação JWT para proteger rotas.

**Passos sugeridos**:
1. Crie o middleware `authMiddleware`.
2. No middleware:
   - Verifique o token no header `Authorization: Bearer <token>`.
   - Decodifique e valide o JWT.
   - Anexe o ID do usuário ao `req`.

3. Crie uma rota `GET /profile` protegida que retorna os dados do usuário logado.
