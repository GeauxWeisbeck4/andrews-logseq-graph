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
			- You can declare boolean types as ints