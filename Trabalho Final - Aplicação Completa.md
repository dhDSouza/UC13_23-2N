# 📚 Atividade Final de Backend — Projeto Integrador com TypeScript e Express

## 🎯 Objetivo da Atividade

Desenvolver um **sistema backend completo**, utilizando todos os conceitos, tecnologias e boas práticas abordadas ao longo do módulo. A proposta será conduzida através da **metodologia ABP (Aprendizagem Baseada em Projetos)**, onde os estudantes irão **resolver um problema real** por meio da construção de um software funcional.

---

## 💡 Cenário do Projeto

**Contexto:** Uma empresa fictícia chamada **"DevTasker"** deseja um sistema para **gestão de tarefas de equipe**. O sistema será utilizado por equipes de desenvolvimento para organizar tarefas, registrar membros e acompanhar o progresso de projetos.

---

## 🛠️ Requisitos Técnicos Obrigatórios

A aplicação **DEVE** conter os seguintes recursos:

### 🔐 Autenticação e Autorização
- Cadastro e login de usuários (senha criptografada com **bcrypt**)
- Geração de token **JWT** no login
- Middleware para proteger rotas privadas

### 👥 Usuários
- CRUD de usuários
- Campos: `id`, `nome`, `email`, `senha`, `dataDeCriacao`

### 📋 Tarefas
- CRUD de tarefas
- Campos: `id`, `titulo`, `descricao`, `status`, `dataDeEntrega`, `usuarioId`
- Cada tarefa deve estar associada a um usuário

### 📁 Estrutura do Projeto
- Arquitetura bem definida com separação em:
  - `models/`
  - `controllers/`
  - `repositories/`
  - `routes/`
  - `middlewares/`

### 🧠 Banco de Dados
- Utilizar **TypeORM** com banco **MySQL**
- Utilizar o `reflect-metadata`
- Uso do arquivo `.env` para configs sensíveis

---

## 🧪 Critérios de Avaliação

| Critério | Peso |
|---------|------|
| CRUDs funcionais (Usuário + Tarefas) | ⭐⭐⭐⭐⭐ |
| Boas práticas com TypeScript (tipagens) | ⭐⭐ |
| Arquitetura organizada e modularizada | ⭐⭐ |
| Segurança (senhas hash, rotas protegidas) | ⭐⭐⭐ |
| Uso correto do TypeORM | ⭐ |

---

## 🗓️ Cronograma (4 Dias)

| Dia | Atividade |
|-----|-----------|
| **1** | Levantamento dos requisitos em grupo, criação do repositório, setup do projeto, conexão com banco e modelagem das entidades |
| **2** | Implementação das rotas de autenticação e CRUD de usuários |
| **3** | Implementação do CRUD de tarefas, relacionamentos e middleware de autenticação |
| **4** | Refatorações, testes manuais com Postman, revisão em grupo e entrega |

---

## 👨‍👩‍👦 Trabalho em Equipe (ABP)

- Serão formadas **3 equipes**:
  - **2 equipes com 3 integrantes**
  - **1 equipe com 4 integrantes**
- Todos os membros devem contribuir no repositório (será verificado via Git)

---

## 📬 Entrega

- Cada equipe deve realizar a entrega do projeto na **aba "Discussões"** do repositório da turma no GitHub.
- No post da entrega deve constar:
  - O **link do repositório GitHub** da equipe
  - Um **README organizado**, contendo:
    - Descrição do projeto
    - Lista de integrantes
    - Tecnologias utilizadas
    - Instruções de como rodar o projeto
