# **Aula 2: Rotas, Requisições, Respostas e Middlewares no Express**  

## 🎯 **Objetivos da Aula**  

- Entender **o que são rotas** no Express.  
- Compreender o conceito de **requisição e resposta**.  
- Aprender a **estrutura de uma rota**.  
- Introduzir **middlewares**, entender seu propósito e como usá-los.  

---

# 🛫 **1. O que é uma Rota?**  

No Express, uma **rota** define como a aplicação responde a uma **requisição HTTP** feita para um **endereço específico**.  

### 🛤 **Analogia para entender melhor**  

🔹 **Imagine que você está em uma cidade e precisa encontrar um restaurante.** Você abre um mapa (navegador) e pesquisa pelo nome do restaurante (URL). Ao clicar nele, você recebe informações como horário de funcionamento e cardápio (dados da resposta).   

>No Express, o caminho que seu pedido segue é definido por **rotas**, e cada uma responde de maneira diferente dependendo do que você solicitar.

---

# 🔄 **2. O que é uma Requisição e uma Resposta?**  

Sempre que interagimos com um site ou aplicativo, estamos enviando **requisições** (requests) e recebendo **respostas** (responses).  

| Termo        | O que significa? |
|-------------|----------------|
| **Requisição (Request)** | É o pedido feito pelo usuário para o servidor. Pode conter dados como um formulário preenchido. |
| **Resposta (Response)**  | É o que o servidor retorna para o usuário. Pode ser uma mensagem, um arquivo ou até mesmo um erro. |

### 🖥 **Exemplo no Express**  

```ts
app.get('/saudacao', (req: Request, res: Response) => {
  res.send('Olá, jovem programador!');
});
```

✅ O navegador **faz uma requisição GET para `/saudacao`**.  
✅ O servidor **responde com a mensagem "Olá, jovem programador!"**.  

---

# 🚦 **3. Como Criar Rotas no Express?**  

Aqui está a estrutura básica de uma rota no Express:  

```ts
app.metodo('/caminho', (req, res) => {
  res.resposta();
});
```

### 🌍 **Principais Métodos HTTP**  

| Método  | O que faz? | Exemplo |
|---------|-----------|---------|
| **GET**    | Busca dados do servidor | `/usuarios` |
| **POST**   | Envia dados para o servidor | `/usuarios` |
| **PUT**    | Atualiza um recurso | `/usuarios/:id` |
| **DELETE** | Remove um recurso | `/usuarios/:id` |

### 📊 **Principais Status HTTP e Seus Significados**  

| Código | Significado | Quando Usar? |
|--------|------------|--------------|
| **200** | OK | Quando a requisição é bem-sucedida |
| **201** | Created | Quando um novo recurso é criado |
| **204** | No Content | Quando a requisição foi bem-sucedida, mas não há retorno |
| **400** | Bad Request | Quando há um erro no envio da requisição |
| **404** | Not Found | Quando o recurso não é encontrado |
| **500** | Internal Server Error | Quando ocorre um erro no servidor |

Agora, vamos **criar essas rotas na prática**!

```ts
import express, { Application, Request, Response } from 'express';

const app: Application = express();
const PORT: number = 3000;

app.use(express.json());

// 🔹 Rota GET (Buscar dados)
app.get('/usuarios', (req: Request, res: Response) => {
  res.status(200).json({ mensagem: 'Lista de usuários' });
});

// 🔹 Rota POST (Criar novo usuário)
app.post('/usuarios', (req: Request, res: Response) => {
  const { nome } = req.body;
  if (!nome) {
    return res.status(400).json({ mensagem: 'Nome é obrigatório!' });
  }
  res.status(201).json({ mensagem: `Usuário ${nome} criado com sucesso!` });
});

app.listen(PORT, () => console.log(`🔥 Servidor rodando em http://localhost:${PORT}`));
```

Agora nosso servidor responde corretamente às **requisições e respostas**! 🚀  

---

# 🏗 **4. O que é um Middleware?**  

Um **middleware** é uma função que roda **entre** a requisição e a resposta. Ele pode:  

✔️ **Modificar a requisição antes de chegar à rota.**  
✔️ **Executar verificações, como autenticação.**  
✔️ **Adicionar logs para depuração.**  

### 🧙 **Analogias para facilitar**  

🔹 **O porteiro do seu prédio:** Antes de entrar, ele pode verificar se você é um morador ou um visitante.  

🔹 **Gandalf na ponte de Khazad-dûm (Senhor dos Anéis):**

  - Ele analisa quem está tentando passar *(requisição)*.  
  - Se for alguém confiável, ele permite a passagem *(`next()`)*.  
  - Se for um inimigo *(requisição errada)*, ele bloqueia o caminho *(`res.status(403)` - acesso negado)*.
  
  <figure style="text-align: center;">
    <img src="https://media.tenor.com/uUnfd6BfpEgAAAAC/you-shall-not-pass.gif" alt="Gandalf falando You Shall Not Pass">    
    <figcaption>Fonte: https://ar.inspiredpencil.com/pictures-2023/gandalf-you-shall-not-pass-gif</figcaption>
  </figure>  

```ts
const porteiroMiddleware = (req: Request, res: Response, next: Function) => {
  console.log(`📢 Requisição recebida em: ${req.url}`);
  next(); // Permite a requisição continuar para a rota
};

app.use(porteiroMiddleware);
```

>✅ Agora **todas** as requisições passarão pelo porteiro antes de chegarem às rotas!  

---

# 📋 **5. Exercícios para Praticar**  

1️⃣ **Crie uma nova rota GET `/sobre` que retorne um JSON com seu nome, idade e uma breve descrição sobre você.**  

2️⃣ **Adicione um middleware que registre a hora exata em que uma requisição foi feita. O log deve exibir algo como:**  
   ```txt
   Requisição feita em: 2025-02-17T18:30:12.345Z
   ```  

3️⃣ **Crie uma rota `POST /comentarios` que receba um JSON com um comentário.**  
   - **Se o campo "texto" estiver vazio, retorne um erro 400.**  
   - **Caso contrário, retorne um status 201 com a mensagem "Comentário recebido!".**  

4️⃣ **Crie uma rota DELETE `/comentarios/:id` que simule a exclusão de um comentário pelo ID.**  
   - **Retorne status 204 se a exclusão for bem-sucedida.**  
   - **Se o ID não for enviado, retorne um erro 400.**  

---

# ✅ **Resumo da Aula**  

✅ Aprendemos **o que são rotas, requisições e respostas** no Express.  
✅ Criamos **rotas GET e POST** para praticar.  
✅ Descobrimos **o que são middlewares e quando usá-los**.  
✅ Fizemos analogias para facilitar o entendimento.  
✅ Criamos uma **lista de exercícios** para praticar o conteúdo.  
