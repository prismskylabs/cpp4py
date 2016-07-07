### RAII

Resource Acquisition Is Initialization is an idiom that equates initializing an object with acquiring a resource. As long as that object exists, the resources that object uses belong to that object. On the flipside, when the object ceases to exist (is destructed), then the resources are released.

RAII makes managing resources an exercise in managing objects. If objects don't leak (ie, they don't get lost, you know where every object is at any state of the program), then your resources do not leak.

In C++, this is the equivalent of ensuring that each member field of a class is properly released when an object of that class is released. This assurance is crystalized into two related "Rules":

- The Rule of Five
- The Rule of Zero

### The Rule of Five

There are five "special member functions", all related to how resources are moved in and out of an object of a class. These are:

- Copy constructor `Object(const Object& obj);`
- Copy assignment operator `Object& operator=(const Object& obj);`
- Move constructor `Object(Object&& obj);`
- Move assignment operator `Object& operator=(Object&& obj);`
- Destructor

Defining any of these member functions typically means you're managing a resource manually. This has great potential for resource errors.

### Example memory errors

Consider:

```C++
class Object {
  public:
    Object() {
      string = new char[10];
    }

    ~Object() {
        delete[] string;
    }

    char* string;
};
```

By default, C++ will _not_ generate the other special member functions if the destructor is user-defined. If we give `Object` the default (member-wise) special member functions in order to make `Object` copyable and `movable`, then we've just introduced a bug:

```C++
Object copy_to;
{
    Object object;
    copy_to = object; // This does a copy assignment to copy_to, which copies object.string (which is a pointer) to copy_to.string
} // At this point, object's destructor is called and object.string is deleted
```

Two memory errors here:

1. `copy_to.string` was originally not deleted, so its original memory leaks
2. `copy_to.string`'s new memory location was just deleted, so it now points to bunk memory

The solution is to define a custom copy assignment operator:

```C++
#include <cstring>

class Object {
  public:
    Object() {
      string = new char[10];
    }

    Object& operator=(const Object& obj) {
        std::strcpy(this->string, obj.string);
        return *this;
    }

    ~Object() {
        delete[] string;
    }

    char* string;
};
```

This does a character-wise copy of each character in `obj.string` into existing memory at `this->string`, which avoids both leaking `this->string` and pointing it to bunk memory.

This illustrates the essence of the Rule of Five:

> Defining any of the special member functions strongly indicates that you should define all of the special member functions


### A better way

It's cumbersome to have to write custom special member functions for each user defined class. The chances of getting them wrong are pretty high as well. There's a better way, and it's called the Rule of Zero:

> Define no special member functions

Why did we define a destructor for `Object` in the first place? It's because `char* string` is a resource whose destructor doesn't do anything relevant! In other words, the member field of `Object` did not handle its own destruction. If it did, then `Object` would not have define a destructor and we would be free to forgo all custom definitions of special member functions.

This is the quintessential feature of a well designed class hierarchy in C++. If every member in the hierarchy knows how to destroy itself, then composing classes together is trivial. An instance of an object, even if its a member field of another object, acquires a resource when it exists, and releases the resource when it ceases to exist.

Instead of `char* string`, let's use a `std::string`:

```C++
#include <string>

class Object {
  public:
    std::string string;
};
```

`std::string` happens to know how to destroy, copy, and move itself around in a leak-free and safe way. Since `Object` is composed of an object that respects RAII, then `Object` respects RAII as well, and so it can be safely destroyed, copied, and moved in the usual ways.


### std::unique_ptr

An easy way to convert a class with an unsafe resource (a resource that does not respect RAII, ie, that does not know how to destroy itself), is to wrap it inside of a `std::unique_ptr`. `std::unique_ptr` automatically calls `delete thing;` for the `thing` that it wraps. So for the above, we could have done this:

```C++
#include <memory>

class Object {
  public:
    Object() {
      string = std::unique_ptr<char*>(new char[10]);
    }

    std::unique_ptr<char*> string;
};
```

When an instance of `Object` goes out of scope, `string` gets released automatically and `Object` becomes an RAII class. The one thing you lose in this process is the ability to copy `Object`s because `std::unique_ptr` is non-copyable.


### SBRM

If a class design respects RAII, then Scope Based Resource Management becomes the primary mode of managing resources. Resources that are acquired on object initializing and released on object destruction enables code designs that leverage the stack to maintain a definitive snapshot of the resources currently in use. This means that as long as your objects don't leak outside of scopes, then you can be sure that your resources don't leak outside of those scopes either. The problem of managing resources is reduced to a trivial problem of object accounting, which is transparent and compiler-friendly.
