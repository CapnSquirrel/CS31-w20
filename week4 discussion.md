# Week 4 Discussion

## Announcements

- **Project 3 due**: 9:00 PM Tuesday, **February 4th**
- I put my notes online! Visit https://github.com/CapnSquirrel/CS31-w20 to read them.
- Professor Howard put up some "Helpful Code" for Project 3, check it out under CCLE week 4.
- Midterm is **NEXT WEEK**!!!! (On Wednesday)

## Questions

Ask away!

## Today's Topics

- References
- Arrays

## Notes From Grading

- If you got a 0 or a Very Low Grade because your program did not pass the grading script's tests, it may be because of how you formatted your output.
- I was lenient in grading your commenting style, but next time I will raise my expectations!
- Test cases should be a list of actual inputs you can give your program, not an explanation or paragraph.

## References

In C++, **references** are aliases for an object, in that they can be treated as actually being the object they're referencing. They are simply different names for the same object in memory. The syntax for a reference is:
`<type>& <name>`  

Example: Creating a variable of type double creates a brand new double; creating a variable of type double& is just having that variable be another name for an already existing double. 

```C++
#include <iostream>
using namespace std;

int main () {
    double trouble = 123.45;
    double& issue = 12.5;

    cout << "issue is: " << issue << endl;

    // Now, change issue...
    issue = 0.0;
    // ...and see what happens to trouble
    cout << "trouble is: " << trouble << endl;
    cout << "issue is: " << issue << endl;
}
```

Output:

```
$ issue is: 123.45
$ trouble is: 0
$ issue is: 0
```

So, when I change issue above, trouble changes also... which isn't surprising because issue is just another name for trouble.

So why have references? Well, one illustration is how function parameters work in C++.

By default, function parameters are taken in what is known as "pass by value."

**Pass by value** means that parameter values are **copied** whenever a function is called, and do not refer to the exact argument variables:

```C++
#include <iostream>
using namespace std;

double f (double x);

int main () {
    double x = 5.0,
    result = f(x);

    cout << result << endl;
    cout << x << endl;
}

// Receiving x by value
double f (double x) {
    x = x + x * x;
    return x;
}
```

Notice how I passed in x (by value) in main, but x in main did not get modified after I called f(x). 

**Pass by reference** means that parameters are references (aliases, nicknames, etc.) to the arguments; they refer to the same variable in memory.:

```C++
  #include <iostream>
  using namespace std;
  
  // Note, I also had to put
  // the & after double in the
  // function prototype
  double f (double& x);
  
  int main () {
      double x = 5.0,
             result = f(x);
      
      cout << result << endl;
      cout << x << endl;
  }
  
  // Receiving x by reference
  // Note the & after double
  double f (double& y) { // y is an alias for x
      y = y + y * y;
      return y;
  }
```

Output:

```
$ 30
$ 30
```

Because y is a reference parameter, changing y's value within f changes the value of x in main. So not only is result set to the result of 5 + 5\*5, x's value is updated to be the result of 5 + 5\*5. 

## Arrays

An **array** in C++ is a list of a particular type of variable. It has a specific amount of space assigned to it, and indexes its items starting at 0. 

### Declaring Arrays

We declare arrays using the following syntax: 

```C++
  <type> <name>[size];
```

#### Examples

 Will the following programs compile? If so, what will they output?

```C++
  #include <iostream>
  using namespace std;
  
  int main () {
      int i[3];
      cout << i[1] << endl; // undefined garbage!
  }
```

```C++
  #include <iostream>
  using namespace std;
  
  int main () {
      string yarn[2];
      cout << yarn[1] << endl; // default string value, ""
  }
```

```C++
  #include <iostream>
  using namespace std;
  
  int main () {
      int i[];
      // Please compiler, I promise I'll
      // deal with i later ;_;
  }
```

#### Takeaways

-  Array sizes must be known at compile time.
-  Arrays of built-in types, if not initialized, will have indeterminate values in their storage.
-  Arrays of strings, if not initialized, will have empty strings in their storage.

### Initializing Arrays

 We can initialize arrays using the following syntax:

```C++
  <type> <name>[<size (optional)>] = {<value0>, <value1>, ...};
```

As long as you initialize an array at declaration, you do not need to specify the size; it will match the dimension of the initialization.

#### Examples

 Will the following programs compile? If so, what will they output?

```C++
  #include <iostream>
  using namespace std;
  
  int main () {
      int i[3] = {1, 2, 3};
    
      cout << i[0] << endl; // 1
      cout << i[1] << endl; // 2
      cout << i[2] << endl; // 3
  }
```

```C++
  #include <iostream>
  using namespace std;
  
  int main () {
      int i[4] = {5, 5, 5};
    
      cout << i[0] << endl;
      cout << i[1] << endl;
      cout << i[2] << endl;
      // What's in here? :O
      cout << i[3] << endl; // This defaults to 0!
  } 
```

```C++
  #include <iostream>
  using namespace std;
  
  int main () {
      int i[2] = {1, 2, 3}; // Too many initializers! 
    
      cout << i[0] << endl;
      cout << i[1] << endl;
  }
```

```C++
  #include <iostream>
  using namespace std;
  
  int main () {
      int i[3];
      i = {104, 101, 121};
    
      cout << i[0] << endl;
      cout << i[1] << endl;
      cout << i[2] << endl;
  }
```

Weird right? It looks like it should work, but the initialization syntax is special at declaration. 

## Arrays in Functions

There's one BIG difference between how C++ functions handle array arguments versus single variables.

How do C++ functions pass arguments by default for basic types like int, double, and string?

- **Pass by Value:** meaning the function parameters are **copies** of the arguments.

How do C++ functions pass arguments for arrays like int[], double[], and string[]?

- Think of it as **passing by reference (though it's not):** meaning the function parameters are **pointers** to the argument; they refer to the same array in memory. (more on pointers later)

The array that we have access to inside of a function is the same as the one we pass in as argument to that function; making changes to one will affect the other!

 ![img](http://forns.lmu.build/assets/images/fall-2013/cs-31/week-5/arrays4.PNG) 

So knowing this, how can I make functions that don't touch the original array?

Well, if I want to be **extra** safe, then I can declare parameters constant, even though what I pass in is not a constant: `void functionInvolvingAnArray(const int a[]);`