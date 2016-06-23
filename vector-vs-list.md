### Getting started

Python:

```python
empty_list = []
another_list = list()
```

C++:

```c++
#include <vector>

std::vector<int> empty_list;
```


### Initializing

Python:

```python
int_list = [1, 2, 3]
```

C++:

```c++
std::vector<int> int_list{1, 2, 3;
```


### Appending

Python:

```python
int_list.append(4)
int_list.append(5)
```

C++:

```c++
int_list.push_back(4);
int_list.emplace_back(5); // Constructs in place without a copy or move
```


### Iterating

Python:

```python
for i in int_list: # i is a name for an int, so it can't be mutated, only rebound
    print i
```

C++:

```c++
for (auto i : int_list) { // i is a copy of every item
    std::cout << i << std::endl;
}

for (auto& i : int_list) { // i is a mutable reference to every item
    std::cout << i << std::endl;
}

for (const auto& i : int_list) { // i is an immutable reference to every item
    std::cout << i << std::endl;
}
```


### Indexing

Python:

```python
print int_list[2]
int_list[4] = 6
```

C++:

```c++
std::cout << int_list[2] << std::endl;
int_list[4] = 6;
```


### Removing

Python:

```python
int_list.remove(4) # Removes the first element of value 4
```

C++:

```c++
#include <algorithm>

auto found = std::find(int_list.begin(), int_list.end(), 4);
if (found != int_list.end()) {
    int_list.erase(found);
}
```
