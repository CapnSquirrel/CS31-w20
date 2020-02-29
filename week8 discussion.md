# Week 8 Discussion

## Announcements

- **Project 6 due**: 9:00 PM Wednesday, **March 4th**.

## Questions

Ask!!!

## Today's Topics

- References
- Pointers

## C++ Reference type

We've used the reference type before in the parameters for functions that are meant to alter arguments in the scope from which they were called. 

```C++
#include<iostream>
using namespace std;

void oneYearCloser(int& x);

int main() {
    int Alejandros_age = 23;
    oneYearCloser(Alejandros_age);
    // one year closer to what? D:
    cout << Alejandros_age << endl;
}

// Increase an integer by one, bringing it that much closer to... ???
void oneYearCloser(int& x) {
    x++; 
}
```

In this example, the integer representing my age, `Alejandros_age`, defined in the main routine, was passed as an argument to a function which increased it by one via the use of the reference type. 

`x` is really just another name for `Alejandros_age` , so increasing the value of `x` is equivalent to increasing the value of `Alejandros_age`.

Turns out we can use the reference type outside of function parameters as well:

```C++
#include <iostream>
using namespace std;

int main() {
    string student = "Alejandro"; // Hate this guy
    string& TA = student;
   
    cout << TA << endl; // The TA is really just a student...
}
```

References let us point to the same locations in memory, but we have yet another tool that gives us more control over memory...

We can declare variables to be **pointers** of a specific type to the address of a matching value in memory. Much like the name suggests... pointers *point* to a location in memory.

## Main Memory

Remember that when we execute our programs, all of our code and variables is stored in memory.

**Main Memory** consists of a contiguous sequence of bytes (8 bits) that each have a memory address associated with them.

**Memory Addresses** are just like indices in an array; they are a sequential numbering of the memory cells.

