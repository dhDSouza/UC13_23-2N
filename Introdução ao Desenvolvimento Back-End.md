# **Aula 1: Introdução ao Desenvolvimento Back-end com TypeScript**  

## 🎯 **Objetivos da Aula**  

- Compreender a diferença entre **Front-end, Back-end e Fullstack**.  
- Introduzir o **TypeScript**, explicando como ele funciona.  
- Configurar o ambiente de desenvolvimento (**VS Code, Node.js, TypeScript e dependências**).  
- Apresentar o **Express.js**, explicando seu funcionamento e importância.  
- Criar o primeiro servidor **com TypeScript e Express.js**.  


# 🏗️ **1. O que é Back-end? E como ele se diferencia do Front-end e Fullstack?**  

O desenvolvimento de software se divide em três principais áreas:  

| Tipo | O que faz? | Exemplos |
|------|-----------|----------|
| **Front-end** | Interface do usuário, interatividade, estilização | HTML, CSS, JavaScript, React, Vue.js |
| **Back-end** | Lógica de negócio, segurança, banco de dados, APIs | Node.js, TypeScript, Express, NestJS |
| **Fullstack** | Trabalha tanto no front-end quanto no back-end | Conhece React e Express, por exemplo |

### 🎭 **Analogia: Restaurante**  

- **Front-end** → O garçom e o cardápio (interação com o cliente).  
- **Back-end** → A cozinha (processamento dos pedidos).  
- **Banco de dados** → O estoque (onde os ingredientes são armazenados).  

No curso, vamos nos especializar no **back-end**, garantindo que o "cozinheiro" do sistema funcione corretamente.  

---

# 🚀 **2. Introdução ao TypeScript**  

### 🔹 **O que é TypeScript?**  

TypeScript é um **superset do JavaScript** que adiciona **tipagem estática** e funcionalidades avançadas, ajudando na escrita de código mais seguro e organizado.  

### 🔄 **Como o TypeScript funciona? (Transpilação para JavaScript)**  

O navegador e o Node.js não entendem TypeScript. Ele precisa ser convertido em JavaScript antes de ser executado. Esse processo é chamado de **transpilação**.  

```ts
// Código TypeScript
let nome: string = "Daniel";
console.log(nome);
```

Após ser "transpilado" pelo TypeScript, vira:  

```js
// Código JavaScript gerado
var nome = "Daniel";
console.log(nome);
```

Isso significa que podemos usar TypeScript sem medo, pois no final **o código será sempre convertido para JavaScript compatível com qualquer ambiente**.  

---

# 🛠 **3. Configuração do Ambiente de Desenvolvimento**  

## 📌 **3.1 Instalando Node.js e NPM**  

O **Node.js** é um ambiente para rodar JavaScript no servidor. O **NPM (Node Package Manager)** é o gerenciador de pacotes que permite instalar bibliotecas.  

🔹 **Passo 1: Baixar e instalar o Node.js**  
Acesse: [https://nodejs.org/](https://nodejs.org/) e baixe a versão **LTS**.  

🔹 **Passo 2: Verificar instalação**  
Após instalar, abra o terminal e execute:  
```bash
node -v
npm -v
```
Se aparecer um número de versão, significa que está funcionando corretamente! 🎉  

---

## 🖥 **3.2 Instalando e Configurando o VS Code**  

🔹 **Baixar o VS Code:**  
[https://code.visualstudio.com/](https://code.visualstudio.com/)  

🔹 **Extensões recomendadas:**  
- **ESLint** → Para manter um código padronizado.  
- **Prettier** → Para formatar código automaticamente.  
- **REST Client** → Para testar APIs sem precisar do Postman.  
- **Material Icon Theme** → Para melhorar a visualização de arquivos.  

---

## 📁 **3.3 Criando o Primeiro Projeto com TypeScript**  

1️⃣ **Criar uma pasta para o projeto e acessar ela:**  
```bash
mkdir meu-backend && cd meu-backend
```

2️⃣ **Inicializar um projeto Node.js:**  
```bash
npm init -y
```

3️⃣ **Instalar o TypeScript no projeto:**  
```bash
npm install typescript ts-node @types/node -D
```

4️⃣ **Criar o arquivo de configuração do TypeScript:**  
```bash
npx tsc --init
```

Isso gera um arquivo `tsconfig.json`, que controla a transpilação do TypeScript.  

### 🔹 **Explicação do `tsconfig.json`**  
Vamos modificar algumas configurações para otimizar o projeto:  

```json
{
  "target": "ES6",
  "module": "CommonJS",
  "outDir": "./dist",
  "rootDir": "./src",
  "strict": true
}
```

- **target** → Define para qual versão do JavaScript o TypeScript vai transpilar.  
- **module** → Define o sistema de módulos usado (CommonJS para Node.js).  
- **outDir** → Define para onde os arquivos transpilados vão ser salvos.  
- **rootDir** → Define onde os arquivos TypeScript estão localizados.  
- **strict** → Ativa verificações mais rígidas no código.  

---

# 🌐 **4. O que é o Express.js?**  

O **Express.js** é um framework minimalista para Node.js que facilita a criação de servidores e APIs.  

### ✅ **Por que usar Express?**  

- Simples e rápido.  
- Permite criar APIs REST de forma fácil.  
- Possui um grande ecossistema e comunidade ativa.  

---

# 🚀 **5. Criando um Servidor com TypeScript e Express**  

## 📌 **5.1 Instalando o Express**  

🔹 **Instalar o Express e suas tipagens**:  
```bash
npm install express  
npm install @types/express -D
```

**Mas por que instalamos `@types/express`?**  

O Express é escrito em JavaScript, mas estamos usando TypeScript. O pacote `@types/express` fornece os tipos necessários para que o TypeScript entenda o Express corretamente.  

---

## 📁 **5.2 Criando a estrutura do projeto**  

```
meu-backend/
│── src/
│   ├── server.ts
│── tsconfig.json
│── package.json
```

Agora, no arquivo **`src/server.ts`**, adicione o seguinte código:  

```ts
import express from 'express';

const app = express();
const PORT = 3000;

// Middleware para permitir que o Express interprete JSON
app.use(express.json());

app.get('/', (req, res) => {
  res.send('🚀 Servidor TypeScript rodando!');
});

app.listen(PORT, () => {
  console.log(`🔥 Servidor rodando em http://localhost:${PORT}`);
});
```

### 📝 **Explicação do código**  

1️⃣ **Importamos o Express** → `import express from 'express'`  
2️⃣ **Criamos a aplicação** → `const app = express();`  
3️⃣ **Definimos a porta** → `const PORT = 3000;`  
4️⃣ **Habilitamos o middleware `express.json()`** → Para aceitar JSON no corpo das requisições.  
5️⃣ **Criamos uma rota GET `/`** → Quando alguém acessar a raiz, retornamos uma mensagem.  
6️⃣ **Iniciamos o servidor** → `app.listen(PORT, () => { ... })`  

---

# 🏆 **Recapitulando**  

✅ Aprendemos as diferenças entre **Front-end, Back-end e Fullstack**.  
✅ Compreendemos o que é **TypeScript e como ele funciona**.  
✅ Configuramos **o ambiente de desenvolvimento (VS Code, Node.js, TypeScript)**.  
✅ Criamos um servidor usando **Express.js e TypeScript**.  

---

# 🎯 **Próxima Aula**  

Na próxima aula, vamos aprofundar em **rotas, middlewares e requisições HTTP no Express**!  

🚀 **Desafio:**  
- Tente criar uma nova rota no servidor que retorne seu nome.  
- Experimente mudar a porta do servidor.
