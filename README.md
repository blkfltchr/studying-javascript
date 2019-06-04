# Studying javascript

## **() => {} arrow functions**

There's a very simple and concise syntax for creating functions, that’s often better than Function Expressions. It’s called “arrow functions”, because it looks like this:

    let func = (arg1, arg2, ...argN) => expression
    
    // it’s roughly the same as:
    
    let func = function(arg1, arg2, ...argN) {
      return expression;
    };
    
    // …But much more concise.

If we have only one argument, then parentheses can be omitted, making that even shorter. If there are no arguments, parentheses should be empty (but they should be present):

    // One argument: let double = function(n) { return n * 2 }
    let double = n => n * 2;
    
    alert( double(3) ); // 6
    
    // No arguments: let sayHi = function() { alert("Hello!") }
    let sayHi = () => alert("Hello!");
    
    sayHi();

### **🚨Arrow functions VS bind**

There’s a subtle difference between an arrow function `=>` and a regular function called with `.bind(this)`:

- `.bind(this)` creates a “bound version” of the function.
- The arrow `=>` doesn’t create any binding. The function simply doesn’t have `this`. The lookup of `this` is made exactly the same way as a regular variable search: in the outer lexical environment.

## Rest parameters **`...args`**

A function can be called with any number of arguments, no matter how it is defined. There will be no error because of “excessive” arguments. But of course in the result only the first two will be counted. Like here:

    function sum(a, b) {
      return a + b;
    }
    
    alert( sum(1, 2, 3, 4, 5) );

The rest parameters can be mentioned in a function definition with three dots `...`. They literally mean “gather the remaining parameters into an array”. For instance, to gather all arguments into array `args`:

    function sumAll(...args) { // args is the name for the array
      let sum = 0;
    
      for (let arg of args) sum += arg;
    
      return sum;
    }
    
    alert( sumAll(1) ); // 1
    alert( sumAll(1, 2) ); // 3
    alert( sumAll(1, 2, 3) ); // 6

We can choose to get the first parameters as variables, and gather only the rest. Here the first two arguments go into variables and the rest go into `titles` array:

    `function showName(firstName, lastName, ...titles) {
      alert( firstName + ' ' + lastName ); // Julius Caesar
    
      // the rest go into titles array
      // i.e. titles = ["Consul", "Imperator"]
      alert( titles[0] ); // Consul
      alert( titles[1] ); // Imperator
      alert( titles.length ); // 2
    }
    
    showName("Julius", "Caesar", "Consul", "Imperator");
    The rest parameters must be at the end`

## Spread Operator **`...arr`**

Let’s say we have an array `[3, 5, 1]`. How do we call `Math.max`with it?

Passing it “as is” won’t work, because `Math.max` expects a list of numeric arguments, not a single array:

    `let arr = [3, 5, 1];
    
    alert( Math.max(arr) ); // NaN`

*Spread operator* to the rescue! It looks similar to rest parameters, also using `...`, but does quite the opposite. When `...arr` is used in the function call, it “expands” an iterable object `arr` into the list of arguments. For `Math.max`:

    let arr = [3, 5, 1];
    
    alert( Math.max(...arr) ); // 5 (spread turns array into a list of arguments)

We can 1) pass multiple iterables this way, 2) combine the spread operator with normal values, and 3) use the spread operator to merge arrays:

    // multiple iterables
    
    let arr1 = [1, -2, 3, 4];
    let arr2 = [8, 3, -8, 1];
    
    alert( Math.max(...arr1, ...arr2) ); // 8
    
    // multiple iterables and a normal value
    
    let arr1 = [1, -2, 3, 4];
    let arr2 = [8, 3, -8, 1];
    
    alert( Math.max(1, ...arr1, 2, ...arr2, 25) ); // 25
    
    // to merge arrays:
    
    let arr = [3, 5, 1];
    let arr2 = [8, 9, 15];
    
    let merged = [0, ...arr, 2, ...arr2];
    
    alert(merged); // 0,3,5,1,2,8,9,15 (0, then arr, then 2, then arr2)

