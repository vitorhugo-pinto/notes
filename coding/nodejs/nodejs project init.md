`pnpm init` -> cria o `package.json`

**Ambiente para TS**
Adiciona as libs necessárias para rodar o projeto completamente com TS
`pnpm add -D typescript tsup tsx @types/node`
- `typescript`: adiciona o TS de fato ao projeto.
- `@types/node`: adiciona os tipos do node.
- `tsx`: lib para rodar o projeto localmente com TS.
- `tsup`: lib para converter o código TS em JS para fazer a build do projeto, necessário no momento de subir o projeto para hospedar.

- Adicionar no `package.json` os seguinte scripts:
```JSON
"scripts": {
	"dev": "tsx watch src/server.ts",
	"start": "node build/server.js",
	"build": "tsup src --out-dir build"
},
```

**Configurar o TS**
De dentro do pacote do typescript, executar o `tsc` para gerar o `tsconfig.json` com o seguinte comando (`pnpm exec tsc --init `) e alterar o `"target": "ES2022"`, ou mais atual. 

**Lib para gerenciamento do servidor node**
`pnpm add fastify`

**Cria-se dois arquivos inicialmente:**
- `app.ts`: onde faz as configurações do app (register ...)
```TS
import fastify from 'fastify';

export const app = fastify()
```
- `server.ts`: apensar para configurar o host e a porta da aplicação e acioná-la
```TS
import { app } from "./app";

app.listen({
	host: '0.0.0.0',
	port: 3333
}).then(() => {
	console.log('👻 Boo')
})
```
## Environment variables
Adiciona a lib `pnpm add dotenv` para fazer o carregamento das variáveis de ambiente para dentro do projeto.

**Arquivo de configuração das variáveis - `.env`**
```.env
NODE_ENV=dev
```
Para acessar as variáveis no código usa-se `process.env`

- Validação das variáveis de ambiente em `src/env/index.ts`
```TS
import 'dotenv/config'
import {z} from 'zod'

const envSchema = z.object({
	NODE_ENV: z.enum(['dev', 'test', 'prod']).default('dev'),
	PORT: z.coerce.number().default(3333)
})

const _env = envSchema.safeParse(process.env)

if(!_env.success) {
	console.error('-- INVALID ENV VARIABLES --', _env.error.format())
	throw new Error('Invalid env')

}

export const env = _env.data
```

**ESLint**
`pnpm add -D eslint`
**Aprender a configurar o ESLint 9.x.x**
