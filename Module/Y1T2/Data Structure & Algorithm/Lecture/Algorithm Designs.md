---
tags:
  - Data-Structure-and-Algorithm
  - Lecture
---
# Divide and Conquer Principles
- We can solve the problem recursively, applying the following 3 steps at each level of recursion
	1. Divide
		- the problem into a number of smaller sub-problems
	2. Conquer
		- the sub-problems by solving them recursively
	3. Combine
		- The solutions to the sub-problems to form the solutions
## Base Case
- Once the sub-problem becomes small enough to solve easily. we stop the recurring divide
- It means we have reached the base case
- it is important that the divide process reaches the base case so that the algorithm does not recur infinitely
- Examples
	- Binary Search
	- Merge Sort
	- Quick Sort
### Binary Search
![[Pasted image 20260202111312.png]]
> - take middle element
> - compare value with middle value
> - take left if <, right if >
> - take new middle element until middle element is number
### Merge Sort
![[Pasted image 20260202111344.png]]
> - divide list into two list until one element left is in the list
> - compare the two item, if right list is smaller than left list, place item on the left side of merge list
> - for item in list compare previously merged list until it is sorted
```python
def merge(left, right):
    result = []
    i = j = 0
    while i < len(left) and j < len(right):
        if left[i] < right[j]:
            result.append(left[i])
            i+=1
        else:
            result.append(right[j])
            j+=1
    print(f"left: {left}, right: {right}")
    result.extend(left[i:])
    result.extend(right[j:])
    print(f"result: {result}")
    return result

def mergesort(a):
    if len(a) <= 1:
        return a
    mid = len(a)//2
    left = mergesort(a[:mid]) # splits the array until there is only one element left in each node
    right = mergesort(a[mid:])# recursively split the array until there is only one element is left in each node
    return merge(left, right)

if __name__ == "__main__":
    print("sorted: ",mergesort([3, 7, 6, -10, 15, 23.5, 55, -13]))
```
#### Result
```c
left: [3], right: [7]
result: [3, 7]
left: [6], right: [-10]
result: [-10, 6]
left: [3, 7], right: [-10, 6]
result: [-10, 3, 6, 7]
left: [15], right: [23.5]
result: [15, 23.5]
left: [55], right: [-13]
result: [-13, 55]
left: [15, 23.5], right: [-13, 55]
result: [-13, 15, 23.5, 55]
left: [-10, 3, 6, 7], right: [-13, 15, 23.5, 55]
result: [-13, -10, 3, 6, 7, 15, 23.5, 55]
sorted: [-13, -10, 3, 6, 7, 15, 23.5, 55]
```
### Quick Sort
![[Pasted image 20260202111403.png]]
> - pick one data item in the list as pivot
> - if x < pivot, then left list
> - if x > pivot then right list
> - pick another pivot on each node
> - compare until each node is left with one element
> - merge everything into one list
# Optimisation & Greedy Algorithms
- an optimisation problem means to find the best solution, not just a solution
- A "greedy algorithm" sometimes works well for optimisation problem
- A greedy algorithm works in phases. At each phase
	- Take the best you can get right now without regard for future consequences
	- Hope that choosing a local optimum at each step will end up at global optimum
## Greedy = Optimal?
- Greedy algorithms do not always yield optimal solutions although they do for many problems
- Examples of Greedy Algorithms
	- Dijksta's shortest path algorithm
	- Kruskal's Minimum Spanning Tree Algorithm
	- Prim's Minimum Spanning Tree Algorithm
## Greedy Algorithm to Count Money
- Suppose we want to gather an amount of money, using the fewest possible bills and coins
- A greedy algorithm to do it:
	- at each step, take the largest possible bill or coin that does not overshoot
		- to form 6.39, we choose (for US$)
			- a $5 bill
			- a $1 bill, $6
			- a 25c coin, 6.25
			- a 10c coin = 6.35
			- four 1c coin = 6.39; total 8 pcs (bills & coins)
		- For US$, the greedy algorithm always give the optimal solution
- Failed cases, some foreign currency uses $1, $7, $10 coins
	- a greedy algorithm to form $15:
		- $10 + 5 $1  = 6 coins
	- a better solution
		- 2 $7 + $1 = 3 coins
	- the greedy algorithm gives a solution, but not an optimal solution
