tags:: Beej Guides, C, Tutorial

- # Table of Contents
	- ## 3.1 Variables
		- We can think of a variable as a human-readable name that refers to some data in memory
		- The index to memory where data is stored is an address, location, or a pointer
		- ### 3.1.1 Variable Names
			- Can't start with digit, two underscores, or underscore w/ capital
		- ### 3.1.2 Variable Types
			- | Type | Example | C Type|
			  |-----|
			  | Integer | 3490 | int |
			  | Floating point | 3.14159 | float |
			  | Character (single) | 'c' | char |
			  | String | "Hello, world!" | char * |
				- C makes an effort to convert auto. between most numeric types when you ask it to
				- Uninitialized variables have indeterminate value - if they are not, they must assume it can be some nonsense number
			- For our example code, we are going to use the `printf()` function to print out our variables -> it does this by searching for a variety of special sequences starting with (%) percentage sign:
				- %d -> Looks to the next parameter that was passed and prints integer
				- %f -> Prints a float
				- %s -> Prints a string
					- ```c
					  #include <stdio.h>
					  
					  int main(void)
					  {
					      int i = 2;
					      float f = 3.14;
					      char *s = "Hello, world!";
					  
					      printf("%s i = %d and f = %f!\n", s, i, f);
					  }
					  ```
		- ### 3.1.3 Boolean Types
			- In C, 0 = "false" & non-zero = "true"
			- You can declare boolean types as ints but...
				- Technically you should be setting a `bool` value to `true` or `false`, or the result of some expression that evaluates to either
			- Be careful if you mix and match since the numeric value of `true` is 1 probably, and if you are relying on some other positive value to be true, you will be in for a surprise
				- ```c
				  print("%d\n", true == 12);  // Prints "0", false!
				  ```
	- ## 3.2 Operators and Expressions
		- similar to other languages
		- ### 3.2.1 Arithmetic
			- ```c
			  i = i + 3;  // Addition (+) and assignment (=) operators, add 3 to i
			  i = i - 8;  // Subtraction, subtract 8 from i
			  i = i * 9;  // Multiplication
			  i = i / 2;  // Division
			  i = i % 5;  // Modulo (division remainder)
			  
			  i += 3;  // Same as "i = i + 3", add 3 to i
			  i -= 8;  // Same as "i = i - 8"
			  i *= 9;  // Same as "i = i * 9"
			  i /= 2;  // Same as "i = i / 2"
			  i %= 5;  // Same as "i = i % 5"
			  ```
				- There is no exponentiation -> use one of the pow() unction variants from `math.h`
			- ### 3.2.2 Ternary Operator
				- Expression whose value depends on the result of a conditional embedded in it
					- ```c
					  // If x > 10, add 17 to y. Otherwise add 37 to y.
					  y += x > 10? 17: 37;
					  
					  // Rewritten as if statements
					  if (x > 10)
					    y += 17;
					  else
					    y += 37;
					  
					  // Another example that prints if number stored in x is odd or even
					  printf("The number %d is %s.\n", x, x % 2 == 0? "even": "odd");
					  ```
		- ### 3.2.3 Pre-and-Post Increment-and-Decrement
			- The legendary post-increment and post-decrement operators:
				- **Post Increment and Decrement**
					- ```c
					  i++;  // Add one to i (post-increment)
					  i--;  // Subtract one from i (post-decrement)
					  
					  // Longerversion of the above
					  i += 1;
					  i -= 1;
					  ```
				- **Pre Increment and Decrement**
					- ```c
					  ++i;  // Add one to i (pre-increment)
					  --i; // Subtract one from i (pre-decrement)
					  
					  ```
					- Pre the value of the variable is done before the expression is evaluated, then it is evaluated
					- The value of expression is done with the value as is, then the value is done after the expression determined - you can embed them in expressions
						- ```c
						  i = 10;
						  j = 5 + i++;  // Compute 5 + i, them increment i
						  
						  printf(%d, %d\n", i, j);  // prints 11, 15
						  ```
					- Compare that to the pre-increment
						- ```c
						  i = 10;
						  j = 5 + ++i;  // Increment i, them computer 5 + i
						  
						  printf("%d, %d\n", i, j);  // Prints 11, 16
						  ```
							- Frequently used with array and pointer access and manipulation -> provides way to use value in a variable while incrementing or decrement the value before or after it is used.
						- Most common place however is in a for loop
							- ```c
							  for (i = 0; i < 10; i++)
							    printf("i is %d\n", i);
							  ```
		- ### 3.2.4 The Comma Operator
			- This is one expression:
				- ```c
				  x = 10, y = 20;  // First assign 10 to x, then 20 to y
				  ```
			- This one is two  separate expressions:
				- ```c
				  x = 10; y = 20;  // First assign 10 to x, then 20 to y
				  ```
			- In the comma operator, the value of the comma expression is value of rightmost expression
				- ```c
				  x = (1, 2, 3);
				  
				  printf("x is %d\n", x);  // Prints 3, b/c 3 is rightmost in the comma list
				  ```
			- It is often used in for loops to do multiple things w/ each section of the statement
				- ```c
				  for (i = 0, j = 10; i < 100; i++, j++)
				    printf("%d, %d\n", i, j);
				  ```
		- ### 3.2.5 Conditional Operators
			- For boolean values, we have raft of standard operators:
				- ```c
				  a == b;  // True if a is equivalent to b
				  a != b;  // True if a is not equivalent to b
				  a < b;   // True if a is less than b
				  a > b;   // True if a is greater than b
				  a <= b;  // True if a is less than or equal to b
				  a >= b;  // True if a is greater than or equal to b
				  ```
			- = is assignment
			- == is comparison
				- Example of comparison in if statements
					- ```c
					  if (a <= 10)
					    printf("Success!\n");
					  ```
		- ### 3.2.6 Boolean Operators
			- Chaining together or altering conditional expressions w/ Boolean operators for *and*, *or*, and *not*
				- | Operator | Boolean meaning |
				  |--------|
				  | && | and |
				  | `two straight lines` | or | 
				  | ! | not |
			- Ex boolean *and*:
				- ```c
				  // Do something i x less than 10 and y greater than 20
				  if (x < 10 && y > 20)
				    printf("Doing something!\n");
				  ```
			- Ex boolean *not*:
				- ```c
				  if (!(x < 12))
				    printf("x is not less than 12\n");
				  ```
				- `!` has higher order of precedence than other boolean ops so we have to use parentheses in that case -> but its the same as:
					- ```c
					  if (x >= 12)
					    printf("x is not less than 12\n");
					  ```
		- ### 3.2.7 The `sizeof` Operator
			- Tells you the size (in bytes) that a variable or data type uses in memory
				- More particularly, the size (in bytes) that the type of a particular expression (might even be a single variable) uses in memory
			- C has a special type to represent the return value from `sizeof` -> `size_t` -> unsigned integer type that can hold the size in bytes of anything you give it in `sizeof`
				- `size_t` shows up a lot of diff places where counts of things are passed or returned -> think of it as returning a count
					- ```c
					  int a = 999;
					  
					  // %zu format specifier for size_t
					  // Leave off "z" if compiler gets mad
					  
					  printf("%zu\n", sizeof a);  // prints 4
					  printf("%zu\n", sizeof(2 + 7));  // prints 4
					  printf("%zu\n", sizeof 3.14);  // prints 8
					         
					  // Negative size_t values = %zd
					  printf("%zu\n", sizeof(int));  // prints 4
					  printf("%zu\n", sizeof(char));  // prints 1
					  ```
						- Its the size in bytes of type of expression - that's why a and 2+7 are the same -> they're both type int
						- Note that `sizeof` is a compile-time operation
	- ## 3.3 Flow Control
		- Let's start with executing a single statement with `if`:
			- ```c
			  if (x == 10) printf("x is 10\n");
			  // Or
			  if (x == 10)
			    printf("x is 10\n");
			  ```
		- Now let's try multiple things to happen - block or compound statement
			- ```c
			  ```