We can imagine main memory like this:

 ![img](http://forns.lmu.build/assets/images/fall-2013/cs-31/week-8/pointers0.PNG) 

As programmers, we don't control where in memory our variables are stored, or what general location in memory our programs use; this is all decided by the compiler. 

What we can do, however, is "move around" in memory and its contents, thanks to pointers.

Let's take a look at what happens when I declare a variable:

`int x = 16;`

Do I, as the programmer, know where the compiler is going to reserve space for x in memory?

- No! The compiler will allocate space for x somewhere that is big enough to hold an int, and then place the value 16 inside. 

So imagine that on some computer, an int takes up 4 bytes of memory (WARNING: We are NOT guaranteed this!), and our compiler sticks it at memory address 1000, then in memory we have:  ![img](http://forns.lmu.build/assets/images/fall-2013/cs-31/week-8/pointers1.PNG) 

Because we know x took up 4 bytes on our system, we know that it spans addresses 1000 - 1003. 

Similarly, when we declare an array of some type, we know a couple of things about how it gets represented in memory.

Take the following initialization for example: `char c[] = "cat";`

 ![img](http://forns.lmu.build/assets/images/fall-2013/cs-31/week-8/pointers2.PNG) 

We are guaranteed a couple of things:

- We know that each cell holding a character of the cstring "cat" will take up the same amount of space on any given system.
- Each character in the cstring "cat" will be located contiguously in memory, meaning that if a `char` takes up 1 byte on our system, then above, c will take up 4 bytes of space in a row in memory (don't forget about the null terminator!).

 If our compiler places c beginning at address 1000:![img](http://forns.lmu.build/assets/images/fall-2013/cs-31/week-8/pointers2.PNG) 

Now that we have a better understanding of how memory works, let's see how we can move around in it.

## C++ Pointers

 A **pointer** is a variable that stores a reference (memory address) to another variable, or more generally, to some location in memory. 

There are some operators that will help us deal with pointers that we should know about first.

**The address-of operator (&)**: The address-of operator says "give me the memory address of this item" (the expression to the right of the ampersand). 

Specifically, in the case where the item we're requesting spans multiple bytes, the address-of operator returns the memory address of the first byte of that object. 

Earlier, we declared an integer x and initialized it to the value 16. 

![img](http://forns.lmu.build/assets/images/fall-2013/cs-31/week-8/pointers1.PNG) 

What, then, is the value of `&x`? 

- 1000, because `&x` says, "Give me the memory address of x." Here, 1000 is the memory address of the start of x, so that's what we return. 

**Pointers** store memory addresses and are assigned a type corresponding to the type of variable that they point to.

We declare and initialize pointers using the * symbol:

```C++
<type>* <name>; // Declares a pointer of the given <type> and calls it<name>
```

Here are some legal pointer declarations.

```C++
int* pointy;
bool* sharp;
char* thorny;
```

To **initialize** pointers, we give them memory addresses corresponding to locations of variables. 

```C++
// The variable being pointed at
int pointedAt = 16;

// The pointer that saves the memory location
// of pointedAt, and so now effectively, points
// at pointedAt
int* pointer = &pointedAt;
```

Now,  how can pointers access the values that they point to? 

**The dereference operator (\*)**: The dereference operator says "Give me the value of whatever's at the address my pointer is pointing to." 

```C++
  #include <iostream>
  using namespace std;
    
  int main () {
      int pointedAt = 16;
      int* pointer = &pointedAt;
      cout << *pointer << endl;
  }
```

Assuming our system is representing ints with 4 bytes and the compiler reserved memory for pointedAt at address 1000, then:

 ![img](http://forns.lmu.build/assets/images/fall-2013/cs-31/week-8/pointers3.PNG) 

To remember when to use & and when to use *, think:
**Ampersand:** Address
**Star:** Substance 

Let's look at an example of everything we've defined so far:

```C++
#include <iostream>
using namespace std;

int main () {
    int pointedAt = -100;
    int* pointy = &pointedAt;

    cout << "The value of what pointy points at: " 
        << *pointy << endl;
    cout << "The address of what pointy points at: " 
        << pointy << endl;
    cout << "The address of where pointy is stored: " 
        << &pointy << endl;
    // Will the above two be the same?
}
```

```
The value of what pointy points at: -100
The address of what pointy points at: 0x7ffc3bb6350c
The address of where pointy is stored: 0x7ffc3bb63510
```

#### Examples

```C++
#include <iostream>
using namespace std;

int main () {
    int ruhRoh;
    int* pointy = &ruhRoh;
    cout << *pointy << endl;
}
```

```C++
#include <iostream>
using namespace std;

int main () {
    int imAnInt = -100;
    int* pointy = imAnInt;
    cout << *pointy << endl;
}
```

**Note**: Reference and dereference operators could be considered inverses of one another, i.e., they "undo" each other. 

```C++
#include <iostream>
using namespace std;

int main () {
    int imAnInt = -100;
    int* pointy = &*&imAnInt;
    cout << *pointy << endl;
}
```

```c++
#include <iostream>
using namespace std;

int main () {
    int imAnInt = 2;
    int* pointy = &imAnInt;
    cout << *(&(*pointy)) << endl;
}
```

```C++
#include <iostream>
using namespace std;

int main () {
    int pointedAt = 1;
    int* pointy = &pointedAt;
    int* ditto = pointy;

    // Will these 2 be equal?
    cout << *pointy << endl;
    cout << *ditto << endl;

    // Will these 2 be equal?
    cout << pointy << endl;
    cout << ditto << endl;
}
```

```C++
#include <iostream>
using namespace std;

int main () {
    // To what is lamePointz pointing after this line?
    int* lamePointz;
    // To what address are we making this assignment?
    *lamePointz = 5;

    cout << *lamePointz << endl;
}
```

So be careful! Uninitialized pointers can lead to undefined behavior or illegal memory accesses when they haven't been assigned somewhere first. 

What can we do to be safe about uninitialized pointers?

There is a special keyword called `nullptr` that represents "the pointer that points at nothing."

The **nullptr** keyword is a pointer literal that indicates a pointer that isn't pointing anywhere. We use it for safety (to not dereference an undefined pointer) and clarity. 

WARNING: attempting to dereference a nullptr will result in undefined behavior, though many compilers are optimized to spot dereference of nullptrs and handle them appropriately. 

```C++
#include <iostream>
using namespace std;

int main () {
    // lamePointz is now safely pointing at nothing
    int* lamePointz = nullptr;

    // ...so later when we try to dereference and assign...
    *lamePointz = 5;

    // ...our code will break, but it will break reliably,
    // indicating that we have an error in the code, rather
    // than continuing blissfully unaware of our mistake
    // as we may have possibly done in the example above

    cout << *lamePointz << endl;
}
```

 Just because dereference is undefined for nullptrs, we can still check to make sure a pointer is or is not the null pointer (test for equivalence):

```C++
#include <iostream>
using namespace std;

int main () {
    int i = 50;
    int* latePointer = nullptr;

    if (latePointer == nullptr) {
        latePointer = &i;
    } else {
        cout << "Good to go sir B|" << endl;
    }
    cout << *latePointer << endl;
}
```

## Pointers and Arrays

Remember this example:

 ![img](http://forns.lmu.build/assets/images/fall-2013/cs-31/week-8/pointers4.PNG) 

Do you see that arrow?...

Yes... array identifiers were pointers all along...

The bracket notation of pointers can be thought of as "dereference with offset," meaning that, if ptr is a pointer:

`ptr[offset]` is equivalent to saying `*(ptr + offset)`, meaning "Give me the value at offset locations away from memory address ptr." 

A **pointer offset** is a number indicating how many x bytes away the next or previous value is, depending on the type of the pointer.

For example, if an int is treated as 4 bytes, and I want to access the int directly following the int at address 1000 (as in an int array starting at address 1000), then an int pointer offset of 1 tells me to look 4 bytes later, at memory address 1004. Similarly, an int pointer offset of -2 tells me to look 8 bytes before, at memory address 992, etc. 

So this is what char array c looks like in terms of pointer offsets:

 ![img](http://forns.lmu.build/assets/images/fall-2013/cs-31/week-8/pointers5.PNG) 

A couple things to note above:

- Notice how c is an array of chars, and so c is a char pointer, meaning that its offsets assume the next char is 1 byte away.
- The bracket c[i] notation is equivalent to the offset *(c + i) notation.
- Although we could generate a pointer to c[4] by saying &c[4], it would be unsafe to assign anything to c[4] (overflow).

Takeaway message: Every time I use an offset with a pointer, it automatically scales to how large its type is in memory.

We can modify a pointer's memory address through increment and decrement whenever that pointer is modifiable, for example:
`char* ptr = c; ptr++; // ptr now points to the address one char-length (1 byte) after c` 

Does this compile?

```C++
#include <iostream>
#include <cstring>
using namespace std;

int main () {
    char c[] = "cat";
    int len = strlen(c);

    for (int i = 0; i < len; i++) {
        cout << c[0] << endl;
        c++; // hey! that's this language!
    }
}
```

So how can we make that work?

```C++
#include <iostream>
#include <cstring>
using namespace std;

int main () {
    char c[] = "cat";
    char* helper = c;
    int len = strlen(c);

    for (int i = 0; i < len; i++) {
        cout << helper[0] << endl;
        helper++;
    }
}
```

Will this work?

```C++
#include <iostream>
using namespace std;

int main () {
    char c[] = "cat";

    for (char* helper = c; *helper != '\0'; helper++) {
        cout << helper[0] << endl;
    }
}
```

 Will the following code compile? If so, what will it print? 

```C++
#include <iostream>
using namespace std;

int main () {
    char c[] = "cat";

    for (char* helper = c; *helper != '\0'; helper++) {
        // Think about what happens when we just print out c.
        cout << helper << endl;
    }
}
```

Will the following code compile? If so, what will it print?

```C++
#include <iostream>
using namespace std;

int main () {
    char c[] = "cat";

    // Notice our new iterator increment...
    for (char* helper = c; *helper != '\0'; helper = &helper[1]) {
        cout << helper[0] << endl;
    }
}
```

Will this compile? Is there undefined behavior?

```C++
#include <iostream>
using namespace std;

int main () {
    int pointedAt = 1;
    int* pointy = &pointedAt;

    *pointy++;

    cout << *pointy << endl;
    cout << pointedAt << endl;
}
```

**Pointer Comparisons**
Pointers can always be compared for equivalence (do they point to the same address), but we must be careful... we still don't know where variables will be placed in memory at runtime. 

Pointers *within* the same array are always guaranteed to be comparable using <, >, etc. because array elements are located contiguously in memory. 

```C++
#include <iostream>
#include <cstring>
using namespace std;

int main () {
    char c[] = "catdog";
    char* cPtr = c;
    int len = strlen(c);

    for (int i = 0; i < len; i++) {
        if (cPtr < &c[3]) {
            cout << *cPtr << endl;
            cPtr++;
        }
    }
}
```

When two pointers are operating in the same array, we can even subtract them to learn their *offsets,* not their difference in bytes.

```C++
#include <iostream>
using namespace std;

int main () {
    int i[] = {1, 2, 3, 4};
    int* p1 = i;
    int* p2 = (i + 2);

    // [?] What gets printed below?
    cout << (p1 - p2) << endl;
    cout << (p2 - p1) << endl;
}
```

```C++
#include <iostream>
#include <cstring>
using namespace std;

int main () {
    int i[] = {5, 6, 7, 8, 9};
    int* iPtr = i;
    int len = 3;

    for (int j = 0; j < len; j++) {
        if (iPtr < &i[3]) {
            // What will print here?
            cout << (&i[3] - iPtr) << endl;
            iPtr++;
        }
    }
}
```

### Pointers and Functions

We can accept pointer parameters using a variety of notations:

```
  void pointerFunc (int* i, int p[]) {
      ...
  }
```

Above, both i and p refer to int pointer parameters.

What does this code print out?

```C++
#include <iostream>
#include <cstring>
using namespace std;

int* findInt (int arr[], int len, int match) {
    for (int i = 0; i < len; i++) {
        if (arr[i] == match) {
            return &arr[i];
        }
    }
    // Case where there is no match!
    return nullptr;
};

int main () {
    int i[] = {5, 6, 7, 8, 9};
    int* ptr = findInt(i, 5, 7);

    if (ptr != nullptr) {
        cout << *ptr << endl;
    } else {
        cout << "null!" << endl;
    }
}
```

We could have also replaced the array parameter with a pointer:

```C++
#include <iostream>
#include <cstring>
using namespace std;

// See the difference?
int* findInt (int* arr, int len, int match) {
    for (int i = 0; i < len; i++) {
        if (arr[i] == match) {
            return &arr[i];
        }
    }
    // Case where there is no match!
    return nullptr;
};

int main () {
    int i[] = {5, 6, 7, 8, 9};
    int* ptr = findInt(i, 5, 7);

    if (ptr != nullptr) {
        cout << *ptr << endl;
    } else {
        cout << "null!" << endl;
    }
}
```

Perhaps the most important thing to know about pointer parameters:

Pointer parameters are **passed by value,** meaning a copy of the address is made and is named by the parameter. The values that pointer can dereference and change are NOT copied.

### Side-Notes about Functions

If we wanted to set the ptr in main to what was changed in findInt, we'd just make sure to pass by reference via: `int* &ptr`

One might ask, then, what the difference is between pointers and references, and the answer is a subtle one (great discussion found [here](http://stackoverflow.com/questions/57483/what-are-the-differences-between-pointer-variable-and-reference-variable-in-c)):

- A pointer can be reassigned any number of times while a reference can not be after initialization.
- A pointer can be set to the nullptr while references can never point to null (they are just names for other variables).
- You can not generate the address of a reference like you can with pointers.
- There are no "reference arithmetics" like we do with pointers (e.g., *(ptr + 1) or &ptr + 4)