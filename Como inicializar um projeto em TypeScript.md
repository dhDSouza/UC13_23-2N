# Como iniciar um novo projeto em TypeScript

- Abra o terminal e digite os seguintes comandos

```bash
npm init -y
npm install express
npm install typescript ts-node-dev @types/node @types/express -D
npx tsc --init
```

>[!WARNING]
>O último comando irá gerar um arquivo chamado `tsconfig.json`.

- Altere o código do arquivo `tsconfig.json` para o seguinte:

```json
{
  "compilerOptions": {
    "target": "ES6",
    "module": "CommonJS",
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true
  },
  "include": ["src/**/*.ts"],
  "exclude": ["node_modules"]
}
```

**🚀 Agora é só partir pro código da sua aplicação!**
