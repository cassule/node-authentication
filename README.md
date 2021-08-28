# Projecto Backend de Autenticação

Passo a passo de como montar uma autenticação decente.

**Criando Projecto Com Node**

1. Abrir o terminal e digitar `yarn init` para iniciar o projecto com Node

- Estamos adicionar typescript como dependência de desenvolvimento `yarn add typescript -D`
- Para iniciar o typescript e criar o tsconfig.json `yarn tsc --init`
- No arquivo tsconfig.json vamos setar o rootDir para src e o outDir para a pasta dist.

2. Vamos adicionar as dependência básicas que vamos usar nesse projecto `yarn add express`

- Vamos adicionar os types do express `yarn add @types/express`

3. Configurar as rotas da aplicação

- Vamos criar um arquivo na pasta src/server.js
  ``
  import express from 'express';

  const app = express();

  app.get('/sessions', (request, response) => {

      const { email, password } = request.body;


      return response.status(200).json({ message: 'success'});

  });

  app.listen(3333, () => {
  console.log('listen in port 3333');
  });
  ``

  - Até aqui já temos uma aplicação básica com Node e Typescript funcional
  - Vamos add como dependência de desenvolvimento o o ts node-dev `yarn add ts-node-dev -D`
  - Abrir o arquivo package.json e criar um script dev:server: `ts-node-dev --transpile-only --ignore node_modules src/node_modules`

4. Vamos agora conectar a nossa aplicação com o banco de dados.

- Usar docker: https://www.notion.so/Instalando-Docker-6290d9994b0b4555a153576a1d97bee2
- `yarn add typeorm pg` para configurarmos o typeorm precisamos criar um arquivo no dir raiz ormconfig.json

` { type: "postgres", host: "localhost", port: 5432, username: "root", password: "root", database: "backend" } `

- Vamos criar um diretório database em src e vamos criar um arquivo index.js
  ``
  import { createConnection } from 'typeorm';

  createConnection();

  ``

- No nosso arquivo express vamos adicionar o a nossa conexão com o banco de dados.
  ` import './database'`
  e se rodarmos no terminal yarn dev:server a nossa aplicação já estará conectada ao banco de dados.

- Vamos configurar as migrations:
  Na pasta database vamos criar a pasta migrations
  ` { migrations: [ "./src/database/migrations/*.ts" ], "cli": { "migrationDir": "./src/database/migrations" } }`

- Editar o package.json em script adicionar
  ` "typeorm": "ts-node-dev node_modules/typeorm/cli.js"`

- Vamos criar uma migration: Explicar as vantagens da migration
  `yarn typeorm migration:create -n CreateUser`
