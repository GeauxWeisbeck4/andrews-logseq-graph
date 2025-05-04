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
			- Series starts w/ 0 and 1, after that it is the series of two previous ones
				- F0 = 0, F1 = 1, and for n > 1, Fn = Fn-1 + Fn-2
		- ### Sorting and Puzzles
			- Tower of Hanoi
	- ## The Backtracking Technique
		- Choose one method of many, and try finding a solution with it. If it succeeds you're done, if you fail backtrack to the point where you made the selection and choose a different option.  If you run out of options, there is no solution
		- ### Finding a Path in a Maze
			- Pseudocode for an algorithm
				- ```
				  ❶ solveMaze(fromCell, toCell, maze, path=[])
				  
				  ❷ if(fromCell === toCell) {
				  
				      return path // success!
				  
				    }
				  
				  ❸ mark fromCell as visited
				  
				  ❹ for all nextCell cells adjacent to fromCell {
				  
				    ❺ updatedPath = solveMaze(nextCell, toCell, maze, path + fromCell)
				  
				      if updatedPath is not null {
				  
				        return path
				  
				      }
				  
				    }
				  
				    // All adjacent cells were tried, and failed...
				  
				  ❻ return null   // failure
				  
				  }
				  ```
		- ### Solving the Squarest Game on the Beach Puzzle
	- ## Dynamic Programming
		- Solve other small problems and store those results so they don't have to be called again when they are needed later
		- Can be:
		- top down ->  Solves problem logically by checking whether its already been solved before dealing with this subproblem
		- bottom up ->  requires first looking at smaller subproblems then solving the original problem
		- Memoization -> Recursive implementations, best for top down
		- tabulation -> bassed on arrays or matrices, best for bottom up DP
		- ### Calculating Fibonacci Series w/ Top Down DP
			- To do this more effectively we need to use memoize to cache previous calcualtions and not repeat the same ones over and over. It first checks an internal cache to see if it was laready made
			- There are packages we can use like npm package [fast-memoize](https://www.npmjs.com/package/fast-memoize)
		- ### Line Breaking with Top-Down DP
			- Building a nice-looking web form w