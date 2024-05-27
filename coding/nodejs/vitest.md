`npm i -D vitest`
`npm i -D supertest` e `npm i -D @types/supertest`

-  `.env.test`
```env
DATABASE_URL="./db/test.db"
```
-  `/env/index.ts
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
