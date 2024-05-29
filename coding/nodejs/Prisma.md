# [Prisma](https://www.prisma.io/)
`pnpm add -D prisma`
-D pois é uma ferramenta CLI para executar os comandos do Prisma
`pnpm prisma init`
Next steps:
1. Set the DATABASE_URL in the .env file to point to your existing database. If your database has no tables yet, read https://pris.ly/d/getting-started
2. Set the provider of the datasource block in schema.prisma to match your database: postgresql, mysql, sqlite, sqlserver, mongodb or cockroachdb.
3. Run prisma db pull to turn your database schema into a Prisma schema.
4. Run prisma generate to generate the Prisma Client. You can then start querying your database.

`prisma/schema.prisma`

```Prisma
model User {
	id String @id @default(uuid())
	name String
	email String @unique
	
	@@map("users")
}
```
model = tabela no banco
`@@` para configurar algo sobre a tabela
`@` para configurar algo sobre atributo/coluna da tabela

`pnpm add @prisma/client`: cliente que gerencia e executa nossas necessidade com o banco dentro da aplicação

`pnpm exec prisma generate`: aplica as alterações do arquivo `prisma/schema.prisma`