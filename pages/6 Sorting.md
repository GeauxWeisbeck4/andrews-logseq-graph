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
		- Each method we will try will get better in terms of optimal performance as we go on
		- ### Bubbling Up and Down
			- Easy to implement and only for smaller sets of data
		- ## Bubble Sort
			- ![image.png](../assets/image_1746693433530_0.png)
			- All sorting functions share the same signature: an array to sort (arr) and the limits for sorting (from, to) that, by default, will be the array’s extremes ❶. The outer loop ❷ goes from the right to the left; after each pass, the element in position j of the array will be in the right place. The inner loop ❸ goes from the left extreme to the right up to (but not reaching) the outer loop j; you compare each element with the next ❹, and if the second is smaller, you swap them.
			- You can improve performance in most sorted arrays (a not 
			  uncommon case) by checking whether any swaps occurred on each pass through the array. If none were detected, it means the array is in order.
			- The performance of this algorithm is *O*(*n*2), which is easy to calculate. First count comparisons: the first pass does (*n* – 1) comparisons, the second pass does (*n* – 2), the third (*n* – 3), and so on. The total number of comparisons is then the sum of all numbers from (*n* – 1) down to 1, which is *n*(*n* – 1) / 2, so *O*(*n*2).
	- ## Sinking Sort and Shuttle Sort
		- Sinking sort is similar to bubble sort, but instead of greatest values moving to the end of the array, the lowest values quickly sink to the beginning of the array, but it takes longer for the greatest values to go to their places
			- We can alternate a pass of bubbling and a pass of sinking to get an enhanced algorithm called a shuttle sort (cocktail shaker or bidirectional bubble sorts)
			- Starting with the same elements, the first pass is the same as bubble sort's, moving 60, which is the greatest value in the array to the rightmost position. The second pass goes right to left and moves 04 the smallest value in the array to the leftmost position. The third pass again goes left to right and moves 56 to its place. After that it goes right to left, left to right, and so on.
			- `shuttleSort.js`
				- As mentioned earlier, the signature for this sort function is always the same: an array to sort and the portion to put in order ❶. You have two variables ❷ that mark how far to the left and right the array is already sorted: f (as in *from*) starts at the left and grows by 1 after each right-to-left pass, and t (as in *to*) starts at the right and decreases by 1 after each left-to-right pass. When these variables meet ❸, the sort is done. You perform a left-to-right pass as shown earlier ❹, and then you decrement t ❺, since you’ve placed a new value in the right place. After this pass, you do the same ❻, but right to left, and you increment f ❼ to finish.
				- The algorithm is still *O*(*n*2), but the actual 
				  implementation typically is double the speed or even better if you 
				  include testing for swaps (see question 6.7). In any case, it’s easy to 
				  show that it can’t do any worse, for in each pass, it places one number at its final position, so after having placed (*n* – 1) numbers at their place, it will be done, the same as bubble sort.
		- ### Sorting Strategies for Sorting Cards
		- ## Selection Sort
			- Look for the lowest card and place it farthest to the let in your hand. Then look for the next lowest card and place it after the first and keep doing that, always selecting the lowest remaining card and placing it next to the already sorted cards.
			- ![image.png](../assets/image_1746696234232_0.png)
			- In the first pass at the top, you find that the minimum number is 04, 
			  and you do a swap to move it to the first place in the array. The second
			   pass finds 09 and swaps it with 12, so you now have two numbers in 
			  order. The process continues the same way; an exception is in the 
			  next-to-last line, in which no swap is needed because 56 was already in 
			  the correct place.
			- `selectionSort.js`
				- Go in order ❶ from the first place in the array to the last. The m variable ❷ keeps track of the position of the minimum value already found. As you loop through the yet unsorted numbers ❸, if you find a new minimum candidate ❹, you update m. After finishing this loop, if the minimum isn’t already in place ❺, do a swap.
				- The order of this algorithm is, again, *O*(*n*2). You have to look at *n* elements to find what should go in the first place; then look at *n* – 1 for the second place, *n* – 2 for the third, and so on. You already know this sum is *O*(*n*2). The algorithm in the next section is also based on how you’d sort playing cards, but it has slightly better performance.
	- ## Insertion Sort
		- This time lets take the first card, which is clearly in order by itself. Next look at the second card and either place it before the first if it's lower or leave it where it is if it's higher. Now you have two cards in order. Look at the third card and decide where it should go among the previous two. Place it there. Insert in between cards that you have already sorted.
			- ![image.png](../assets/image_1746696628577_0.png)
			- Start with a single card in order, in this case, number 34. Then 
			  consider the next value, 12, and place it to the left of 34, so the two 
			  numbers are in order. Then consider 22, which goes between 12 and 34, 
			  and now three values are ordered. Continue working this way, always 
			  inserting the next number where it belongs among the previously sorted 
			  ones, until you reach the last line. After placing 14 among the already 
			  sorted numbers, the whole array becomes ordered.
			- Set up a loop that starts at the second place in the array and goes to the end ❶, and loop back as long as the list isn’t in order ❷, swapping to get new numbers in place ❸.
			- Looking at this carefully, you’ll notice it’s doing too many swaps to get the new element to its place.
			- You can quickly optimize the code to avoid that and do just one swap per loop:
			- The first loop ❶ is exactly the same as earlier, but the difference lies within. You set
			  the number to be inserted among the previously sorted aside ❷, and you loop to find where it should go ❸, pushing values that are greater to the right. At the end ❹, you place the new value in its final position.
			- Insertion sort is a simple algorithm, which makes it a 
			  good choice for smaller arrays. Later in the chapter we’ll look at how 
			  it’s sometimes used in hybrid sorting algorithms as a replacement for 
			  theoretically more convenient, but practically slower, alternative 
			  methods.
	- ## Making Bigger Jumps with Comb and Shell Sort
		- The idea of swapping elements and making the bubble up or sink down isn't bad, and applying the idea of making larger jumps (swapping elements that are farther apart) eventually leads to a better algorithm, shell sort. The variant that is combined with bubble sort variant is comb sort
	- ## Comb Sort
		- Lets look at bubble sort and consider how keys move in an array like rabbits and turtles. Rabbits represent the large values near the beginning o fthe list which quickly move to their new places at the end of the array, swap after swap. On the other hand, turtles represent the small values near the end of the list, which slowly move to their places in a single swap per pass