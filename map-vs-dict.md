### Getting started

Python:

```python
empty_dict = {}
another_dict = dict()
```

C++:

```c++
#include <unordered_map>

std::unordered_map<std::string, std::string> string_map;
```


### Initializing

Python:

```python
string_dict = {"hello": "world", "hola": "mundo", "bonjour": "monde"}
```

C++:

```c++
std::unordered_map<std::string, std::string> string_map{{"hello", "world"}, {"hola", "mundo"}, {"bonjour", "monde"}}
```


### Inserting

Python:

```python
string_dict["hello"] = "world"
string_dict["hola"] = "mundo"
string_dict["bonjour"] = "monde"
```

C++:

```c++
string_map["hello"] = "world";
string_map.insert(std::make_pair("hola", "mundo")); // Need <utility> header
string_map.emplace(std::piecewise_construct,
                   std::forward_as_tuple("bonjour"),
                   std::forward_as_tuple("monde")); // Constructs in place without a copy or move if possible
```


### Searching

Python:

```python
if "hello" in string_dict:
    print("Yes!")
```

C++:

```c++
if (string_map.find("hello") != string_map.end()) {
    std::cout << "Yes!" << std::endl;
}
```


### Iterating

Python:

```python
for k, v in string_dict: # i is a name for an int, so it can't be mutated, only rebound
    print("{}: {}".format(k, v))
```

C++:

```c++
for (auto p : string_map) { // p is a copy of type std::pair<std::string, std::string>
    std::cout << p.first << ": " << p.second << std::endl;
}

for (auto& p : string_map) { // p is a mutable reference to every pair in string_map
    std::cout << p.first << ": " << p.second << std::endl;
}

for (const auto& p : string_map) { // p is an immutable reference to every pair in string_map
    std::cout << p.first << ": " << p.second << std::endl;
}
```


### Removing

Python:

```python
del string_dict["hello"] # Removes the value "world" of key "hello" from string_dict
```

C++:

```c++
auto found = string_map.find("hello");
if (found != string_map.end()) {
    string_map.erase(found);
}
```

### `std::map` vs `std::unordered_map`

|     | std::map | std::unordered_map |
| --- | --- | --- |
| Implementation | balanced tree or RB tree | hash table |
| Search | log(n) | O(1), up to O(n) for collisions |
| Insert | log(n) + rebalance | Same as Search |
| Delete | log(n) + rebalance | Same as Search |

Source: http://stackoverflow.com/a/13799886/2297365
