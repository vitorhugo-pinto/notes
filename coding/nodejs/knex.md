## Migrations
`package.json`
```JSON
"scripts": {
  "knex": "node --no-warnings --import tsx ./node_modules/knex/bin/cli.js"
},
```
Arquivo de configuração `./src/database.ts`
```TS
import { knex as knexConfig, Knex } from 'knex'

export const config: Knex.Config = {
	client: 'sqlite',
	connection: {
	filename: './db/app.db',
	},
	useNullAsDefault: true,
	migrations: {
		extension: 'ts',
		directory: './db/migrations',
	},
}

export const knex = knexConfig(config)
```
E então, na raiz do projeto, cria o arquivo `knexfile.ts`
```TS
import { config } from './src/database'

export default config
```
Para rodar os comandos knex:
`npm run knex -- <comandos>`
- Ex: `npm run knex -- migrate:make <nome do arquivo>`
- rodar as migrations: `npm run knex -- migrate:latest`
- desfazer um migrations: `npm run knex -- migrate:rollback` -> só deve ser utilizado caso não tenha repassado o migrations pra outras equipes ou ambientes.

## Tipagem de Tables
[Documentação](https://knexjs.org/guide/#typescript)
Em `src/@types/knex.d.ts`
```TS
// eslint-disable-next-line
import { Knex } from 'knex'

declare module 'knex/types/tables' {
	export interface Tables {
		transactions: {
		id: string
		title: string
		amount: number
		created_at: string
		session_id?: string
		}
	}
}
```
Para qualquer nova tabela ou nova informação sobre a tabela, adiciona-se as informações de tipagem nesse arquivo.