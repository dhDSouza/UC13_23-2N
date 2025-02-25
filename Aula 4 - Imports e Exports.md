# **Aula 4: Modularização com Imports e Exports no TypeScript**

## 🎯 **Objetivos da Aula**

- Compreender a **importância da modularização** no desenvolvimento.
- Aprender a **separar código em módulos** reutilizáveis.
- Utilizar **import e export** no TypeScript.
- Modularizar um projeto Express para maior organização.

---

# 📦 **1. O que é Modularização?**

A **modularização** é uma prática que separa um sistema em pequenos **módulos reutilizáveis**, tornando o código mais:

✔ **Organizado**
✔ **Fácil de manter**
✔ **Reutilizável**
✔ **Testável**

No TypeScript, usamos **export** para expor funções, classes ou variáveis e **import** para trazê-las para outros arquivos.

---

# 🏗 **2. Criando e Exportando Módulos**

Podemos criar um **arquivo de módulo** para organizar nosso código. Vamos dividir a classe `Usuario` em um arquivo separado.

### 📌 **Criando um Módulo de Usuário**

📄 `models/Usuario.ts`

```ts
export class Usuario {
  constructor(public id: number, public nome: string, public email: string) {}
}
```

Aqui estamos usando `export` para permitir que essa classe seja usada em outros arquivos.

---

# 🔗 **3. Importando Módulos**

Agora, podemos importar a classe `Usuario` em outro arquivo.

📄 `routes/usuarioRoutes.ts`

```ts
import { Usuario } from "../models/Usuario";
import { Router, Request, Response } from "express";

const router = Router();
const usuarios: Usuario[] = [];

router.post("/usuarios", (req: Request, res: Response) => {
  const { id, nome, email } = req.body;
  const novoUsuario = new Usuario(id, nome, email);
  usuarios.push(novoUsuario);
  res.status(201).json({ mensagem: "Usuário criado com sucesso!", usuario: novoUsuario });
});

router.get("/usuarios", (req: Request, res: Response) => {
  res.status(200).json(usuarios);
});

export default router;
```

Aqui usamos `import { Usuario } from "../models/Usuario";` para acessar a classe `Usuario`.

>[!TIP]
>Estamos exportando `router` usando `export default`, o que permite importá-lo sem chaves `{}`.

---

# 🚀 **4. Modularizando o Servidor Express**

Agora, podemos usar esse módulo no nosso servidor principal.

📄 `server.ts`

```ts
import express from "express";
import usuarioRoutes from "./routes/usuarioRoutes";

const app = express();
app.use(express.json());
app.use("/api", usuarioRoutes);

app.listen(3000, () => console.log("Servidor rodando na porta 3000!"));
```

Agora nosso servidor está organizado e reutilizável! 🚀

---

# 🎯 **5. Exercícios para Praticar**

1️⃣ **Separe as rotas de `Produto` em um arquivo `produtoRoutes.ts`.**

2️⃣ **Crie um arquivo `database.ts` para armazenar listas de `usuarios` e `produtos`.**

---

# ✅ **Resumo da Aula**

✅ Aprendemos **o conceito de modularização** e sua importância.  
✅ Vimos como **usar import e export** no TypeScript.  
✅ Criamos **módulos para organizar um projeto Express**.  
✅ Aplicamos **separação de responsabilidades** no nosso servidor.  

Agora você pode estruturar projetos de forma profissional e escalável! 🚀
