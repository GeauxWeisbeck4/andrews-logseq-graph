tags:: JavaScript, Data Structures, Algorithms, Computer Science, No Starch Press, Programming Books

- title:: Abstract Data Types
- chapter:: 3
- book:: [[Data Structures and Algorithms in JavaScript]]
- # 3 Abstract Data Types
	- Abstract data types (ADT) are defined by the operations it supports and the behavior it provides. It specifies needs and requirements in general
	- This book looks at data structures in the context of an ADT
	- ## Data Types
		- Data types are those defined by sets of possible values it may represent and the operations that can be performed on it -> example: possible to concatenate two strings, perform logical operations with booleans, do arithmetic with integer numbers, or compare floating point numbers
		- Basic idea of an ADT is specifying the operations that can be done, not internal aspects
	- ## Abstraction
		- Abstraction implies hiding or omitting details and reaching instead for an overarching higher-level idea -> Ignore implementation aspects so we can concentrate on our needs
		- ### Encapsulation
			- Designing modules as if they had a "shell" or a "capsule" around them so only the module is responsible for handling its data. Makes design more coherent and cohesive
		- ### Data Hiding
			- Hide inner details of a module's implementation from rest of systems so they can ensured to be changed without affecting any other parts of the code. No one can access inner details from the outside
		- ### Modularity
			- Dividing a system into spearate modules that can be designed and developed independently from the rest of the system.
	- ## Operations and Mutations
		- Mutable vs immutable values are a common way to classify data types
		- In JS, objects and arrays are mutable, you can edit them w/out creating new ones. Numbers and strings are immutable -> a new, different and distinct value is created when you apply an operation to them
		- > React Redux is an example of immutability -> if you want to modify state of a React application, you must generate a new state with whatever changes you want. Assume you manage state data in immutable way
		- Operations categories that apply to an ADT
			- ### Creators
				- Fx that produce a new object of a given type, possibly taking some values as arguments. Using the date ADT example, a creator could build a new date out of day, month, and year values
			- ### Observers
				- Fx that take objects of a given type and produce some values of a different type. For date ADT, `getMonth()` operation might produce the month as an integer, or an `isSunday()` predicate could determine whether the given date falls on a Sunday
			- ### Producers
				- Fx that take an object of a given type and possibly some extra arguments and produce a new object of the given type. Date ADT could have a fx that added an integer number of days to a date producing a new date
			- ### Mutators
				- Fx that directly modify an object of a given type. A `setMonth()` method could modify an object instead of producing a new one
	- ## Implementing an ADT
		- Creating a bag or multiset
			- Container like a set, but it allows for repeated elements (sets cannot have repeated elements by def.)
			- Also add an extra operation ("greatest") to make it more interesting
			- | *Operation* | *Sign|ature* | *Description* |
			  |--------------|
			  | Create | -> bag | Create a new bag. |
			  | Empty? | bag -> boolean | Given a bag, determine whether its empty. | 
			  | Add | bag x value -> bag | Given a value, add it to the bag. |
			  | Remove | bag x value -> bag | Given a value, remove it from the bag. |
			  | Find | bag x value -> boolean | Given a vlue, check whether it exists in the bag. |
			  | Greatest | bag -> value \ undefined | Given a bag, find the greatest value in it |
		- Specifying an operation's signature is specifying a function's parameters and the returned results -> based on a type system called Hindley-Milner
	- ## Implementing ADTs Using Classes
		- We'll use a class to implement a bag ADT. For example, if we add HOME, SWEET, HOME words to a bag, it looks like:
			- ```
			  {
			  	count: 3,
			      data: {
			      	HOME: 2,
			          SWEET: 1,
			      },
			  };
			  ```
	- ## Implementing ADTs Using Functions (Mutable Version)
		- What if we were to use functions instead of classes
	- ## Implementing ADTs Using Functions (Immutable Version)
		- If we want to use immutability fo rmore functional way and avoiding side effects
			- We may not modify the bag directly, but may need to change the implementation of the mutator methods. Must create and return a new object if bag needs changes
			- ```javascript
			  ```