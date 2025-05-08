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
				  ```