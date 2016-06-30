---
layout: single
title:  "Modern C++ in Embedded Systems"
header:
  teaser: "unsplash-gallery-image-2-th.jpg"
  <!--teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"-->
tags:
  - tags
  - project_notebook
date:   2014-11-22
published: false
---

# Modern C++ in Embedded Systems

## Sources

I wanted to start off the day with a literature review about how well C++ and
embdedded systems work together. I found an excellent article:
[Modern C++ in embedded systems – Part 1: Myth and Reality][1] by Dominic Herity.
There is also a part 2: [Modern C++ embedded systems – Part 2: Evaluating C++][4].

I also wanted to better understand globals. Often I find my self desiring a global
variable but I am unsure how to handle it. I will also look at: 
[A pox on globals][2] Jack Ganssle.


## Learned
### #warnings
From [Effective C Tip #9 – use #warning][3] by Nigel Jones.
```Cpp
#warning This is a warning
```
This will result in the compiler issuing a warning with the text
‘This is a warning’ printed to stderr. Please note that, just as for #error,
Useful for:
- Protecting Incomplete Code
- Commenting Out Code

### References (a safer pointer)

With C++11 it may be better to investigate unique\_ptr and shared\_ptr before
considering references, but it is still good to know

[References][1]
> References are safer than pointers because they can’t be null, they can’t be 
> uninitialized, and they can’t be changed to point to something else. The closest 
> thing to a reference in C is a const pointer.

### Inline functions are often faster

Need more research into it to quantify the amount of bytes.

[Inline functions][1]
> To estimate the code size impact of an inline function, estimate how many bytes
> of code it takes to implement it and compare that to the number of bytes needed
> to do the corresponding function call.

### Inheritance Physical, not just conceptual

[Inheritance][1]
> What we don’t usually learn in OOD is that the ‘is a’ relationship in C++ has 
> a physical as well as a conceptual basis. In C++, an object of derived class B 
> is made up of an object of base class A, with the member data of B tacked on at 
> the end. The result is the same as if the B contains an A as its first member. 
> So any pointer to a B is also a pointer to an A. Any member functions of class 
> A called on an object of class B will work properly. When an object of class B 
> is constructed, the class A constructor is called before the class B constructor 
> and the reverse happens with destructors.

### Virtual Functions cost at runtime
Impacts
- Slightly slower (vtable lookup to call function)
- Increased size of class (must have vtable)
- Increased size of generated code (they are always linked)

[Virtual Functions][1]
> A class with at least one virtual function is referred to as a ‘polymorphic’
> class. The distinction between a polymorphic and a non-polymorphic class is 
> significant because they have different trade-offs in runtime cost and 
> functionality. 

> Virtual functions are implemented using an array of function pointers, called 
> a vtable, for each class that has virtual functions.

### Templates allow the flexible methods and classes
Benefits
- Standard C++ Library to be used with any types
- Write a single function or class to be able to use many types.

The author provided a very simple and easy to understand use of templates.

[Templates][1]
> Sample template class

```Cpp
template<typename T> class A {
 private:
     T value;
 public:
     A(T);
     T f();
 };

 template<typename T> A<T>::A(T initial) {
     value = initial;
 }

 template<typename T> T A<T>::f() {
     return value;
 }

 int main() {
     A<int> a(1);
     a.f();
     return 0;
 }
```

### What is Heap Fragmentation

Solutions
- Allocate heap space in fixed size buffers that exist in a pool of available buffers
    - More memory.
    - Need to estimate what size buffers are in the pool.
- Only use in initializtion.
    - This removed the chance that it will heap fragment after years of running

[C++ and the heap][4]
> The main reason for banning heap usage in an embedded application is the threat
> of heap fragmentation. As the software runs, memory allocations of different
> sizes are acquired and released. The situation can arise where many small
> allocations are scattered through the heap and, although a large fraction of
> the heap may be available for use, it is all in small fragments and it is not
> possible to provide an allocation bigger that the largest fragment.

### Storing data in ROM

Don't use **static** in classes. It is forced to occupied RAM. Instead use **const**.

#### static in C 
[ROMable objects][4]
> Linkers for embedded systems allow const static data to be kept in ROM. For a 
> system written in C, this means that all the non-varying data known at compile
> time can be specified by static initializers, compiled to be stored in ROM and 
> left there.

#### Static data for C++ classes

Static data can be stored in a nested private class that is able to be statically
initialized. The author provides a great great example
[A clean C++ ROMable dictionary](#L23).

First he defined a "ROMable class".
[ROMable objects][4]
> The criteria for a static initializer to be allowed for a class are:
> - The class must have no base classes.
> - It must have no constructor.
> - It must have no virtual functions.
> - It must have no private or protected members.
> - Any classes it contains must obey the same rules.

<a id="L23" /> [A clean C++ ROMable dictionary][4]
> Now, let’s do it right. In Listing 23, the class Dict in Listing 22 becomes 
> Table, which is nested privately within the new class Dict. Class Dict also 
> contains, as a static member, an array of Tables, which we can initialize 
> statically. The function main() shows use of this class Dict, which has a clean 
> interface.

```Cpp
#include <iostream>
using namespace std;
class Dict {
public:
    typedef enum {
        german,
        french
    } Language;
    
    Dict(Language lang);
    
    const char* fromEnglish(const char* english) const;
    
    const char* toEnglish(const char* foreign) const;

private:
    class Table {
    public:
        const char* fromEnglish(const char* english) const;
        const char* toEnglish(const char* foreign) const;
        
        enum { DictSize = 3 };
        
        struct {
            const char* english;
            const char* foreign;
        } table[DictSize];
    };

    const static Table tables[];

    Language myLanguage;
};

const Dict::Table Dict::tables[]= {
    {
        {
            {"yes", "ja"},
            {"no", "nein"},
            {NULL, NULL}
        }
    },
    {
        {
            {"yes", "oui"},
            {"no", "non"},
            {NULL, NULL}
        }
    }
};

// ...

int main() {
    Dict germanDict (Dict::german);
    cout << germanDict.fromEnglish("yes");
    return 0;
}
```

### lambdas in C++11

The author provides a good example of a lambda used in C++11
[More about lambdas][4]
```Cpp
#include <algorithm>

int count_less_than(int const* first, int const* last, int value) {
    auto ltt = [value](int x) { return x < value; };    
    return std::count_if(first, last, ltt);
}
```

### Be aware of the Assembly generated by your code.

Check how your compiler will handle different implmentations. Sometimes the
winner is not obvious.
Compiler Explorer shows assembly for common compilers. [http://gcc.godbolt.org/#][5]


## Future Reads

[The C++14 Standard: What You Need to Know][6] by Mark Nelson


[1]: http://www.embedded.com/design/programming-languages-and-tools/4438660/2/Modern-C--in-embedded-systems---Part-1--Myth-and-Reality
[2]: http://www.embedded.com/electronics-blogs/break-points/4025723/A-pox-on-globals
[3]: http://embeddedgurus.com/stack-overflow/2011/09/effective-c-tip-9-%E2%80%93-use-warning/
[4]: http://www.embedded.com/design/programming-languages-and-tools/4438679/Modern-C--embedded-systems---Part-2--Evaluating-C--?isCmsPreview=true
[5]: http://gcc.godbolt.org/#
[6]: http://www.drdobbs.com/cpp/the-c14-standard-what-you-need-to-know/240169034
