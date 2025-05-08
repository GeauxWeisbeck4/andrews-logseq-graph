tags:: Programming Books, JavaScript, Data Structures, Algorithms, Computer Science, No Starch Press

- title:: Sorting
- Chapter:: 6
- book:: [[Data Structures and Algorithms in JavaScript]]
- # 6 Sorting
	- How to sort a set of records into order, where each record consists of a key (alphabetical, numerical, or several fields) and data. The algorithm's output should include the exact same set of records, but shuffled so that the keys are in order. Usually keys are in ascending order, but descending order requires only a minor change in sorting algorithms
	- ## The Sorting Problem
		- A sorting algorithm is an algorithm that given a list of records containing a key and some data, reorders the list so that the keys are in non-decreasing order (no key is smaller than its preceding key) and the output list is a permutation of the input list, retaining all original records.
		- ### Sorting Stability
			- Maintains same order as the input in terms of where keys and elements end up in sorting algorithm -> if one element preceded another and both  had the same key, in the ordered input, the first one will precede the second
			- You can add an extended key to make sort more stable
		- ### JavaScript's Own Sort Method
			- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort
			- Given an array, the sort method reorders the array in place using an optional comparison function (toSorted - newer method - doesn't sort in place but instead produces a new sorted version of the array.)
				- If no function is provided, JavaScript converts elements to strings and then sorts lexicographically
				- ```javascript
				  const a = [22, 9, 60, 12, 4, 56];
				  a.sort();
				  console.log(a);
				  ```
			- To accomodate we need to provide a function that will return two elements, a and b, and return a negative value if a should precede b, positive value if a should follow b, and zero if both keys are equal and if a and b can be in any order - its an easy fix
				- ```javascript
				  const a = [22, 9, 60, 12, 4, 56];
				  a.sort((a, b) => a - b);
				  console.log(a);
				  ```
			- Check out `complexSortMethod.js`
				- The data to sort ❶ has dates as three separate fields (d, m, and y, for day, month, and year) and name (n). If two persons are from different years ❷,
				  you return the correct negative or positive value by subtracting years.
				  If the years are equal, you can compare months with the same kind of 
				  logic ❸, and if the months are also equal ❹, you do the same once more for days. If the dates are equal, you resort to comparing names ❺, and since you cannot use math and just subtract dates, you need to make actual comparisons, date part by date part. The final return 0 is done ❻ only if all fields were compared and found to match.
				- If you sort the people array with the dateNameCompare(...) function you just wrote, you get the expected result:
				- Finally, consider stability. Originally, the specification for the .sort(...)
				   method didn’t require it, but ECMAScript 2019 added the requirement. Be
				   aware, however, that if using an earlier JavaScript engine, you cannot 
				  assume stability, so you might have to resort to the solution described 
				  in “Sorting Stability” on [page 93](https://learning.oreilly.com/library/view/data-structures-and/9798341620001/xhtml/chapter6.xhtml#pg_93). Also, keep in mind that any given engine may just not correctly implement the standard.
		- ### Sort Performance
			- If you are comparing elements to sort an array, its indirectly implied that you're deciding on what was the original permutation. Well-place questions divide the range of options in half, so you need to know how many questions are needed for n! possibilities -> Equivalent ot asking how many times you should divide n by 2 until you get down to 1.
			- The answer is log n!, in base 2. Stirling's approximation says that n! grows as n^n, so logarithm of n! is O(n log n). It will always be at least  O(nlogn) with no better results, but possibly worse results.
	- ## Sorting with Comparisons
		-