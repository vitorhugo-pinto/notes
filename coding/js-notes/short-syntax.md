When declaring an object and its atributes matches the variables that are gonna be assigned JS allows to omit the value.

e.g.:
```js
const name = 'Vitor Hugo';
const age = 32;

const user = {
	name: name,
	age: age,
};

// It's the same as:

const user = {
	name,
	age,
};
```

