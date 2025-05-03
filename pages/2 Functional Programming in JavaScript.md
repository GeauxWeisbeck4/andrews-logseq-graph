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
				-