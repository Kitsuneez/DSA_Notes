	# Symbol Tables
- Data structure used for associative search
	- Purpose is to associate a value to a key
	- Client can insert key-value pairs into a symbol table
	- Client can Search or delete value associated with a given key
	- Also known as maps, dictionary, associative arrays
	- Language supported: Java, C#, C++, Perl, PHP, JavaScript, Python
	- Example: Python Dictionary
	   ```Python
	   capitals = {}
	   capitals["Singapore"] = "Singapore"
	   capitals["France"] = "France"
	   ```
## Symbol Table API
- Associative array abstraction. Associate one value with each key

| public class ST<Key, Value>  | Description                                 |
| ---------------------------- | ------------------------------------------- |
| ST()                         | Create an empty symbol table                |
| void put(Key key, Value val) | Put/insert key-value pair into the table    |
| Value get(Key key)           | Get/Search value paired with key            |
| boolean contains(Key key)    | Check if there is a value paired with key   |
| void delete(Key key)         | Remove key and its value from table         |
| boolean isEmpty()            | is the table empty?                         |
| int size()                   | Get number of key-values pairs in the table |
| Iterable<Key\> keys()        | Get all keys in the table                   |
## Convention for Symbol Table
- values are not usually not null (Java and Python allows null value)
- Method `get()` returns null if key is not present
	- Python throws a KeyError exception
- Method put() overwrites old value with new value
	`put("singapore.edu.sg", "8.8.8.8")`
- Easy to implement contains()
```Python
def contains(key):
	return get(key) == None
```
- Can implement lazy version of delete():
```Python
def delete(key):
	put(key, None)
```
## Keys and Values, Equality Test
- Values types: any generic type
- Key type:
	- Primitive types (e.g., int, char), use default language equality test operator `==` to test for equality
	- user defined types (e.g Date, Record), may need to override/implement custom equality operator to test equality.
```Python
class Date:
	day=0
	month=0
	year=0
	def __init__(self, d,m,y):
		self.day = d
		self.month = m
		self.year = y
		
	a = Date(1,1,2015)
	b = Date(1,1,2015)
	print(f"a==b is {a==b}")
```
---
```Output
a==b is False
```
---
### Solution
- override default equality operator `__eq__()` in the Python class to test for equality of individual attribute of the class
```Python
class Date:
	day=0
	month=0
	year=0
	def __init__(self, d,m,y):
		self.day = d
		self.month = m
		self.year = y
		
	def __eq__(self, other):
		if self.day == other.day and 
		self.month == other.month and 
		self.year == other.year:
			return True
		return False
		
	a = Date(1,1,2015)
	b = Date(1,1,2015)
	print(f"a==b is {a==b}")
```
---
```Output
a==b is True
```
---
## Example - Associate Value
- Build ST by associating value _i_ with the _$i^{th}$_ character from the standard input. This is a symbol table implemented on top of Python's dictionary type.
```Python
class ST:
	st = {}
	def put(self, key, value):
		self.st[key] = value
		
	def get(self, key):
		return self.st[key]
	
	def contains(self, key):
		return key in self.st
	
	def delete(self, key):
		del self.st[key]
	
	def isEmpty(self):
		return len(self.st) == 0
	
	def size(self):
		return len(self.st)
	
	def keys(self):
		return self.st.keys()
```
```Python
def main():
	st = ST()
	count = 0
	while True:
		x = input("Enter the next character")
		print(x)
		if x!="":
			st.put(x,count)
			count += 1
		else:
			break
	print(st.st)
	
main()
```
## Linear/Sequential Search in Unordered Linked-List
- Data Structure: Maintain an (unordered) linked list of key-value pairs.
- Search: Scan through all keys until a match is found.
- Insert: Scan through all keys until you find a match; if no match add to front
### Implementation
```Python
class Node:
	key = 0
	val = 0
	next = None
	
	def __init__(self, k,v):
		self.key = k
		self.val = v
		
class linearSearchST:
	head = None
	def put(self, key, value):
		if self.head is None:
			newNode = Node(key, value)
			self.head = newNode
		else:
			node = self.head
			# if the first node matches, return immediately
			if node.key == key:
				node.val = value
				return
			# iteratively search the rest of the nodes
			while node.next is not None:
				if node.key == key:
					node.val = vallue
					return
				node = node.next # advance pointer to next node
			# node not found in list, append to front of list
			newNode = Node(key, value)
			newNode.next = self.head
			self.head = newNode
```
### Analysis
- Complexity of Sequential Search in an unordered array -> O(N)
- Complexity of Insert in an unordered array -> O(N) 
## Binary Search in an Ordered Array
- Data Structure: Maintain an ordered array of key-value pairs
- Rank helper function: How many keys < k?
### Implementation
```Python
class BinarySearchST:
	keys = []
	vals = []
	def put(self, key, value):
		r = self.rank(key)
		if r >= len(self.keys) or self.keys[r] not in keys
			self.keys.insert(r, key)
			self.vls.insert(r, value)
		else:
			self.vals[r] = value
	def rank(self, key):
		lo = 0
		hi = len(self.keys) - 1
		while lo <= hi:
			mid = lo + (hi-lo)/2
			if key < self.keys(mid):
				hi = mid - 1
			else:
				if key > self.keys[mid]:
					lo = mid + 1
				else:
					return mid
		return lo
```
### Analysis
- Complexity of Binary Search in an ordered array -> O(log N)
- Complexity of insert in an ordered array -> O(N)
## Hash Table
- Efficient data structure for dynamic sets with simple operations:
	- Insert/Put, Search/Get, Delete
