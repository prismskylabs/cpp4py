## Getting started

1. Go to http://coliru.stacked-crooked.com/
2. Useful includes:
  * <iostream> <--- for std::cout
  * <vector>   <--- for std::vector
  * <string>   <--- for std::string
3. int main() {}
4. std::cout << "hello world" << std::endl;

## Names vs references vs values

```python
my_list = [1, 2, 3]
another_list = [3, 4, 5]
my_list = [3, 4, 5]
```

```c++
int a = 1;
int& b = a;
b = a = 1;
b = 2;
a = 2;
```
