
![](https://images.unsplash.com/photo-1501594907352-04cda38ebc29?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1632&q=80)


# CS 136 Notes

A list of definitions and other stuff that I think is important or I can't remember from the cs lecture notes slides.

### Modules
$$\text{A module provides a collection of functions that share a common aspect or purpose}$$
C modules are also known as **libraries**.

Other: 
- Modules do not contain a main function
- Only one main function can be defined per program

#### Motivation for Modules
- **Re-usability**: can be re-used by multiple clients
- **Maintainability**: Code that needs to be changed can be changed in one file.
- **Abstraction**: Client does not need to know how the module is implemented.



### Declaration
$$\text{A declaration introduces an identifier (“name”) into a program and specifies its type}$$

### Definition
$$\text{A definition includes a declaration. It instructs C to "create" the identifier}$$

**Example**: Function Declaration
```c
int my_add(int a, int b);

int main(void) {
	trace_int(my_add(1, 2));
}

int my_add(int a, int b) {
	return a + b;
}
```


**Example**: Variable Declaration
```C
extern int g;

int main(void) {
	printf("%d", g);
}

int g = 7;
```

by declaring the function/variable above main, the function/variable are in scope in the main function even though they are defined below the main function

IMP: Remember that Scope and Memory are different
**Example**:
```C
int main (void) {
	printf("%d", z);
}

int z = 2;
```

Here, since 2 is a global variable, it is in memory before the program is "run". However, it is still not in scope in the main function as it hasn't been declared.


### Static
$$\text{The keyword } \textbf{Static } \text{restricts the scope of a global identifier to the file(module) it is defined in.}$$
{In other words, the keyword static assigns **module scope**

**Note**:
Global identifiers are available to any file in the program (if declared)


### # include
```
A pre-processor directive temporarily modifies a source file just before it is run (does not save the modifications).
```

\# include is a one such pre-processor directive that cut and pastes the contents of another file directly into the current file.


### stdio.h
provides `printf` and `scanf`

### assert.h
provides the function `assert`

### limits.h
provides the constants `INT_MAX` and `INT_MIN`

### stdbool.h
provides the `bool` data type and constants `true` and `false`

### stdlib.h
provides the constant `NULL`. provides `malloc`.



### Cohesion and Coupling
We want to achieve **high cohesion** and **low coupling**.

##### High cohesion
A program where all functions are related and working towards a single goal

##### Low coupling
A program with little interaction between modules.


### Benefits of Information Hiding through Interface Files
There are 2 main benefits

**Security** is important because we don't want the client to tamper with data used by the module. Secondly, by hiding the implementation, we have the **flexibility** to change the implementation in the future, without the client ever knowing.



Following was a midterm question

### Opaque Structure
An *opaque structure* is like a "black box" that the client cannot "see" inside of.
They are implemented in C using incomplete declarations, where a structure is declared without any fields.

**Example**:
```C
struct box           // Incomplete declaration
```

With an incomplete declaration, only pointers to the structure can be defined.
```C
struct box my_box;      // INVALID
struct box *box_ptr;    // VALID
```

In other words, if an incomplete declaration is provided by a module, then the client can not create an instance of the struct or access any of the fields.

A **transparent** structure on the other hand is one whose complete definition is present in the intererfact(.h) file.



### Abstract Data Types
ADTs are data storage modules that only allow the access to the data through interface functions (ADT operations). The underlying data structure and implementation of an ADT are hidden from the client (which provides flexibility and security). The best part about them is that clients can implement them with merely an abstract idea of the structure of the data. 



### Arrays
**Note**:
- Uninitialized Global Arrays are zero-filled.
- Uninitialized Local Arrays are filled with arbitrary ("garbage") local values from the stack.


### Pointer Arithmetic Rules
- When adding an integer `i` to a pointer `p`, the address computed by `p+i` in C is given in "normal" arithmetic by:
$$p + i \times \text{sizeof}(*p)$$
- Subtracting an integer from a pointer `p-i` works in the same way.
- Mutable pointers can be incremented (or decremented). `++p` is the same as `p = p + 1`
- You cannot add 2 pointers
- A pointer can be subtracted from another pointer p if the pointers are the same type (point to the same type). The value of `(p-q)` in C is given in "normal" arithmetic by
$$(p-q)/\text{sizeof}(*p)$$
In other words, if `p = q + i`, then `i = p - q`
- Pointers of the same type can be compared using comparison operators `<, <=, ==, !=, >=, >`


#### Swap function
**Code**:
```C
void swap (int *a, int *b) {
	int temp = *a;
	*a = *b;
	*b = temp;
}
```


### Selection Sort
General Idea: The smallest element is found and made the first element multiple times.

**Code**:
```C
void selection_sort(int a[], int len) {
	int pos = 0;
	for (int i = 0; i < len - 1; i++) {
		pos = i;
		for (int j = i + 1; j < len; j++) {
			if (a[j] < a[pos]) {
				pos = j;
			}
		}
		swap(&a[i], &a[j]);
	}
}
```


### Insertion Sort
General Idea: Each iteration assumes that the first i elements are sorted. the element `a[i]`  is inserted into the correct position each time.

**Code:**
```C
void insertion_sort(int a[], int len) {
	for (int i = 1; i < len; i++) {
		for (int j = i; j > 0 && a[j - 1] > a[j]; j--) {
			swap(&a[j], &a[j - 1]);
		}
	}
}
```


### Quicksort
General Idea: It is a divide and conquer algo. an element is selected as a pivot, then the list is divided into 2 - greater than the pivot, and lesser than the pivot. both lists are sorted using this logic recursively.

**Code:**
```C
void quicksort_range(int a[], int first, int last) {
	if (last < first) return;

	int pivot = a[first]; // this code makes the first element the pivot
	int pos = last;

	for (int i  = last; i > first; i--) {
		if (a[i] > pivot) {
			swap(&a[pos], &a[i]);
			pos--;
		}
	}

	swap(&a[first], &a[pos]);
	quick_sort_range(a, first, pos - 1);
	quick_sort_range(pos + 1, last);
}

void quicksort(int a[], int len) {
	quicksort_range(a, 0, len - 1);
}
```


### Linear Search
General Idea: Iterating through all elements in a list and checking when `item = a[i]`.


### Binary Search
Requires: List needs to be sorted
General idea: Divide and Conquer
$$T(n) = \sum_{i = 1}^{\log_2{n}} O(1) = O(\log{n})$$
**Code:**
```C
int binary_search(int item, const int a[], int len) {
	int low = 0;
	int high = len - 1;
	while (low <= high) {
		int mid = (low + high) / 2;
		if (a[mid] == item) {
			return mid;
		} else if (a[mid] < item) {
			low = mid + 1;
		} else {
			high = mid - 1;
		}
	}
	return -1;
}
```


Following was a midterm question

### Oversized Array
Because of the concept of pre-defined lengths of arrays, oversized arrays are used.

They have more number of elements than are used. 
- They can be quite wasteful if maximum length is excessively high.
- They can be quite restrictive if maximum length is too small.

When working with oversized arrays, we need to keep track of
- the actual length of the array (where the last element is)
- the maximum possible length


### Recurence Relations

| Syntax                                 | Description |
| -----------                            | ----------- |
| T(n) = $O(1) + T(n - k_1)$             | O(n)        |
| T(n) = $O(n) + T(n - k_1)$               | O(n^2)      |
| T(n) = $O(n^2) + T(n - k_1)$             | O(n^3)      |
| T(n) = $O(1) + T(n/k_2)$                | O(log n)    |
| T(n) = $O(1) + k_2 T(n/k_2)$            | O(n)        |
| T(n) = $O(n) + k_2 T(n/k_2)$             | O(n log n)  |
| T(n) = $O(1) + T(n - k_1) + T(n - k_1')$  | O(2^n)      |

where $k_1, k_1' \geq 1$  and $k_2  > 1$



### Sorting Summary

| Algorithm  |  Best Case |  Worst Case |
|---|---|---|
| Selection Sort | O(n^2)   | $O(n^2)$  |
| Insertion Sort  | O(n)  | $O(n^2)$  |
| Quick Sort  | O(n log n)  | $O(n^2)$  |





### The maximum subarray problem
very important

There are several approaches. The best one is:
```C
int max_subarray(const int[], int len) {
	int maxsofar = 0;
	int maxendhere = 0;
	for (int i = 0; i < len; i++) {
		maxendhere = max(maxendhere + a[i], 0);
		maxsofar = max(maxsofar, maxendhere);
	}
	return maxsofar;
}
```



### Implementation of String Functions

#### strlen
```C
int strlen(const char s[]) {
	int index = 0;
	while (s[index]) {
		index++;
	}
	return index;
}
```

Using pointer arithmetic
```C
int strlen(const char s[]) {
	char *new = s;
	while (new) {
		new++;
	}
	return (new - s);
}
```


**Note**: Do not put strlen inside a loop unnecessarily. it'll lead to increased run time.


#### strcmp
```C
int strcmp(const char s1; const char s2) {
	int i = 0;
	while (s1[i] == s2[i] && s1[i]) {
		++i;
	}
	return s1[i] - s2[i];
}
```

In the case when `s1` has lesser characters than `s2`, the loop will break. since `s1[i] = 0` , a negative value will be returned as `0 - positive num < 0`.

Similarly, if `s2` has lesser characters than `s1`, the loop will break. since `s2[i] = 0`, a positive value will be returned as  `positive num - 0 > 0`.


### strcpy
```C
int strcpy(char *dest, const char *src) {
	char *d = dest;
	while (*src) {
		*d = *src;
		++d;
		++src;
	}
	*d = 0;
	return dest;
}
```

### strcat
```C
char *strcat(char *dest, const char *src) {
	strcpy(dest + strlen(dest), src);
	return dest;
}
```


### String literals
C strings in quotations that in expressions are known as string literals

**Example:**
`printf("literal\n");`
`printf("literal %s\n", "another literal");`

These are stored in the read-only data section.


**Good Example**:
```C
int main (void) {
	char a[] = "mutable char array";
	char *p = "constant string literal";
}
```

The first reserves space for an unitialized 19 character array in the stack frame.
The second reserves space for a char pointer (p) in the stack frame (8 bytes), initialized to point at a string literal (const char array) created in the read only data section.


It is helpful to think of arrays as **constant** pointers (that cannot change what it "points" at).

**Example**:
```C
char a[] = "pointers are not arrays";
char *p = "pointers are not arrays";
char d[] = "different string";
```

As we said above, since arrays can be seen as constant pointers, the operation
`a = d`  is invalid. 

However, we can change the value it points to.
`a[0] = 'P'`  is valid.

In this case, since p is merely a pointer to a character, we can change what it points to.
`p = d`
`p[0] = 'D'` is valid.



### Array of Strings
This is an array of pointers.

```C
char *aos[] = {"cs136", "is", "nice"};
```

Here, aos is an array of pointers with each pointer pointing to a string literal.
Though it is not an actual 2D array, elements can be accessed using 2D array syntax

**Example**:
`aos[1][1]` is 's'




### The heap
- The heap can be thought of as a large pool of memory that is available to a program.
- Memory is ***allocated*** from the heap upon request.
- This memory can be *borrowed* and *returned* back to the heap when it is no longer needed (deallocation).
- Returned memory may be reused for a future allocation.


### Advantages of heap-allocated memory
- **Dynamic**: The allocation size can be determined at run time (while the program is running).
- **Resizable**: A heap allocation can be resized.
- **Duration**: Heap allocations persist until they are "freed". A function can create multiple data structures using allocation that can exist even after the function returns.
- **Safety**: If memory runs out, it can be detected and handled properly (unlike stack overflows).


Heap memory provided by malloc is **unitialized**.


### Disadvantage of heap-allocated memory
After multiple allocations and deallocations, the heap can become fragmented, which may affect the performance of `malloc`.



General Note:
- An unsuccessful call to `malloc` returns `NULL`.
- `malloc` and `free` are both considered O(1).
- A pointer to a freed allocation is known as a “dangling pointer”. It is often good style to assign NULL to a dangling pointer.


### Memory Leak
A memory leak occurs when allocated memory is not eventually freed.


####  Garbage Collection
A garbage collector detects when memory is no longer being used and automatically frees memory and returns it to heap.

**Disadvantage**: It is slow and affects performance. 

Racket has a garbage collector, C does not.


### Merge Sort
General idea: divide and conquer

```C
void merge(int dest[], const int src1[], int len1, const int src2[], int len2) {
	int pos1 = 0;
	int pos2 = 0;
	for (int i = 0; i < len + len2; i++) {
		if (pos2 == len2 || (pos1 < len1  && src1[pos1] < src2[pos2])) {
			dest[i] = src1[pos1];
			pos1++;
		} else {
			dest[i] = src2[pos2];
			pos2++;
		}
	}
}

void merge_sort(int a[], int len) {
	if (len <= 1) return;
	int llen = len/2;
	int rlen = len - llen;

	int *left = malloc(llen *sizeof(int));
	int *right = malloc(rrlen * sizeof(int));

	for (int i = 0; i < llen; i++) left[i] = a[i];
	for (int i = 0; i < rlen; i++) right[i] = a[i + llen];

	merge_sort(left, llen);
	merge_sort(right, rlen);

	merge(a, left, llen, right, rlen);
	
	free(left); 
	free(right);
}

```

Time Complexity: `O(n log n)` even in the worst case


### strdup
```C
char *strdup (const char *s) {
	char *newstr = malloc((strlen(s) + 1) *sizeof(char));
	strcpy(newstr, s);
	return newstr;
}
```


### realloc
A realloc can be though of as a `malloc`, then `copy`, then a `free` - all combined into one function.

It is extremeley important to return the address returned by realloc as if additional memory is requested, then the location of the data in the heap might change. 

On the other hand, if the size is made smaller,  extraneous memory is discarded.

> Time:  O(n), where n is min(newsize, oldsize)


### Doubling Strategy
```C
char *read_str(void) {
	char c = 0;
	if (scanf(" %c", &c) != 1) return NULL;
	int maxlen = 1;
	int len = 1;
	char *str = malloc(maxlen * sizeof(char));
	str[0] = c;
	while (1) {
		if (scanf("%c", &c) != 1) break;
		if (c == ' ' || c == '\n') break;
		if (len == maxlen) {
			maxlen *= 2;
			str = realloc(str, maxlen * sizeof(char));
		}
		len++;
		str[len - 1] = c;
	}
	str = realloc(str, (len + 1) * sizeof(char));
	str[len] = '\0';
	return str;
}
```

> Time: O(n)