- Searching in hash table in the worst case with linked-list is O(n), but can be reduced to O(1) with a good hash function
- Size of the table usually proportional to the number of stored values
### Direct Addressing Hash Table
- use with a relatively small set of k keys
- Given a hash table `H[0, ..., m-1]` with m places, m >= k
- Hash function: h(k) -> k maps key k to index k
### Pros and Cons
- Advantage
	- simple implementation of insert, search, delete
		- `Search(H,k): return H[k]`
		- `Insert(H,k,x): H[k] = x`
		- `Delete(H,k): H[k] = NULL
- Disadvantage: impractical for large number of keys
- Another problem is when actual number of keys observed, k may be very small compared to the size of table m:
	- M >> k, much space is wasted
	- Hash function with collision, hashing with separate chaining, hashing with probing
### Hash Function with Collision
- restrict the size of hash table, use hash function that maps set of possible keys to a much smaller set m
- Modulus 7 function makes any integer key between `[0..6]`
- Problem with Collision: two distinct keys can be hashed onto the same hash code: $15\mod(7) =1$, $22 \mod(7) = 1$ 
### Design of Good Hash Functions
- Objectives
	- Good distribution of the search key k to the index range of the hash table H
	- Without underlying assumptions about a probability distribution
- Division Method aka Modular Hashing
	- Hash function h is defined by: `h(k) = k mode m`, where m is the size of the hash table H
	- Example: let m = 12, k = 100, h(100) = 4
### Separate Chaining
- Also known as Open Chaining or Closed Addressing
- Use an array of size m < k linked-lists
	- Hash: map key to integer _i_ between 0 to m-1
	- Insert: put at front of _$i^{th}$_ chain (if not already there)
	- Search: need to search only _$i^{th}$_ chain
#### Implementation
```Python
class Node:
	key = value = 0
	next = None
	
	def __init__(self, k, v):
		self.key = k
		self.value = v
		
class SeparateChainingST:
	m = 5
	st = [None for _ in range(0,m)]
	
	def hashcode(self, key):
		return ord(key) % self.m
	
	def put(self, key, value):
		k = self.hashcode(key)
		node = self.st[k]
		while node is not None:
			if node.key == key:
				node.value = value
				return
			node = node.next
		node = Node(key, value)
		node.next = self.st[k]
		self.st[k] = node
