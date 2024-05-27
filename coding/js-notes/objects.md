
```js
const user = {
	name: "Vitor Hugo",
	age: 32,
	address: {
		street: "5th Avenue",
		number: 176,
	},
};
```

Method _in_ allows to check if there is a information inside an object, e.g.:
```js
('name' in user) => true
('nickname' in user) => false
```

Method _keys_ returns the keys attributes of an object.
```js
Object.keys(user) => name, idade, address
```

The method _Object.values()_ returns the values of the keys inside the data structure of an Object.

**TAKE NOTE:** Where there is a more complex data structure, the method returns a [object, Object], thus it's necessary to use _JSON.stringfy_ to make it readable.
```js
Object.values(user) => Vitor Hugo, 32, [object Object]
JSON.stringfy(Object.values(user))
// => ["Vitor Hugo", 32, {"5th Avenue", 176,}]
```

Method _Object.entries()_ returns an array of a given object's own enumerable string-keyed property key-value pairs. 
```js
JSON.stringfy(Object.entries(user))
// output
{
	["name", "Vitor Hugo"],
	["age", 32],
	["address", {street: "5th Avenue", number: 176}],
}
```

## Destructuring assignment
```js
const address = user.address
// it's the same
const { address } = user
// and can be used 
const { address, age } = user
```

Thus, it's possible rename a key when destructuring.
```js
const { address, idade: age } = user
```
Now _idade_ is a variable with age value.

It's possible to set a default value to a key when destructuring:
```js
const { address, nickname = "Vituxo" } = user => JSON
	.stringfy({address, nickname})
{
	"address": {
	street: "5th Avenue",
	number: 176,
	},
	"nickname": "Vituxo"
}
```
**TAKE NOTE:** destructured properties can be passed as functions arguments.

## Rest operator (...rest)
The rest operator _(...rest)_ takes the remaining information other than was already destructured, e.g.:
```js
const { name, ...rest } = user
// rest. (access remainder attributes)
```

Destructuring it's also used with arrays and rest operator as well. 
```js
const array = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

const [first, second] = array;
// => first: 1, second: 2
const [ first, second, ...rest ] = array;
// => first: 1, second: 2, rest: [3, 4, 5, 6, 7, 8, 9, 10]
const [first, , third, ...rest ] = array;
// => first: 1, third: 3, rest: [4, 5, 6, 7, 8, 9, 10]
//deixa em branco a posição q deseja pular
```