In the examples above we used an array to demonstrate the spread operator, but any iterable will do.

    let str = "Hello";
    
    alert( [...str] ); // H,e,l,l,o

### Rest parameters vs. spread operator

- When `...` is at the end of function parameters, it’s “rest parameters” and gathers the rest of the list of arguments into an array.
- When `...` occurs in a function call or alike, it’s called a “spread operator” and expands an array into a list.

## **Destructuring**

An example of how the array is destructured into variables:

    `// we have an array with the name and surname
    let arr = ["Ilya", "Kantor"]
    
    // destructuring assignment
    let [firstName, surname] = arr;
    
    alert(firstName); // Ilya
    alert(surname);  // Kantor`

Now we can work with variables instead of array members. It looks great when combined with `split` or other array-returning methods:

    `let [firstName, surname] = "Ilya Kantor".split(' ');`

The destructuring assignment also works with objects. The basic syntax is:

    let options = {
      title: "Menu",
      width: 100,
      height: 200
    };
    
    let {title, width, height} = options;
    
    alert(title);  // Menu
    alert(width);  // 100
    alert(height); // 200

If an object or an array contain other objects and arrays, we can use more complex left-side patterns to extract deeper portions.

    let options = {
      size: {
        width: 100,
        height: 200
      },
      items: ["Cake", "Donut"],
      extra: true    // something extra that we will not destruct
    };
    
    // destructuring assignment on multiple lines for clarity
    let {
      size: { // put size here
        width,
        height
      },
      items: [item1, item2], // assign items here
      title = "Menu" // not present in the object (default value is used)
    } = options;
    
    alert(title);  // Menu
    alert(width);  // 100
    alert(height); // 200
    alert(item1);  // Cake
    alert(item2);  // Donut

## Constructor, operator `new`

Often we need to create many similar objects, like multiple users or menu items and so on. That can be done using constructor functions and the `"new"` operator.

Constructor functions technically are regular functions. There are two conventions though:

1. They are named with capital letter first.
2. They should be executed only with `"new"` operator.

For instance:

    `function User(name) {
      this.name = name;
      this.isAdmin = false;
    }
    
    let user = new User("Jack");
    
    alert(user.name); // Jack
    alert(user.isAdmin); // false`

When a function is executed as `new User(...)`, it does the following steps:

1. A new empty object is created and assigned to `this`.
2. The function body executes. Usually it modifies `this`, adds new properties to it.
3. The value of `this` is returned.

In other words, `new User(...)` does something like:

    function User(name) {
      // this = {};  (implicitly)
    
      // add properties to this
      this.name = name;
      this.isAdmin = false;
    
      // return this;  (implicitly)
    }

We can add to this not only properties, but methods as well. For instance, `new User(name)` below creates an object with the given `name` and the method `sayHi`:

    function User(name) {
      this.name = name;
    
      this.sayHi = function() {
        alert( "My name is: " + this.name );
      };
    }
    
    let john = new User("John");
    
    john.sayHi(); // My name is: John
    
    /*
    john = {
       name: "John",
       sayHi: function() { ... }
    }
    */

## Sort

The method arr.sort sorts the array in place.

    let arr = [ 1, 2, 15 ];
    
    // the method reorders the content of arr (and returns it)
    arr.sort();
    
    alert( arr );  // 1, 15, 2

The order became `1, 15, 2`. Incorrect. But why? **The items are sorted as strings by default.** Literally, all elements are converted to strings and then compared. So, the lexicographic ordering is applied and indeed `"2" > "15"`. To use our own sorting order, we need to supply a function of two arguments as the argument of `arr.sort()`. The function should work like this:

    function compareNumeric(a, b) {
      if (a > b) return 1;
      if (a == b) return 0;
      if (a < b) return -1;
    }
    
    let arr = [ 1, 15, 2, 22, 11, 14 ];
    
    // the following does the same as arr.sort(compareNumeric);
    arr.sort( (a, b) => a - b );
    
    console.log(arr);  // 1, 2, 11, 14, 15, 22

