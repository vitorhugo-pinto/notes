- NodeJS doesn't run TS natively, therefore some actions must be fulfilled to run fastify with node:
	1. install the following libs as dev dependencies:
		- `npm install -D typescript`
		- `npm install -D @types/node`
		- `npm install -D tsx`
	2. Now, to run the server during development add the following script in the `package.json` file:
```JSON 
"scripts": {
	"dev": "tsx watch src/server.ts"
},
```
- The `watch` flag makes the server reload automatically on saving changed files.

	3. Finally, just execute `npm run dev` to run the project in dev mode.