```
#### Resizing
- goal: Average length of list k/m = constant.
	- double size of array M when k/m >= 8 
	- Halve size of array M when k/m <= 2
	- Need to rehash all keys when resizing
# Linear Probing
- Key idea: when new key collides, find the next empty slot and put it there
	- also known as Closed Hashing or Open Addressing
	- hash function: i between 0 and m-1
	- Insert: put at table _i_ if free; if not try `(i+1)%m`, `(i+2)%m`, etc...
	- Search: Search table index _i_; if occupied but not match, try `(i+1)%m`, `(i+2)%m`, etc
- other types of probe sequence
	- Quadratic Probing
	- Double Hashing
## Quadratic Probing
- Hash function: Map key to integer i between 0 and m-1
- Insert: put at table index i if free, if not try `(i+1)%m`, `(i+4)%m`, `(i+9)%m`, etc
$$h+1^2, h+2^2, h+3^2, h+4^2, h+5^2$$
$$h+1, h+4, h+9, h+16, h+25$$
- Search: search table index i; if occupied but not match, try `(i+1)%m`, `(i+4)%m`, etc
![Pasted image 20260301151129.png](./images/Pasted_image_20260301151129.png)
## Double Hashing
- Probe Sequence is
	- H(k) mod m
	- $(h_1(k) + 1*h_2(k))$ mod m
	- $(h_1(k) + 2*h_2(k))$ mod m
	- ...
- $h_2(k)$ should never evaluate to 0
	- $h_2(k)$ = (k mod 5) + 1
![Pasted image 20260301151312.png](./images/Pasted_image_20260301151312.png)
## Hashing Analysis
- Best and Average case : O(1)
- Worst case: O(N)
- Separate Chaining:
	- Performance degrades gracefully
	- Clustering less sensitive to poorly-designed hash function
- Linear Probing:
	- Less wasted space
	- better cache performance

# Binary Search Tree (BST)
- A BST is a binary tree in symmetric order
- a binary tree is either (1) Empty or (2) Two disjoint binary tree (left and right)
- Each node has a key and every node's key is
	- larger than all keys in its left subtree
	- smaller than all keys in its right subtree
## Searching
- if less, go left
- if greater, go right
- if equal, search hit
## Insert
- if less, go left
- if greater, go right
- if null, insert
## Data Structure
- A BST is a reference to a root node
- a node is composed of the following fields
	- a key and a value
	- a reference to the left and right subtree
```Python
class BST:
	root = None
	def get(self, key):
	
	def put(self, key, val):
	
	def delete(self, k):
	
class Node:
	left = None
	right = None
	count = 0
	key = 0
	val = 0
	
	def __init__(self, key, val):
		self.key = key
		self.val = val
```
## Implementation
- GET
	- Cost: Number of compares = 1 + depth of Node
	- return value corresponding to a given key, or None if no such key
```Python
def get(self, key):
	p = self.root
	while p is not None:
		if p.key == key:
			return p.val
		elif p.key > key:
			p = p.left
		else:
			p = p.right
	return None
```
- PUT
	- Cost: Number of compares = 1 + depth of Node
	- Associate a value to a key
	- Search for key. consider two cases
		- key in tree => update value
		- key not in tree => add new node
```Python
def insert(self, key, val):
	self.root = self.put(self.root, key, val)
	
def put(self, node, key, val):
	if node is None:
		return Node(key, val, 1)
	if key < node.key:
		node.left = self.put(node.left, key, val)
	elif key > node.key:
		node.right = self.put(node.left, key, val)
	else:
		node.val = val
	node.count = 1 + self.size(node.left) + self.size(node.right)
	return node
```
## Tree Shape
- Many BSTs corresponds to same set of keys
- Number of compares for Search/Insert is equal to 1 + depth of node ![Pasted image 20260304145833.png](./images/Pasted_image_20260304145833.png)
- Bottom line: tree shape depends on order off insertion
## Traversal of nodes in BST
- visiting all the nodes in a graph
- can be specified by order of three objects to visit
	- current node
	- left subtree
	- right subtree 
	![Pasted image 20260304150502.png](./images/Pasted_image_20260304150502.png)
- inOrder
	1. Left
	2. Current
	3. Right
	- `[2, 3, 4, 5, 6, 7, 8, 9, 11, 12, 15, 19, 20]`
- PreOrder
	1. Current
	2. Left
	3. Right
	- `[7, 4, 2, 3, 6, 5, 12, 9, 8, 11, 19, 15, 20]`
- PostOrder
	1. Left
	2. Right
	3. Current
	- `[3, 2, 5, 6, 4, 8, 11, 9, 15, 20, 19, 12, 7]`
## BST Maximum & Minimum
- Minimum
	- Smallest key in the table
- Maximum
	- Largest key in the table
### Subtree Count
- in each node, we store the number of nodes in the subtree rooted at that node
- implement `size()`
- return the count at the root
![Pasted image 20260304151159.png](./images/Pasted_image_20260304151159.png)
### Deleting Minimum Key
- To delete the minimum key:
	- Go left until find a node with a null left link
	- Replace that node by its right link
	- Update subtree count
```Python
def deleteMin(self):
	self.root= self.deleteMin2(self.root)
	
def deleteMin2(self, node):
	if node.left is None:
		return node.right
	node.left = self.deleteMin2(node.left)
	node.count = 1 + sel.size(node.left) + self.size(node.right)
	return node
