---
notion-id: 2f42294e-f74f-805a-8706-e54a6dded9dd
base: "[[Module.base]]"
Trimester: Y1T2
Module: Data Structure and Algorithm
Type: Lecture
tags:
  - Data-Structure-and-Algorithm
  - Lecture
---
# Recurrence
- based on a mathematical concept called recurrence
- Examples
    - Fibonacci Sequence
        - 0,1,1,2,3,5,8,21,34
## Recursive calls

- used frequently in computer programs
- A recursive function calls itself
```python
n! = 1 x 2 x 3 ... (n-1) x n for n >= 1
0! = 1 by definition
Hence, n! = n * (n-1)! for n >= 1
factorial(n) = n * factorial(n-1)
```

# Algorithm

## Strategy

- identify the recurrence relation to solve the problem
- translate the recurrence relation to a recursive algorithm
- Take not to translate the initial condition in the recurrence relation into the BEST CASE for the recursive algorithm

## Binary Search

- Goal
    - given a sorted array and a key
    - find index (location) of the key in the array
- Binary Search
    - compare key against middle entry
    1. smaller, search in the left half
    2. bigger search in the right half
    3. Equal, return the indnex
    4. Size ≤ 0, return -1 (not found)

## Exponentiation

- Compute a, n for $a^n$ for n
- a quick and easy algorithm
```python
def power(a,n):
	answer = 1
	for i in range(n):
		answer = answer * a
	return answer
```
- $2^8 = 2*2*2*2*2*2*2*2$
- faster way to compute $a^n$

## Fast Exponentiation
- Computer $a^n$ for an integer n
- divide and conquer strategy
	$2^8 = 2^4 * 2^4 = 16 * 16 = 256$
	$2^4 = 2^2 * 2^2 = 4*4 = 16$
	$2^2 = 2*2 =4$
```python
def power(a,n):
	if n==0: return 1
	answer = power(a, (int)(n/2))
	if n%2 == 0:
		return answer * answer
	else:
		return answer * answer * a
```
# Analysis
- decide on the parameter n indicating input size
- Identify algorithm’s basic operation
- Set up a recurrence relation with an appropriate initial condition expressing the number of times the basic operation is executed
- Solve the recurrence (or, at least, establish the solution’s order of growth) by backward substitutions or other methods