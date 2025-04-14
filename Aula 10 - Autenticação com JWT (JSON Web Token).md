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
import jwt from "jsonwebtoken";

const secret = process.env.JWT_SECRET || "minha_chave_secreta";

export function generateToken(payload: object, expiresIn = "1h") {
    return jwt.sign(payload, secret, { expiresIn });
}

export function verifyToken(token: string) {
    return jwt.verify(token, secret);
}
```

---

### 🔐 Exemplo no Login (`UserController`):

```ts
import { generateToken } from "../auth";

static async login(req: Request, res: Response): Promise<Response> {
    const { email, password } = req.body;

    const user = await repo.findUserByEmail(email);
    if (!user) return res.status(404).json({ message: "Usuário não encontrado." });

    const isValid = await bcrypt.compare(password, user.password);
    if (!isValid) return res.status(401).json({ message: "Senha inválida." });

    const token = generateToken({ id: user.id, email: user.email, role: user.role });

    return res.json({ message: "Login bem-sucedido!", token });
}
```

---

## 🛡️ 6. Protegendo rotas com JWT (Middleware)

### 📁 `middlewares/authMiddleware.ts`

```ts
import { Request, Response, NextFunction } from "express";
import jwt from "jsonwebtoken";

const secret = process.env.JWT_SECRET || "minha_chave_secreta";

export function authenticateToken(req: Request, res: Response, next: NextFunction) {
    const authHeader = req.headers.authorization;

    const token = authHeader?.split(" ")[1]; // Bearer <token>
    if (!token) return res.status(401).json({ message: "Token não fornecido" });

    try {
        const user = jwt.verify(token, secret);
        req.user = user; // Você pode usar isso em rotas depois
        next();
    } catch (error) {
        return res.status(403).json({ message: "Token inválido ou expirado" });
    }
}
```

---

### 🔐 Exemplo de rota protegida:

```ts
import { authenticateToken } from "../middlewares/authMiddleware";

router.get("/users", authenticateToken, UserController.getAll);
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