## Array. methods

### splice

How to delete an element from the array? The [arr.splice(str)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice) method is a swiss army knife for arrays. It can do everything: insert, remove and replace elements. The syntax is:

    arr.splice(index[, deleteCount, elem1, ..., elemN])

It starts from the position `index`: removes `deleteCount` elements and then inserts `elem1, ..., elemN` at their place. Returns the array of removed elements.

    // deletion
    
    let arr = ["I", "study", "JavaScript"];
    
    arr.splice(1, 1); // from index 1 remove 1 element
    
    alert( arr ); // ["I", "JavaScript"]
    
    // delete 3, replace with 2
    
    let arr = ["I", "study", "JavaScript", "right", "now"];
    
    // remove 3 first elements and replace them with another
    arr.splice(0, 3, "Let's", "dance");
    
    alert( arr ) // now ["Let's", "dance", "right", "now"]
    
    // return deleted elements
    
    let arr = ["I", "study", "JavaScript", "right", "now"];
    
    // remove 2 first elements
    
    let removed = arr.splice(0, 2);
    
    alert( removed ); // "I", "study" <-- array of removed elements
    
    // insert the elements without any removals (set deleteCount to 0)
    
    let arr = ["I", "study", "JavaScript"];
    
    // from index 2
    // delete 0
    // then insert "complex" and "language"
    arr.splice(2, 0, "complex", "language");
    
    alert( arr ); // "I", "study", "complex", "language", "JavaScript"

### slice

The method [arr.slice](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice) is much simpler than similar-looking `arr.splice`. The syntax is:

    arr.slice(start, end)

It returns a new array containing all items from index "start" to "end" (not including "end"). Both `start` and `end` can be negative, in that case position from array end is assumed.

    let str = "test";
    let arr = ["t", "e", "s", "t"];
    
    alert( str.slice(1, 3) ); // es
    alert( arr.slice(1, 3) ); // e,s
    
    alert( str.slice(-2) ); // st
    alert( arr.slice(-2) ); // s,t

### concat

The method [arr.concat](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/concat) joins the array with other arrays and/or items. It accepts any number of arguments – either arrays or values. The result is a new array containing items from `arr`, then `arg1`, `arg2` etc. For instance:

    let arr = [1, 2];
    
    // merge arr with [3,4]
    alert( arr.concat([3, 4])); // 1,2,3,4
    
    // merge arr with [3,4] and [5,6]
    alert( arr.concat([3, 4], [5, 6])); // 1,2,3,4,5,6
    
    // merge arr with [3,4], then add values 5 and 6
    alert( arr.concat([3, 4], 5, 6)); // 1,2,3,4,5,6

### filter

The `find` method looks for a single (first) element that makes the function return `true`. If there may be many, we can use [arr.filter(fn)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter). The syntax is similar to `find`, but filter continues to iterate for all array elements even if `true` is already returned:

    let users = [
      {id: 1, name: "John"},
      {id: 2, name: "Pete"},
      {id: 3, name: "Mary"}
    ];
    
    // returns array of the first two users
    let someUsers = users.filter(item => item.id < 3);
    
    alert(someUsers.length); // 2

### map

The [arr.map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) method is one of the most useful and often used. It calls the function for each element of the array and returns the array of results. For instance, here we transform each element into its length:

    let lengths = ["Bilbo", "Gandalf", "Nazgul"].map(item => item.length);
    alert(lengths); // 5,7,6

### reduce

When we need to iterate over an array – we can use `forEach`. When we need to iterate and return the data for each element – we can use `map`. [arr.reduce](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce) [i](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduceRight)s used to calculate a single value based on the array. Here we get a sum of array in one line:

    `let arr = [1, 2, 3, 4, 5];
    
    let result = arr.reduce((sum, current) => sum + current, 0);
    
    alert(result); // 15`

The calculation flow:

![](https://javascript.info/article/array-methods/reduce.png)