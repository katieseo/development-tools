## Array

#### Creating *

const myArr = new Array(3) // [empty x 3] sparse array, cannot iterate over
const myArr = [...new Array(3) // [undefined, undefined, undefined] populate the new array properly 

const myArr = Array.from('foo') // ["f", "o", "o"]
const myArr = Array.from([1, 2, 3], x => x + x) // [2, 4, 6]

#### compare *
function(a,b){return a-b}/define an alternative sort order (return a negative, sero, or positive value)


#### Iterate *
forEach( (item, index) => do something ) // applies some operation with side effects to each list items, no return
map // transforms each items and return another list with transformed items

#### filter *

#### sorting *

myArr.sort(); //array alphabetically
myArr.reverse(); // descending order

myArr.sort((a, b) => a-b) // numeric sort

#### Shuffle *
```js
myArr.sort(() => Math.random() - 0.5);

// (optimized version of Fisher-Yates) *
// Durstenfeld shuffle *
function shuffleArray(array) {
    for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
    }
}
```

## Object

#### Iterate *

Object.keys(myObj).map((key, index) => myObj[key] *= 2);
for (const propName in myObj) { }

#### return Obj keys arr
```js
const object1 = {
  a: 'somestring',
  b: 42,
  c: false
};

console.log(Object.keys(object1));
// expected output: Array ["a", "b", "c"]
```

#### Using Object.keys
* returns an array whose elements are strings corresponding to the enumerable properties found directly upon object
```js
const object1 = {
  a: 'somestring',
  b: 42,
  c: false
};
console.log(Object.keys(object1)); // expected output: Array ["a", "b", "c"]

// simple array
const arr = ['a', 'b', 'c'];
console.log(Object.keys(arr)); // console: ['0', '1', '2']

// array-like object
const obj = { 0: 'a', 1: 'b', 2: 'c' };
console.log(Object.keys(obj)); // console: ['0', '1', '2']

// array-like object with random key ordering
const anObj = { 100: 'a', 2: 'b', 7: 'c' };
console.log(Object.keys(anObj)); // console: ['2', '7', '100']

// getFoo is a property which isn't enumerable
const myObj = Object.create({}, {
  getFoo: {
    value() { return this.foo; }
  }
});
myObj.foo = 1;
console.log(Object.keys(myObj)); // console: ['foo']

// In ES2015+ (before: TypeError)
Object.keys('foo');  // ["0", "1", "2"]

```


## String

#### returns the character at the specified index in a string *
```js
myStr.charAt(0);   //'H'ello
```


## Math

#### random number *
Math.floor(Math.random() * 10);   // 0-9
Math.floor(Math.random() * 10) + 1;   // 1-10

Math.floor(Math.random() * (max - min) ) + min; // getRndInteger (max excluded)


## Number

#### Integer *
Number.isInteger(3) // Returns a random integer from 0 to 9:



## DOM

#### Make a list *
const ul = document.createElement('ul');
document.body.appendChild(ul);
ul.addEventListener('click', hide, false);

#### event.target *
event.target.style.visibility = 'hidden';



