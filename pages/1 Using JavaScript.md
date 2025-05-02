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
		- This will be used a lot in this book (first argument):
		- ```javascript
		  const _getHeight = (tree) => (isEmpty(tree) ? 0: tree.height);
		  ```