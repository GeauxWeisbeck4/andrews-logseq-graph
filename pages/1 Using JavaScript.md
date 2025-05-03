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
				  ```