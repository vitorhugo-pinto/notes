It is a logical operator to deal with _null_ or _undefined_ values.

**SYMBOL:** _??_

```js
const nullValue = null;
const emptyText = ""; // falsy
const someNumber = 42;

const valA = nullValue ?? "default for A";
const valB = emptyText ?? "default for B";
const valC = someNumber ?? 0;

console.log(valA); // "default for A"
console.log(valB); // "" (as the empty string is not null or undefined)
console.log(valC); // 42
```

**TAKE NOTE:** for JS, values such as 0, "", [], undefined, null are all _"falsy"_, invalid values. So the _??_ discards null and undefined, but allows "empties" values, such as 0, "" and [].
Great alternative to _OR_ (||) operator.
