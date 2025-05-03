tags:: JavaScript, Data Structures, Algorithms, No Starch Press, Programming Books, Chapter

- title:: Using JavaScript
- chapter:: 1
- book:: [[Data Structures and Algorithms in JavaScript]]
- # 1 Using JavaScript
	- We’ll start with an exploration of some modern JavaScript features that 
	  will simplify coding: arrow functions, classes, spreading values, 
	  destructuring, and modules. This list isn’t exhaustive, and we’ll look 
	  at other features in later chapters, including functional programming, 
	  map/reduce and similar array methods, functions as first-class objects, 
	  recursion, and more. We certainly can’t cover all of the language’s 
	  features, but here the focus is on the most important and newer features
	   that are used throughout the book.
	- ## Arrow Functions
		- There are many ways to specify fx in JavaScript
			- Named functions -> `function alpha() {...}`
			- Nameless function expressions -> `const bravo = function () {...}`
			- Named function expressions -> `const charlie = function something() {...}`
			- Function constructors -> `const delta = new Function()`
			- Arrow functions -> `const echo = () => {...}`
		- Arrow functions are different b/c:
			- They may return a value w/out a `return` statement
			- They cannot be used as constructors or generators
			- They don't bind the `this` value
			- They don't have an `arguments` object or a `prototype` object
		- #### Example `arrow1.js`
			- This will be used a lot in this book (first argument):
			- ```javascript
			  // arrow1.js
			  const _getHeight = (tree) => (isEmpty(tree) ? 0: tree.height);
			  ```
				- Given a `tree` arg, this fx returns 0 if the tree is empty, otherwise it returns the tree's `height` attribute
				- This example uses `return` and is an equivalent, but longer way to write it
					- ```javascript
					  // arrow1.js
					  const _getHeight = (tree) => {
					    return isEmpty(tree) ? 0 : tree.height;
					  };
					  ```
		- #### Example `arrow2.js`
			- If you use short version and want to return an object, enclose it in parentheses
		- #### Example `arrow3.js`
			- Provide default value for missing parameters -> this example will initialize an `s` with an empty string if no value is provided for the recursive part
	- ## Classes
		- Won't spend a lot of time here - reference Objects and Classes in MDN Web Doc Notes -> [[MDN Web Docs]]
			- This example is modified but will be examined in chapter 13
				- ```javascript
				  class Tree {		// 1
				    _children = [];	// 2
				  
				    constructor(rootKey) {	//3
				      this._key = rootKey;
				    }
				  
				    isEmpty() {
				      return this._key === undefined;
				    }
				  
				    get key() {		// 4
				      this._throwIfEmpty();
				      return this._key;
				    }
				  
				    set key(v) {		// 5
				      this._key = v;
				    }
				  }
				  ```
					- Start by defining a simple class, or extend an existing one -> you could have another `class BinaryTree extends Tree` to define a class based on `Tree`.
					  logseq.order-list-type:: number
					- You can define attributes outside a constructor
					  logseq.order-list-type:: number
					- You don't need to do it inside a constructor. Constructors are available if you need more complex object instance initialization.
					  logseq.order-list-type:: number
					- Getters
					  logseq.order-list-type:: number
					- Setters
					  logseq.order-list-type:: number
						- Getters and setters are other powerful features -> they bind an object's property to functions that are invoked whenever we try to modify or access that property.
	- ## The Spread Operator
		- (...) You can spread an array, string, or object into separate values in a single operation
			- Example of spreading array:
				- ```javascript
				  const myArray = [3, 1, 4, 1, 5, 9, 2, 6];
				  const arrayMax = Math.max(...myArray);	// 1
				  const newArray = [...myArray];	// 2
				  ```
			- Example of spreading object:
				- ```javascript
				  const myArray = [3, 1, 4, 1, 5, 9, 2, 6];
				  const arrayMax = Math.max(...myArray);
				  const newArray = [...myArray];
				  ```
		- We can also use the spread operator for fx that need to deal with an undefined number of parameters.
			- See `spreadMathMax.js`
	- ## The Destructuring Statement
		- Allows you to assign several variables at the same time.
			- ```javascript
			  [first, last] = ["Abraham", "Lincoln"];
			  ```
		- We can also mix destructuring and spreading
			- ```javascript
			  [first, last, ...years] = ["Abraham", "Lincoln", 1809, 1865];
			  ```
		- You can also use default values if there are no corresponding values on the right hand side
			- ```javascript
			  let [first, last, role = "President", party] = ["Abraham", "Lincoln"];
			  ```
		- You can also swap or rotate variables, which is used a lot later on
			- ```javascript
			  [heap[p], heap[i]] = [heap[i], [heap[p]]];
			  ```
		- Another pattern for destructuring is to return two or more variables at once from a function
			- ```javascript
			  const order2 = (a, b) => {
			  
			    if (a < b) {
			  
			      return [a, b];
			  
			    } else {
			  
			      return [b, a];
			  
			    }
			  
			  };
			  
			  
			  
			  let [smaller, bigger] = order2(22, 9); // smaller==9, bigger==22
			  ```
	- ## Modules
		- Split code into pieces you can import when needed so that functionality is packaged in a way that is easier to understand and maintain. Should be an aggregation of related functions and classes, providing a set of features.
		- High cohesion -> elements put together should truly belong together as unrelated functionalities should not be mixed in the same module.
		- Low coupling -> Distinct modules should be interdependent as little as possible
		- ### CommonJS Modules vs ECMAScript Modules
	- ## Closures and Immediately Invoked Function Expressions
		- Closure is the combo of fx plus its encompassing scope to which the function has access. It allows you to have private variables which in turn allows you to create equivalent of classes and modules
			- Look at `closure.js`
				- The variables first and last are not