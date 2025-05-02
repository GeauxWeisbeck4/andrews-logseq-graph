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
							-