```
![Pasted image 20260304151429.png](./images/Pasted_image_20260304151429.png)
### Deleting Any Key
- to delete a node with key k: search for node t containing key k
- Case 0
	- 0 children
	- Delete t by setting parent link to null
![Pasted image 20260304151558.png](./images/Pasted_image_20260304151558.png)
- case 2
	- 2 children
	- find the successor x of t
	- delete t
	- put x in t's spot
![Pasted image 20260304151650.png](./images/Pasted_image_20260304151650.png)
```Python
def delete(self, key):
	self.root = self.delete2(self.root, key)
	
def delete2(self, node, key):
	if node is None:
		return None
	# Search for key
	if key < node.key:
		node.left = self.delete2(node.left, key)
	elif key > node.key
		node.right = self.delete2(node.right, key)
	else:
		# Case 0 & Case 1 0 or 1 child
		if node.right is None:
			return node.left
		if node.left is None:
			return node.right
		#Case 2 2 children, replace with successor
		t = node
		node = self.min(t.right)
		node.right = self.deleteMin2(t.right)
		node.left = t.left
	# update subtree count
	node.count = self.size(node.left) + self.size(node.right)+1
	return node
```
# 2-3 Tree
- Allow 1 or 2 keys per node
	- 2-node: one key, two children
	- 3-node: two keys, three children
- Perfect balance
	- every path from root to null link has same length
## Search 
- Compare search key against keys in node
- Find interval containing search key
- follow associated link (recursively)
![Pasted image 20260304155239.png](./images/Pasted_image_20260304155239.png)
## Insert
- Insertion into a 2 node at bottom.
	- add new key to 2-node to create a 3-node
	![Pasted image 20260304155409.png](./images/Pasted_image_20260304155409.png)
- Insertion into a 3-node at bottom
	- Add new key to 3-node to create temporary 4-node
	- Move middle key in 4-node into parent
	- Repeat up the tree, as necessary
	- If you reach the root and it is a 4-node, split it into tree 2-nodes
	![Pasted image 20260304155921.png](./images/Pasted_image_20260304155921.png)
## Local Transformation
- splitting a 4-node is a local transformation: constant number of operations
![Pasted image 20260304160023.png](./images/Pasted_image_20260304160023.png)
## Global Properties
- Invariants: Maintains symmetric order and perfect balance
- Each transformation maintains symmetric order and perfect balance
![Pasted image 20260304160114.png](./images/Pasted_image_20260304160114.png)
## Tree Performance
- Perfect balance
	- each path from root to null link has the same length
- Tree Height
	- Worst case: log N \[all 2 node]
	- Best Case: $log_3$ N \[all 3 node]
	- Between 12 and 20 for a million nodes
	- Between 18 and 30 for a billion nodes
- Bottom line: Guaranteed logarithmic performance for search and insert
## Tree Implementation
- direct implementation is complicated, because
	- maintaining multiple node types is cumbersome
	- Need multiple compared to move down tree
	- need to move back-up the tree to split 4 nodes
	- large number of cases for splitting
- there is a better way 
### Implementation with Binary Tree
- Challenge: How to represent a 3 node?
- Approach 1: regular BST
	- No way to tell a 3-node from a 2-node
	- cannot map from BST back to 2-3 Tree
- Approach 2: regular BST with "glue" nodes
	- waste space, wasted link
	- Code probably messy
- Approach 3: regular BST with red "glue" links
	- widely used in practice
	- Arbitrary restriction: red links lean left
### Left Learning Red-Black BSTs
- represent 2-3 tree as a BST
- Use "Internal" left-leaning links as "glue" for 3-nodes
![Pasted image 20260304160820.png](./images/Pasted_image_20260304160820.png)
- no node has two red links connected to it \[no 4 nodes]
- each path from root to null link has the same number of black links
- Red links lean left
#### LLRB BST vs 2-3 Trees
- key property
	- 1-1 correspondence between 2-3 Tree and LLRBT
![Pasted image 20260304161047.png](./images/Pasted_image_20260304161047.png)
### LLRBT Search
- Observation:
	- search is the same as for elementary BST (ignore color)
	- Runs faster because of better balance
- Remark: most other operations (e.g min, max, size) are the same ![Pasted image 20260304161303.png](./images/Pasted_image_20260304161303.png)
### LLRBT Representation
- Each node is pointed to by precisely one link (from its parent)
	- can encode colour of links in nodes
```Python
class Node:
	RED=False
	BLACK = True
	left = None
	right = None
	count = 0
	key = 0
	val = 0
	colour = None
	
	def __init__(self, key, val):
		self.key = key
		self.val = val
		self.count = 1
		self.colour = self.RED
	
	def isRed(self, n):
		if n is None:
			return False
		else:
			return n.color == NODE.RED
