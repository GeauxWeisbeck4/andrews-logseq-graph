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
				- `.every()` ->