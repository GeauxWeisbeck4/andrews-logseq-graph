tags:: JavaScript, Data Structures, Algorithms, Computer Science, No Starch Press, Programming Books

- title:: Functional Programming in JavaScript
- chapter:: 2
- book:: [[Data Structures and Algorithms in JavaScript]]
- # Functional Programming in JavaScript
	- ## Why use FP?
		- ### Understandable
			- Produces shorter and cleaner code
		- ### Maintainable
			- Makes it easier to understand and maintain
		- ### Testable
			- Unit testing allows you to verify the behavior of each component of your code and provides a sort of documentation, providing examples of how to use any given function
		- ### Modular
			- Focuses on separation of concerns by writing code's features independent of each other and are highly cohesive and loosly copuled
		- ### Reusable
			- Reusing code saves time and money
- # JavaScript as a Functional Language
	- ## Functions as First Class Objects
		- You can do anything with a fx that you can with other objects. Store em in variables, pass fx as arguments, or return a fx as a result of some other fx
			- For example, in Axios:
				- ```javascript
				  axios.get("your/url/api").then((response) => {
				    // ... do something with the response
				  })
				  ```
					- Parameter to `.then()` method is a function, and it's passed in the same way you pass a number or an array.
			- `firstClassObject.js` Example
				- `preOrder()` fx takes two arguments, a tree and a visit unction. If the visit function isn't provided, the default value is just a simple function that just logs whatever you pass it.
			- Taking one or more functions makes it a *higher-order function* -> it should also return a fx as a result
			- Common functions that don't do either of those are *first-order functions*
			- Back to `firstClassObject.js`
				- The function is stored in a variable `myVisit` as an object and then the contents are passed into another function.
	- ## Declarative-Style Programming
		- We don't list out steps like we would in procedural programming. There are methods we use to to accomplish our task directly without having to create the solution by hand, like with looping - we can use `for` or `while` loop, but a loop method is more declarative.
			- A good example for JavaScript is arrays. We can use the array methods  to alter arrays without having to create the whole solution by hand. Here are a few:
				- `filter()` -> Pick elements that satisfy some condition out of an array
				- `find()` and `.findIndex()` -> Search an array to find an element that satisfies some condition
				- `.some()` -> Let you know whether at least one element of an array satisfies a condition
				- `.every()` -> Lets you test whether all elements of an array satisfy a condition
			- Other functions transform an array into a new array or a single result
				- `.map()` -> Lets you transform one array into another by applying a given function to its elements
				- `.reduce()` -> Applies a given operation to the whole array from left to right into a single result
				- `.reduceRight()` -> Works like `.reduce()` but from right to left
	- ## Filtering an Array
		- Go through an array, select some elements that satisfy some condition and dropping the rest. Provide a predicate (fx that produces a boolean result in terms of arguments) and new array will be produced with elements of original for which predicate returned true
			- ```javascript
			  const under21 = (value) => value < 21;
			  
			  let myArray = [22, 9, 60, 12, 4, 56];
			  let newArray = myArray.filter(under21);
			  console.log(newArray);
			  ```
				- This is truly declarative - specify what we want to get, not how to get it
	- ## Searching an Array
		- Search an array for some element that satisfies a predicate
			- `find()` -> Goes through array from beginning to end, testing for the given predicate, if an element of the array satisfies it, the element is returned; if no elements satisfy the predicate, undefined is returned.
				- `findLast()` does same, but searches from beginning to end
			- `findIndex()` -> Similar to find except it returns position of the first element satisfying the predicate or -1 if no elements satisfy it
				- `findLastIndex()` -> same like findLast() above
			- > If you need something more specific `includes()`, `index()` and `lastIndex()` are good, but they aren't functional because you can't test them.
	- ## Testing an Array
		- `some()` and `every()` check whether any element of an array satisies some predicate and the second checks whether all elements satisy it
	- ## Transforming an Array
		- Algorithm that goes through an array and applies some operation to create a new set. A list of strings that represent numbers but we need that list to become a list of the corresponding numeric values, we can set up a loop to go through all elements of the array systematically, processing each one by one and producing a new array -> `map()`
	- ## Reducing Array to a Single Value
		- Write loops that go through a complete array perform some kind of operation and end up with a single computed value (like add a list of numbers up) -> `reduce()` and `reduceRight()`
	- ## Looping Through Arrays
		- `forEach()` array function takes care of looping through an array and invoking a callback for each element so declare what kind of work to do and nothing else -> we'll redo the array summing logic
	- ## Higher-Order Functions
		- All fx that work with callbacks are hofs and so are lal the array methods
		- Some allow you to work in a more declarative style and some allow you to extend a fx -> example: adding logging as an aid to debugging or memoizing for better performance
		- Returnning a new function -> example: wrapped behavior. Wrapping produces a new fx that keeps its original funcitonality but adds some extra behavior. If we want to add logging, we could modify it, even though it is risky
		- Look at `hofLogging.js` -> we could create the functionality ourselves and implement it in the function, but if we have many return statements it becomes cumbersome
			- Using a wrapper hof logging function, we can create it and then add it to our function we want to log
	- ## Side Effects
		- Pure function -> a fx depends only on parameters it receives and doesn't produce any side effects. Closely related to mathematical functions -> given an f(x) function, all it does when given a value for x is calculate a new value.
			- Advantageous b/c they don't produce any side effects like changing program state, modify variables, mutate objects, and so on. Depends only on input and no external things.
	- ## Using Global State
		- Most common reason for side effects is using nonlocal variables that are shared with other parts of code. Global variables make it hard to debug your functions
	- ## Keeping Inner State
		- Don't want internal variables to keep state between calls b/c they could potentially be changed and return different outputs even if they have the same input arguments -> why fp doesn't like OOP
	- ## Mutating Arguments
		- Don't modify actual arguments to the function