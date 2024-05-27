# Array methods
Given this array:
```js
const array = [1, 2, 3, 4, 5];
```
There are some ways to iterate through the elements in the array.

## The usual for loop
```js
for (const item of array) {
	console.log(item); 
	//output: 1, 2, 3, 4, 5
}
```
But, instead of looping with for/in or for/of it is possible to use some array methods to do the same task.

## array.forEach()
```js
array.forEach(item => {
	console.log(item);
	//output: 1, 2, 3, 4, 5
})
```
**TAKE NOTE:** although it is assumed this function to be immutable, it is possible to mutate the original array with a callback argument.

## array.map()
The map() method returns a new array transformed by the callback function passed as argument, must have a return.
```js
const mappedArray = array.map(item => {
	return item * 2;
})
console.log(mappedArray) // -> [2, 4, 6, 8, 10]
```

## array.filter()
Filters a array returning the values that passes the test in its argument.
```js
const filteredArray = array.filter(item => item % 2 === 0);
console.log(filteredArray) // -> [2,4];
```
**TAKE NOTE 1:** filter() doesn't mutate array values.
**TAKE NOTE 2:** it's possible to concatenate filter() with other methods, e.g.:
```js
const filteredArrayThenMapped = array
	.filter(item => item % 2 === 0).map(item => item * 2);
filteredArrayThenMapped // -> [4,8];
```

## array.every()
Executes a function for each array element and returns true if the returns true for all elements and false otherwise.
**TAKE NOTE:** immutable function.
```js
const allElementsAreNumbers = array.every(item => typeof item === 'number');
console.log(allElementsAreNumbers) // -> true
```

## array.some()
Executes a function for each array element. Returns true (and stops) if the function returns true for one of the array elements and false otherwise.
**TAKE NOTE:** immutable function.
```js
const allElementsAreNumbers = array.some(item => typeof item !== 'number');
console.log(allElementsAreNumbers) // -> false
```


## array.find()
Finds first element that satisfies given condition, otherwise returns undefined.
**TAKE NOTE:** Finds **_FIRST_** element given the array order.
```js
const findsFirstEvenNumber = array.find(item => item % 2 === 0);
console.log(findsFirstEvenNumber) // -> 2
```

## array.findIndex()
Finds first element that satisfies given condition and returns its index value, otherwise returns -1.
**TAKE NOTE:** Finds **_FIRST_** element given the array order.
```js
const indexOf = array.findIndex(item => item % 2 === 0);
console.log(indexOf) // -> 1 (The number 2 is allocated at index 1 in the array)
```

## array.reduce()
Main idea when using _reduce()_ is to return a single value out of a array that satisfies passing a function with the logic needed.
Passes two arguments in the function, first is the function with  _(accumulator, item)_ => { } and the initial value. 
```js
const sum = array.reduce((accumulator, item) => {
	return accumulator + item;
}, 0);
console.log(sum) // -> 15
```

**TAKE NOTE 1:** The argument following the function is the initial value, the initial value that _reduce()_ function will sum over.
**TAKE NOTE 2:** must return in the function called the new accumulated value for its next iteration.
