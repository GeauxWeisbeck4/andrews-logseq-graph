tags:: Beej Guides, C, Tutorial

- # Table of Contents
	- ## 3.1 Variables
		- We can think of a variable as a human-readable name that refers to some data in memory
		- The index to memory where data is stored is an address, location, or a pointer
		- ### 3.1.1 Variable Names
			- Can't start with digit, two underscores, or underscore w/ capital
		- ### 3.1.2 Variable Types
			- Some example types, some of the most basic:
			- | Type |  Example | C Type | 
			  
			  | ---- |
			- | 
			  | Integer | 
			  | `3490` | 
			  | `int` | 
			  |
			- | 
			  | Floating point | 
			  | `3.14159` | 
			  | `float`[35](https://beej.us/guide/bgc/html/split/function-specifiers-alignment-specifiersoperators.html#fn35) | 
			  |
			- | 
			  | Character (single) | 
			  | `'c'` | 
			  | `char` | 
			  |
			- | 
			  | String | 
			  | `"Hello, world!"` | 
			  | `char *`[36](https://beej.us/guide/bgc/html/split/function-specifiers-alignment-specifiersoperators.html#fn36) | 
			  |
			- C makes an effort to convert automatically between most numeric types when you as