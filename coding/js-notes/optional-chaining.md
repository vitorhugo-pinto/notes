Allows to check if a attribute or method can be accessed if it's possible for its values to be _null_ or _undefined_.

**SYMBOL:** _?._

```js
const user = {
	name: 'Vitor Hugo',
	idade: 32,
	address: {
		street: '5th avenue',
		number: 176,
		zip: {
			code: '59000123'
		},
		showFullAddress() {
			return 'ok';
		},
	},
};

user.address?.zip?.code ?? "Not informed";
```

In this example if the address object doesn't exists the message _"Not informed"_ will be displayed.

```js
user.address?.showFullAddress?.()
```

**TAKE NOTE:** can be used for methods of an object as well.