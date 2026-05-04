---
tags:
  - Data-Structure-and-Algorithm
  - Lecture
---
# Bubble Sort
## Method
1. Step through the list to be sorted
2. compare two adjacent items at a time until the end of the list
	- Swap the adjacent items if they are in the wrong order
3. Repeat from the beginning of the list until no swaps are needed
## Algorithm
```Python
def bubblesort(a:list[int])->list[int]:
    n = len(a)
    for i in range(n):  # iterate through each item
        # get current last position from iteration (i=1, means we are placing the 2nd largest in its position)
        for j in range(n - i - 1):
            if a[j] > a[j + 1]:  # if item is larger than next value
                a[j], a[j + 1] = a[j + 1], a[j]  # swap
        print(a)
    return a

def main():
    alist = [54, 26, 93, 17, 77, 31, 44, 55, 20]
    bubblesort(alist)
    print(alist)

if __name__ == "__main__":
    main()
```
## Analysis
- Regardless of how the items are arranged in the initial list, (n-1) passes will be made to sort a list of size n
- Total number of comparisons
	$1+2+3+...+(n-1) = \frac{1}{2}n^2-\frac{1}{2}n$
- Complexity is $O(n^2)$
- In the best case, if the list is already ordered, no exchanges will be made
- In the worse case, every comparison will cause an exchange
- On average, bubble sort exchanges half of the time

# Selection Sort
1. The array is divide into two parts: sorted and unsorted. Initially sorted part is empty
2. Find the maximum value in the list
3. Swap it with the value in the last position of the unsorted part. This will form the sorted part
4. Repeat steps 2 & 3 for the remainder of the list

## Algorithm
```Python
def selectionSort(a:list[str]):
    n = len(a)
    for positionToFill in range(n - 1, 0, -1): # starts from the back of the list
        currentMax = 0 # set default max to first item in list
        for current in range(1, positionToFill + 1): # starts from index 1 to end of unsorted portion
            if a[current] > a[currentMax]: # if current value is bigger than max
                currentMax = current # change max to current
        a[positionToFill], a[currentMax] = a[currentMax], a[positionToFill]
        print(a)

def main():
    alist = [54, 26, 93, 17, 77, 31, 44, 55, 20]
    selectionSort(alist)
    print(alist)

if __name__ == "__main__":
    main()
```
## Analysis
- Selection sort makes the same number of comparisons as bubble sort
- Complexity is there also $O(n^2)$
- However, due to the reduction in the number of exchanges, selection sort typically executes faster than bubble sort in benchmark studies
# Insertion Sort
## Method
1. Every iteration of insertion sort removes an element (normally the first one) from the input data, and inserts it into the correct position in the already-sorted list
2. Repeat step 1 until no input element remains
## Algorithm
```Python
def insertionSort(a):
    n = len(a)
    for i in range(1, n):  # starts from index 1 to end of list
        j = i  # set aside index of element
        while j > 0 and a[j - 1] > a[j]:
            # while index j has not reach the beginning
            # and previous index is bigger than current element
            a[j], a[j - 1] = a[j - 1], a[j]
            j -= 1  # element position is reduced by 1
        print(a)
    return a

def main():
    alist = [54, 26, 93, 17, 77, 31, 44, 55, 20]
    insertionSort(alist)
    print(alist)

if __name__ == "__main__":
    main()
```
## Analysis
- Insertion Sort uses (n-1) passes to sort n items.
- Max number of comparisons is $O(n^2)$ complexity
	> $1+2+3+...+(n-1)=\frac{1}{2}n^2-\frac{1}{2}n$
- Best case: Only one comparison needs to be done on each pass for an already sorted list
# Merge Sort
## Method
1. Divide the unsorted list into two nearly equal size sub-lists
2. Sort each sub-list recursively by applying merge sort
3. Merge the sub-lists back into one sorted list
## Algorithm
```Python
arrayC = []

def merge(a1, a2):
    global arrayC
    arrayC.clear()
    n1, n2 = len(a1), len(a2)
    i1 = i2 = 0
    while i1 < n1 and i2 < n2:
        if a1[i1] < a2[i2]:
            arrayC.append(a1[i1])
            i1 += 1
        else:
            arrayC.append(a2[i2])
            i2 += 1
    arrayC += a1[i1::] + a2[i2::]
    return arrayC.copy()

def mergesort(a):
    n = len(a)
    if n == 1:
        return a
    mid = n // 2
    firstHalf = mergesort(a[0:mid])
    secondHalf = mergesort(a[mid::])
    return merge(firstHalf, secondHalf)

def main():
    alist = [54, 26, 93, 17, 77, 31, 44, 55, 20]
    alist = mergesort(alist)
    print(alist)


if __name__ == "__main__":
    main()
```
## Analysis
- Time Complexity
	- Merge is $O(n)$
	- Merge is called $O(log(n))$ times recursively
	- MergeSort is O(n log n)
- Space Complexity
	- Merge uses an additional arrayC
	- if arrayC was local inside merge, much more storage would be used because of recursive calls
	- Consider using a global arrayC in the implementation
# Quick Sort
## Method
1. Pick an element (pivot) from the list
	- Pivot is arbitrarily chosen
	- Normally, the first element is selected
2. Partition the list into two halves such that:
	- All the elements in the first half are smaller than the pivot.
	- All the elements in the second half are greater than or equal to the pivot
3. Quick sort the $1^{st}$ half and $2^{nd}$ half

## Algorithm
```Python
def quickSort(a):
    n = len(a)
    if n > 1:
        pivotIndex = partition(a)  # partition the array
        a[0:pivotIndex] = quickSort(
            a[0:pivotIndex]
        )  # recursively quicksort the left half
        a[pivotIndex + 1 : :] = quickSort(
            a[pivotIndex + 1 : :]
        )  # recursively quicksort the right half
    return a

def partition(a):
    pivot = a[0]
    n = len(a)
    pivotIndex = 0
    for i in range(1, n):
        if a[i] < pivot:
            pivotIndex += 1  # Element at index is smaller than pivot -> belongs to the left half. Increment pivotIndex to increase size of left half by 1
            a[i], a[pivotIndex] = a[pivotIndex], a[i]  # move element into left half
    a[0], a[pivotIndex] = a[pivotIndex], a[0]  # move the pivot into place
    return pivotIndex

def main():
    alist = [54, 26, 93, 17, 77, 31, 44, 55, 20]
    quickSort(alist)
    print(alist)

if __name__ == "__main__":
    main()
```
## Analysis
- Time complexity
	- on average, each partition halves the size of the array to be sorted
	- on average, each partition swap half the element
	- on average, algorithm is O(n log n)
	- worst case, algorithm is $O(n^2)$
## Choice of pivot
- in this version of quicksort, the leftmost element of the partition is used as the pivot element
- unfortunately, this causes worst-case behaviour on already sorted arrays because size of sub-array is only reduced by 1
- can be resolved by
	1. a random index of pivot
	2. middle index of the partition for the pivot
	3. the median of the first, middle and last elements of the partition for the pivot