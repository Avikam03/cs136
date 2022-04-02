
![](https://images.unsplash.com/photo-1501594907352-04cda38ebc29?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1632&q=80)


# CS Notes

A list of definitions from the cs lecture notes slides.

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


### <stdio.h>
provides `printf` and `scanf`

### <assert.h>
provides the function `assert`

### <limits.h>
provides the constants `INT_MAX` and `INT_MIN`

### <stdbool.h>
provides the `bool` data type and constants `true` and `false`

### <stdlib.h>
provides the constant `NULL`



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