```
![Pasted image 20260304161555.png](./images/Pasted_image_20260304161555.png)
### LLRBT Maintenance
- Basic strategy: maintain 1-1 correspondence with 2-3 trees
- During internal operations, maintains
	- Symmetric order
	- Perfect black balance ( but not necessarily colour invariants) ![Pasted image 20260304161729.png](./images/Pasted_image_20260304161729.png)
- Apply elementary red-black BST operation: rotation & flip
### LLRBT Rotation
- Left rotation
	- orient a (temporarily) right leaning red link to lean left
![Pasted image 20260304161841.png](./images/Pasted_image_20260304161841.png)
```Python
def rotateLeft(self, h):
	assert(self.isRed(h.right))
	x = h.right
	h.right = x.left
	x.left = h
	x.color = h.color
	h.color = Node.RED
	return x
```
- Right Rotation
	- orient a left-leaning red link to (temporarily) lean right ![Pasted image 20260304162037.png](./images/Pasted_image_20260304162037.png)
```Python
def rotateRight(self, h):
	assert(self.isRed(h.left))
	x = h.left
	h.left = x.right
	x.right = h
	x.color = h.color
	h.color = NODE.RED
	return x
```
- Color Flip
	- Recolour to split a (temporarily) 4-node ![Pasted image 20260304162242.png](./images/Pasted_image_20260304162242.png)
```Python
def flipColors(self, h):
	assert(not self.isRed(h))
	assert(self.isRed(h.left))
	assert(self.isRed(h.right))
	h.color = Node.RED
	h.left.color = Node.BLACK
	h.right.color = Node.BLACK
```
### LLRBT Insertion
- insert into a tree with exactly 1 node ![Pasted image 20260304162437.png](./images/Pasted_image_20260304162437.png)
- insert into a 2-node at the bottom
	- do standard BST insert, color new link red
	- if new red link is a right link, rotate left 
	![Pasted image 20260304162657.png](./images/Pasted_image_20260304162657.png)
- Insert into a tree with exactly 2 nodes ![Pasted image 20260304162736.png](./images/Pasted_image_20260304162736.png)
- insert into a 3 node at the bottom
	- do standard BST; color new link red
	- Rotate to balance the 4 node (if needed)
	- Flip colors to pass red link up one level
	- Rotate to make lean left (if needed)
	- Repeat case 1 or case 2 up the tree (if needed)
### LLRBT Implementation
- Same code for all cases
	- Right child red, left child black: rotate left
	- left child, left - left grandchild red: rotate right
	- Both children red: flip colours
![Pasted image 20260304163342.png](./images/Pasted_image_20260304163342.png)
```Python
def put(self, node, key, val):
	# Insert at bottom and color it red
	if node is None:
		return Node(key, val.Node.RED)
	if key < node.key:
		node.left = self.put(node.left, key, val)
	elif key > node.key:
		node.right = self.put(node.right, key, val)
	else:
		node.val = val
	
	#Lean left
	if self.isRed(node.right) and not self.isRed(node.left):
		node = self.rotateLeft(node)
	#balance 4 node
	if self.isRed(node.left) and self.isRed(node.left.left):
		node = self.rotateRight(node)	
	# split 4 node
	if self.isRed(node.left) and self.isRed(node.right):
		self.flipColours(node)
		
	return node
```

# Summary
## Searches

| Type               | Worst Case<br>Search | Worst Case<br>Insert | Worst case<br>Delete | Avg Case<br>Search hit | Avg Case<br>Insert | Avg Case<br>Delete |
| ------------------ | -------------------- | -------------------- | -------------------- | ---------------------- | ------------------ | ------------------ |
| Linear Search      | O(N)                 | O(N)                 | O(N)                 | O(N)                   | O(N)               | O(N)               |
| Binary Search      | O(log N)             | O(N)                 | O(N)                 | O(log N)               | O(N)               | O(N)               |
| Binary Search Tree | O(N)                 | O(N)                 | O(N)                 | O(log N)               | O(log N)           | O($\sqrt{N}$)      |
| 2-3 Tree           | O(log N)             | O(log N)             | O(log N)             | O(log N)               | O(log N)           | O(log N)           |
| LLRBT              | O(log N)             | O(log N)             | O(log N)             | O(log N)               | O(log N)           | O(log N)           |

## Hash Table
- Best and Average -> O(1)
- Worse -> O(N)