# Backend
## [Fastify](https://www.fastify.io/)
Web framework for NodeJS
### Fastify and cors configuration
```PowerShell
npm i fastify
```

**TAKE NOTE:**_@fastify/cors_ is a middleware, thus it's necessary to use _register()_ method.
```PowerShell
npm i @fastify/cors
```

```ts
import cors from "@fastify/cors";
import Fastify from "fastify";

export app = Fastify();
app.resgister(cors);
```

## [Prisma ORM](https://www.prisma.io/) 
Deal with database with intuitive data model, automated migrations, type-safety e auto-completion.

**TAKE NOTE: Developer: Reload Window**
	Some times VSCode has a hard time updating the interface with Prisma information, if that's the case, open Command Palette (Ctrl+Shift+P) and run _Developer: Reload Window_

_Executes migrations_
```powershell
npx prisma migrate dev
```

_Graphic interface to manage the database_
```powershell
npx prisma studio
```

_Community library for ER Diagram_
```powershell
npm i -D prisma-erd-generator @mermaid-js/mermaid-cli
```

_Generates a ER Diagram with tables relationships_
```powershell
npx prisma generate
```

_Setup configuration_
_In the file ->_ "/prisma/schema.prisma"
```js
generator erd {
  provider = "prisma-erd-generator"
}
```

_seed.ts_ file that seeds the database with made up data.
_In the file ->_ "./package.json"
```js
"prisma": {
	"seed": "tsx prisma/seed.ts"
}
```

*Then runs:*
```PowerShell
npx prisma db seed
```


## Endpoints routes
### _request:_ what it passed within the request
- _body_: JSON data.
- _params:_ within the URL -> '/resource/:id'
- _query:_ within the URL, using the characters ? and &.
	usually used for filters or pagination:
	-> /resource?filterAttribute=value&anotherFilterAttribute=value
	-> /resource?page=value&amount=value


## [Zod](https://zod.dev/) - Data validation
TypeScript schema validation with static type inference
```PowerShell
npm i zod
```

_Example: creating an object schema_
```ts
import { z } from "zod";

const User = z.object({
	username: z.string(),
});

User.parse({ username: "Ludwig" }); 
// extract the inferred type type
User = z.infer<typeof User>; 
// { username: string }
```