## Greedy Algorithm for Scheduling Problem
- to execute nine jobs with the following running times 3,5,6,10,11,14,15,18,20 mins
- resources: 3 processors to run the jobs
- Approach 1: Do longest jobs first, on whatever processor is available
	![[Pasted image 20260202114652.png]]
	Time to completion: $18 + 11 + 6 = 35$ mins. is there a better solution?
- Approach 2: do shortest jobs first
	![[Pasted image 20260202114754.png]]
	Not good; time needed is $6+14+20=40$ minutes.
	However that the greedy algorithm itself is fast; at each stage just pick min or max
### Optimal Solution
- Better solutions do exists:
	![[Pasted image 20260202114918.png]]
- This solution is clearly optimal
- How do we find such a solution then?
	- One way: try all possible assignments of jobs to processors
	- However, this might take exponential time

# Backtracking Algorithms
- a methodical way of trying out various sequences of decisions, until we find one that works
- Based on depth-first recursive search
- Approach
	1. Tests whether a solution has been found
	2. If found, return the solution
	3. Else, for each choice that can be made:
		 1. Make a choice
		 2. Recur
		 3. If recursion gives a solution, return it
	 4. if no choices remain, return failure
 - Sometimes called a "Search tree"
 - Systematic search technique to completely work through solution pace
 - Prime example: labyrinth
	 - How does the mouse find the cheese
## Find Path Through Maze
- Start at beginning of maze
- if at exit, return True
- else, for each step from current location
	- recursively find path
	- return with first successful step
	- return false if all steps fail
## Finding the cheese
- Solution
	- Systematic exploration of the labyrinth
	- Backtrack if meet dead end (hence backtracking)
- Trial and Error
- Possible paths (use a tree to represent maze)
	![[Pasted image 20260202115850.png]]
	```
	BackTrack(K):
		if K is solution:
			output L;
		else:
			for each direct extension K' of K:
				BackTrack(K')
	```
	> Initial call using "BackTrack($K_0$)"
	
- Termination of backtracking
	- only if solution space is finally exhausted
	- only if it is ensured that no configurations remain to be tested
- Complexity of backtracking
	- directly dependent on the size of the solution space
	- usually exponential, this O($2^n$) or worse
	- Can use for small problems only
- Alternative:
	- limit the depth of recursion
	- Then select the best solution so far: chess programs
## The n-Queens Problem
- find all possible ways of placing n queens on an $n \times n$ chessboard so that no two queens occupy the same row, column or diagonal
	![[Pasted image 20260202120306.png]]
> - Consider one row at at time
> - within the row, consider one column at a time
> - Look for a "safe" column to place a queen
> if we find a safe column, place the queen there, and make a recursive call to place a queen on the next row
> - if we run out of columns, backtrack to row 1 by returning from the recursive call
> 	- pick up where we left off
> 	- we had tried columns 0-2, so now we try column 3

# Dynamic Programming
## Rod Cutting Problem
- given a rod of length n meters and a table of prices $p_i$ for length i = 1, 2, ..., n
  Determine the maximum revenue $r_n$ for cutting up the rod and selling the pieces
- Divide & conquer vs Dynamic Programming
- Note that if the price $p_n$ for a rod of length n is large enough, an optimal solution may require no cutting at all

| length i    | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   |
| ----------- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| price $p_i$ | 1   | 5   | 8   | 9   | 10  | 17  | 17  | 20  | 24  |
- for a rod of length n, there are $2^{n-1}$ ways to cut 
- Example, when n =4, there are 8 possible ways to cut the rod
### Divide & Conquer
![[Pasted image 20260202121152.png]]
`return max(1+8, 5+5, 8+1, 9+0) = 10`
#### Observation
- the sub-problem (with n=2,1,0) are solved repeatedly
- Better to solve each sub problem only once and save each solution
- if we encounter same sub-problem again, just look it up (don't recompute)
### Dynamic Programming
- Stores the solution to each sub-problem in case there are needed again
- Uses additional memory to cut computation time
- Time-memory trade-off
- Dynamic Programming can transform many exponential time algorithms into polynomial-time
![[Pasted image 20260202121412.png]]