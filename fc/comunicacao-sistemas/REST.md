

### Níveis de maturidade do REST (Richardson Maturity Model)
### Nível 0: The Swamp of POX (Plain old XML)
- Única URI e único verbo HTTP
	- Escolhe um verbo HTTP, normalmente o POST, e passa-se as informações variadas, por exemplo:
Adicionar um produto:
```xml
POST /stock HTTP/1.1
[various other headers]

<addStockProduct>
 <product name="apple" quantity=200 />
</addStockProduct>
```
Buscar informações de um produto:
```xml
POST /stock HTTP/1.1
[various other headers]

<retrieveAppleStockInfo />
```

### Nível 1: Resource URI
- Múltiplos URIs separados por recurso (resource), mas ainda 1 único verbo HTTP (comumente o POST)

```xml
POST /apple HTTP/1.1
[various other headers]

<retrieveStockInfo />

POST /orange HTTP/1.1
[various other headers]

<retrieveStockInfo />
```

```xml
POST /apple HTTP/1.1
[various other headers]

<addStockProduct>
 <product name="apple" quantity=200 />
</addStockProduct>
```

### Nível 2: HTTP verbs
- Agora utiliza-se os verbos HTTP de acordo com o que deseja-se executar no recurso.

| VERBO  | USO                  |
| ------ | -------------------- |
| GET    | Recuperar informação |
| POST   | Inserir informação   |
| PUT    | Alterar informação   |
| DELETE | Remover informação   |
```xml
GET /apple HTTP/1.1
[various other headers]

<retrieveStockInfo />

GET /orange HTTP/1.1
[various other headers]

<retrieveStockInfo />
```

```xml
POST /apple HTTP/1.1
[various other headers]

<addStockProduct>
 <product name="apple" quantity=200 />
</addStockProduct>
```

### Nível 3: HATEOAS (Hypermedia as the engine of the application state)
Agora, acrescenta-se no retorno da informação um atributo que indica quais próximas ações podem ser tomadas pelo usuário.

```JSON
HTTP/1.1 200 OK
Content-Type: application/vdn.acme.account+json
Content-Length: ...
{
	"account": {
		"account_number": 12345,
		"balance": {
			"currency": "usd",
			"value": 100.00
		},
	},
	"links": {
		"deposit": "/accounts/12345/deposit",
		"withdraw": "/accounts/12345/withdraw",
		"transfer": "/accounts/12345/transfer",
		"close": "/accounts/12345/close"
	}
}
```
