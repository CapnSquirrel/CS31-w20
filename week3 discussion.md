# Week 3 Discussion

## Announcements

- **Project 2 DUE TODAY AT 9 !!!!**
- **Project 3 due**: 9:00 PM Tuesday, **February 4th**

## Questions

Anything except maybe about project 3... (you have more to cover in lecture).

## Today's Topics

- Switch statements
- Functions

## Switch Statements

Switch statements provide us with yet another control structure to dictate the flow of our programs.

They are particularly useful for cases in which we have custom behavior for a variable that could be many different things.

Here's their syntax in C++:

```C++
switch (case) {
	case value1: ...
	case value2: ...
	case value3: ...
	...
	case valueN: ...
	default: ...
}
```

`case` and the values we check it against are only allowed to be of "integral" type. For us, this means we either use **int** or **char** for switch statements.

Here's the example I tried to bring up last week:

```C++
#include <iostream>
using namespace std;

int main() {
    char d;
    cout << "Input a day of the week: ";
    cin >> d;
    cout << endl;
    
    switch(d) {
        case 'M': 
            cout << "Monday";
            break;
        case 'T':
            cout << "Tuesday";
            break;
        case 'W':
            cout << "Wednesday";
            break;
        case 'R':
            cout << "Thursday";
            break;
        case 'F': 
            cout << "Friay";
            break;
        default:
            cout << "Didn't recognize your input, sorry";
    }
}
```

The `break` after every each case is optional, but if we don't use it, we will execute multiple cases in succession. 

```C++
#include <iostream>
using namespace std;

int main() {
    char d;
    cout << "Input a day of the week: ";
    cin >> d;
    cout << endl;
    
    switch(d) {
        case 'M': 
            cout << "Monday";
            break;
        case 'T':
            cout << "Tuesday";
        case 'W':
            cout << "Wednesday";
        case 'R':
            cout << "Thursday";
        case 'F': 
            cout << "Friay";
        default:
            cout << "Didn't recognize your input, sorry";
    }
}
```

## Functions

In mathematics, we can define operations like `f(x) = x + x^2` and easily refer to this function as `f` to save ourselves from saying/writing `x + x^2` every time.

Similarly, functions in programming languages allow us to define operations we intend on repeating throughout our code.

A function uses its definition to describe the relationship between its inputs and how they map to outputs.

A function definition in C++ has the following syntax:

```C++
// The first line below is commonly called a "function signature"
return_type function_name (p1_type p1_name, p2_type p2_name, ...) {
      // function body
}
```

**Parameters** are the ZERO or MORE inputs to your functions. Each has its type defined, and a placeholder name assigned to it. 

**Return type** is the type of value I expect to receive back from a function (e.g. ints, doubles, strings, etc.) 

#### Example

So, how would I convert my simple mathematical function `f(x) = x + x^2` into a C++ function? (assume x is a double) 

```C++
double f (double x) {
	return x + x * x;
}
```

Later, if I wanted to use this function, then I would just say `f(5.3)` or even pass in a variable that's a double like `f(someDoubleVar)`. We call this... "calling" the function. 

**Arguments** are what you call the values you pass into a function when you're **calling** it. 

Calling a function means that you've passed in some inputs (or none, if none are expected) and are expecting it to execute, possibly returning some result.

Note that we call the inputs "arguments" when we call a function, and "parameters" when we are designing its signature.

**Parameters** are what you call the placeholder values passed into a function when you're **defining** it.

## Functions in C++

Here's a program that implements my math function. Will it compile? What will it print?

```C++
#include <iostream>
#include <string>
using namespace std;

int main () {
    double x = f(5.0);
    
    cout << x << endl;
}

double f (double x) {
    return x + x * x;
}
```

So how do we get around that? I want to put my main function first because it makes sense for the first thing I run to be up towards the top.

**Function Prototypes** are hints to the compiler that say, "Hey, here's a function with a name, a return type, and some parameters... I'm not going to define it now, but if you use it, I promise I'll have it defined later!"

They consist of a function signature without the function body. 

```C++
#include <iostream>
#include <string>
using namespace std;

// Function prototype for f
double f (double x);

int main () {
    double x = f(5.0);
    double y = f(3.6);
    
    cout << x << endl;
    cout << y << endl;
}

double f (double x) {
    return x + x * x;
}
```

After a function has returned, the code will resume execution from wherever that function was called.

This means that functions can even call one another!

Finally, if we have a function that is not returning anything (can still have 0 or more parameters), we can specify a return type of void.

The **void** return type asserts that the function will not return anything. (though you can still stop the execution of a void function with `return;`)

Lastly, if I promise my compiler that my function is going to return something of a certain type, I better deliver... or else... 

 ```C++
#include <iostream>
#include <string>
using namespace std;
    
string deliverGoods (string s);
    
int main () {
    cout << deliverGoods("My goods, please.") << endl;
}

string deliverGoods (string s) {
    if (s == "Got the goods?") {
        return "Yeah I got the goods.";
    }
    // Our function is not guaranteed to return something upon being called...
    // So Mr. X will break our legs... (We get a compilation error)
}
 ```
