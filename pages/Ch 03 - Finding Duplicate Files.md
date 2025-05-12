tags:: Software Design, Python

- title:: [[Software Design By Example - Python]]
- # Ch 03 - Finding Duplicate Files
	- A hash function creates a fixed-size value from an arbitrary sequence of bytes.
	- Use big-oh notation to estimate the running time of algorithms.
	- The output of a hash function is deterministic but not easy to predict.
	- A good hash function's output is evenly distributed.
	- A large cryptographic hash can be used to uniquely identify a file's contents.
- Suppose we want to find duplicated files,
  such as extra copies of photos or data sets.
  People often rename files,
  so we must compare their contents,
  but this will be slow if we have a lot of files.
- We can estimate how slow “slow” will be with a simple calculation.
- objects can be paired in  ways.
  If we remove duplicate pairings
  (i.e., if we count A-B and B-A as one pair)
  then there are  distinct pairs.
  As  gets large,
  this value is approximately proportional to .
  A computer scientist would say that
  the [time complexity](https://third-bit.com/sdxpy/glossary/#gl:time_complexity) of our algorithm is
- ,
  which is pronounced “[big-oh](https://third-bit.com/sdxpy/glossary/#gl:big_oh) of N squared”.
  In simpler terms,
  when the number of files doubles,
  the running time roughly quadruples,
  which means the time per file increases as the number of files increases.
- Slowdown like this is often unavoidable,
  but in our case there’s a better way.
  If we generate a shorter identifier for each file
  that depends only on the bytes it contains,
  we can group together the files that have the same identifier
  and only compare the files within a group.
  This approach is faster because we only do the expensive byte-by-byte comparison
  on files that *might* be equal.
  And as we’ll see,
  if we are very clever about how we generate identifiers
  then we can avoid byte-by-byte comparisons entirely.
	- ## Section 3.1: Getting Started
		-