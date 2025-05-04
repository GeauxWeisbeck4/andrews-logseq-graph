tags:: Programming Books, JavaScript, Data Structures, Algorithms, No Starch Press, Computer Science

- title:: Designing Algorithms
- Chapter:: 5
- book:: [[Data Structures and Algorithms in JavaScript]]
- # 5 Designing Algorithms
	- This chapter covers several techniques for designing 
	  algorithms. We’ll start with recursion, which solves a problem by 
	  breaking it up into one or more simpler cases of the same problem. We’ll
	  also look at *dynamic programming*, which solves a complex problem
	  by solving simpler cases first and storing those solutions to avoid 
	  needless recalculations, as well as the *brute-force* (or *exhaustive*) *search* strategy, where you find a solution to a problem by systematically trying all possible solutions. Finally, we’ll explore *greedy algorithms*
	  that apply a heuristic of choosing the best local option at each 
	  junction of a problem, with the hope that the given methodology will 
	  lead to the solution. Unlike the other strategies mentioned in this list, greedy algorithms may not always arrive at the best solution.
	- The strategies explored here are successfully applied to 
	  develop algorithms used along with data structures for the 
	  implementation of specific abstract data types (ADTs), so focusing on 
	  how to design a new solution for any given problem is worthwhile. The 
	  techniques covered in this chapter aren’t exhaustive, but they lie below
	  the surface in many of the algorithms that we’ll explore later.
	- ## Recursion
		- A function calls itself over and over again. Appears naturally in:
			- ### Mathematics
				- Factorial of a number or Fibonacci series are naturally recursive
			- ### Data Structures
				- List may be empty of consist of a special node, head of the list, followed by another list
				- Tree, consisting of parent node called a root that is connected ot children
			- ### Procedures
				- Several algorithms can be expressed recursively
			- Two kinds of cases:
				- Simple ones that can be solved directly w/out any recursion
				- Complex ones that need to use the function itself as an aid
			- Four step procedure
				- Assume you already have a function that solves your problem
				- Find some simple base cases that you can solve directly without any complications
				- Figure out how you can solve the original problem by first solving one or more smaller versions of it
				- Apply your assumed function from step 1 to solve minor prolems of step 3, or if small enough solve as in step 2
	- ## The Divide an Conquer Strategy
		- Divide problem into smaller versions of itself and conquer it using the solutions to all of them
		- ### Calculating Factorials
			- Factorial of a non-negative number n is defined as: n = 0, 0! = 1, and for n > 0, n! = n x (n - 1)!
			- How many ways can you order n books in a row on a shelf?
		- ### Searching and Traversing
			- binary search and traversing (if there is no tasks you're done, if not go to top of list and do all tasks till your done)
		- ### Considering the Fibonacci Series
			-