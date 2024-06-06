
`pnpm add -D vitest`
`pnpm add -D supertest` e `pnpm add -D @types/supertest`

_OBS_: caso use o alias path no `tsconfig` -> `pnpm add -D vite-tsconfig-paths`

explicar os testes unitários do use case, in-memory repository...

`package.json`
```JSON
.
.
.
"scripts": {
	"test": "vitest run",
	"test:watch": "vitest",
	"test:coverage": "vitest run --coverage"
},
.
.
.
```
`vitest run` executa todos os testes somente uma vez
`vitest` executa todos os testes em modo `watch`

**Coverage e `tsconfig` paths**

`vite.config.ts`
```TS
import { defineConfig } from 'vitest/config'
import tsConfigPaths from 'vite-tsconfig-paths'

export default defineConfig({
	plugins: [tsConfigPaths()],
	test: {
		coverage: {
		include: ['src/**'],
		},
	},
})
```

`tsConfigPaths`: serve para o `vitest` conseguir mapear o alias de path no:
`tsconfig.json`
```JSON
.
.
.
"baseUrl": "./",
"paths": {
	"@/*": ["./src/*"] ,
},
.
.
.
```

**Vitest UI**
[Documentação](https://vitest.dev/guide/ui)
## Testes de integração

 `.env.test`
```env
DATABASE_URL="./db/test.db"
```

`/env/index.ts
```TS
.
.
.
import { config } from 'dotenv'

if (process.env.NODE_ENV === 'test') {
	config({ path: '.env.test' })
} else {
	config()
}
.
.
.
```

-  Preparações pré-testes: antes dos testes, devido ao fastify trabalhar somente com assincronismo, esperar o app ficar pronto com o app.ready() e encerrá-lo no fim de tudo com o app.close(). Como também, antes de cada teste limpar o banco (que é o banco de testes) e rodar os migrations para criar as tabelas.
```TS
beforeAll(async () => {
	await app.ready()
})

afterAll(async () => {
	await app.close()
})

beforeEach(() => {
	execSync('npm run knex migrate:rollback --all')
	execSync('npm run knex migrate:latest')
})
```
