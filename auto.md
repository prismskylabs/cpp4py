### auto


`auto` almost always returns a copy, and should be treated as a locally scoped object.


For primitives:

```c++
int a = 1;

auto b = a; // Equivalent to int b = a;
b = 2; // This does NOT change a to 2
```


For `const` primitives:

```c++
const int a = 1;

auto b = a; // Equivalent to int b = a; Notice that b is non-const
b = 2; // This is allowed and does NOT change b to 2
```


For primitive references:

```c++
int a = 1;
int& b = a;

auto c = b; // Equivalent to int c = b; Notice that c is not a reference
c = 2; // This does NOT change a or b to 2
```


For `const` primitive references:

```c++
int a = 1;
const int& b = a;

auto c = b; // Equivalent to int c = b; Notice that c is not a reference and non-const
c = 2; // This is allowed and does NOT change a or b to 2
```


For pointers:

```c++
int a = 1;
int* b = &a;

auto c = b; // Equivalent to int* c = b;

int d = 3;
c = &d; // This does NOT change b to point to d
```


### auto&

`auto&` almost always creates a mutable reference.


For primitives:

```c++
int a = 1;

auto& b = a; // Equivalent to int& b = a;
b = 2; // This does change a to 2
```


For `const` primitives:


```c++
const int a = 1;

auto& b = a; // Equivalent to const int& b = a; Notice that b is STILL const
b = 2; // This is NOT allowed
```


For primitive references:


```c++
int a = 1;
int& b = a;

auto& c = b; // Equivalent to int& c = b;
c = 2; // This does change a and b both to 2
```


For `const` primitive references:


```c++
int a = 1;
const int& b = a;

auto& c = b; // Equivalent to const int& c = b; Notice that c is STILL const
c = 2; // This is NOT allowed
```


For pointers:

```c++
int a = 1;
int* b = &a;

auto& c = b; // Equivalent to int*& c = b;

int d = 2;
c = &d; // This changes where b points to as well and a remains unchanged
```


### const auto&


`const auto&` almost always creates an immutable reference.


For primitives:

```c++
int a = 1;

const auto& b = a; // Equivalent to const int& b = a;
b = 2; // This is NOT allowed
```


For `const` primitives:


```c++
const int a = 1;

const auto& b = a; // Equivalent to const int& b = a; Notice that b is STILL const
b = 2; // This is NOT allowed
```


For primitive references:


```c++
int a = 1;
int& b = a;

const auto& c = b; // Equivalent to const int& c = b;
c = 2; // This is NOT allowed
```


For `const` primitive references:


```c++
int a = 1;
const int& b = a;

const auto& c = b; // Equivalent to const int& c = b; Notice that c is STILL const
c = 2; // This is NOT allowed
```


For pointers:

```c++
int a = 1;
int* b = &a;

const auto& c = b; // Equivalent to const int*& c = b;

int d = 2;
c = &d; // This is NOT allowed
```
