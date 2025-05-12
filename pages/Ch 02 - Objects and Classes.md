tags:: Software Design, OOP, Class, Python

- title:: Software Design By Example - Python
- # Chapter 2: Objects and Classes
	- Objects are useful without classes, but classes make them easier to understand.
	- A well-designed class defines a contract that code using its instances can rely on.
	- Objects that respect the same contract are 
	  polymorphic, i.e., they can be used interchangeably even if they do 
	  different specific things.
	- Objects and classes can be thought of as dictionaries with stereotyped behavior.
	- Most languages allow functions and methods to take a variable number of arguments.
	- Inheritance can be implemented in several ways that differ in the order in which objects and classes are searched for methods.
	- ## Object Oriented Programming
		- Answers these two questions:
			- What is a natural way to represent real-world “things” in code?
			- How can we organize code to make it easier to understand, test, and extend?
	- ## Section 2.1: Objects
		- `shapes_original.py`
			- Spec like this is called a contract because an object must satisfy it to be considered a shape -> It must provide methods with these names tat do what those names suggest
				- We can derive classes from Shapes to represent squares and circles
			- ### Polymorphism
				- If two objects have same methods, we can use them interchangeably
				- Allow people to ignore differences when using related things easier
				- Bytes in a string represent characters and bytes in an image represent pixels, bytes in a function represent instructions.e
				- `func_obj.py`
					- When Python executes this code, it creates an object in memory to print a string and assign the variable to it.
				- ### Alias
					- Assigning a second variable to a function and referencing the second function doesn't alter or erase the connection between function and original name
			- We can also store functions in lists and dictionaries `shapes_dict.py`
				- ```
				  square ->  { 
				  			 name -> "sq"
				  			 side -> 2
				               perim -> square_perim()
				               area -> square_area()
				              }
				              
				  circle ->   {
				  			  name -> "ci"
				  			  radius -> 3
				                perim -> circle_perim()
				                circle_area -> circle_area()
				  }
				  ```
		- The function `call` looks up the function stored in the dictionary,
		  then calls that function with the dictionary as its first object;
		  in other words,instead of using `obj.meth(arg)` we use `obj["meth"](obj, arg)`.Behind the scenes, this is (almost) how objects actually work.
		  We can think of an object as a special kind of dictionary.
		  A method is just a function that takes an object of the right kind
		  as its first parameter (typically called `self` in Python).
	- ## Section 2.2: Classes
		- We can use classes to ensure every object has similar behavior
			- ![image.png](../assets/image_1747052853015_0.png)
		- Calling a method involves one more lookup because we go from the object to the class to the method, calling the "method" with the object as the first argument
	-
	-