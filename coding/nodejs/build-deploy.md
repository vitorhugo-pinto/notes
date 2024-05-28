`npm i -D tsup`

`package.json`
```JSON
.
.
.
"scripts": {
	"build": "tsup src --out-dir build"
},
.
.
.
```
no render.com

build command:
`npm install && npm run knex -- migrate:latest && npm run build`

start command:
`node build/server.js`

advanced -> env variable:
`DATABASE_CLIENT pg`
`DATABE_URL <path from render dashboard`
