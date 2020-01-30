# Week 1 Discussion

## Self Introductions

### About Alejandro: 

- Go by Alejandro, He/Him/His
- Born in Colombia, raised in California
- 1st year CS M.S. student

- Computer Science B.S. from LMU
- Minors in Animation and Pure Mathematics
- Anime & video games
- Arts & crafts

### My Role as your TA

Feel free to email me at any time of the day; my email is azapataa@g.ucla.edu

Come to my office hours! Tuesday from 1:30pm - 3:30pm in Boelter 3256S. You can see any of the other TAs for office hours if mine don't fit your schedule, and if none of their hours work, email me to set up an appointment. 

### Elena's Introduction

This is my first time meeting you too!

### Class Introductions

Tell everyone: 
- What you would like to go by
- where you grew up
- hobbies

### How Discussion Works

We'll review and practice topics covered in class.

Ask anything you didn't get to ask in lecture! This is absolutely the place for any clarification.

## Important Dates

- **Project 1**: Due **Tuesday**, January 14, 9:00 PM
- **Project 2**: Due **Friday**, January 24, 9:00 PM PST

## Questions

AMA, we have time.

## Development Environment

A development environment is made up of the tools you'll be using for developing software. Usually, you only need:
- Code editor: popular ones are VS Code, Atom, Sublime. 

  - Some people even use the terminal, or notepad. I personally don't know how they live the way they do, but power to them.

- Compiler (compiles your code and makes it an executable. I like to think the compiler looks like Mr. X from Resident Evil).

  - We do not want to upset Mr. X...

  ![Image result for mr x](https://vignette.wikia.nocookie.net/residentevil/images/8/80/Tyrant_%28Remake%29_4.png/revision/latest?cb=20190210203921)

In this class you are free to use whatever code editor you like as long as your code compiles correctly in Visual Studio C++ and g31

- Project instructions will likely refer heavily to Visual Studio 2019 (for Windows) and Xcode (for Mac) functionality, so you might as well just stick to those for this course. 

#### About g31

Compilers are just programs that read your source code, check that it is valid according to the language's rules, and make an executable out of it. As such, there is more than one compiler out there for any given language. **g31** is just another compiler that checks for a few more things than the Visual Studio 2019 compiler does. It was made just for CS 31 at UCLA :). 

## Let's Write a C++ Program!

What kind of programmers are we if we don't start with a Hello World program!

It's always a good idea to name things appropriately, so we'll name our file "hello.cpp"

This program sends the string "Hello World!" to standard output: the terminal.

```c++
#include <iostream>
using namespace std;

int main() {
  cout << "Hello World!\n";
}
```

Let's take a look at what this program does, part by part:

`#include <iostream>` lets our compiler know that we'll be using code that has been defined within the "iostream" library. "io" is short for "input/output," so the code that comes from this library is primarily concerned with taking in user input and outputting to some destination.

`using namespace std;` The proper way to use functions like `cout` and `cin` is to prefix them with `std::`... Including this line at the top of our program lets us omit that prefix (most of the time). So instead of writing out `std::cout`, we can just use `cout` and `endl` (amongst other things).

We'll probably include those first two lines at the top of every program in this class!

```C++
int main() {
	...
}
```

This is what we call the main method. Every program that we want to be able to execute needs one; it is the "main" point of execution, a.k.a. where the computer will begin executing code when we run a program. As such, we'll want to place some code inside of it!

`cout << "Hello World!\n";` is the line printing the string "Hello World!" to the terminal. 

`cout` is what we call the "output stream" and will output (print) whatever we feed into it to the standard output (the terminal). 

The `<<` is how we supply cout with input. It sort of looks like a funnel, with everything to the right of it being what gets funneled into cout. We can chain strings, expressions, and variables together using `<<`. 

`'\n'` is the character representation for "new line" and having it at the end of our string will ensure that whatever is printed next will be on the next line.

We could have equivalently written the following lines and have achieved the same effect:

```C++
cout << "Hello World!\n"; // method 1
cout << "Hello World!" << '\n'; // method 2
// method 3:
cout << "Hello World!";
cout << '\n';
```

We'll compile and run our Hello World program inside of Visual Studio and then send it over to the Linux server to compile with g31.

Here's a little cheat-sheet of commands for compiling code with g31:

```
// logs you into your seasnet machine
ssh <username>@lnxsrv07.seas.ucla.edu

// compiles <somefile>.cpp and creates an executable named <desiredExeName>
g31 <somefile>.cpp -o <desiredExeName>	
```

Come to my office hours if you want help setting this process up for yourself, with whatever method you prefer. 

## Errors

C++ is a powerful language designed to give developers a large degree of control over their programs. 

One of the main ways it does this is by allowing us access to pointers (Covered later in the course).

This makes C++ dangerous in the hands of a careless developer, and confusing for newcomers. 

### Syntax / Compilation errors

These are violations of the language's syntax. Much like English, Spanish, Japanese; languages have grammatical rules by which we must abide. 

These prevent the code from compiling, but are usually easy to spot and fix.

What are some common syntax errors?
- Missing semicolons at ends of statements.
- Missing brackets around blocks.
- Missing namespace or #include definitions
- Misspelled variables or names

### Runtime / Logic Errors

These are errors in the code that may allow it to compile successfully but will cause the program to break while running or produce unexpected (wrong) results. 

What are some common runtime errors?
- Division by 0.
- overflow (e.g. trying to hold a really big number in an int variable that exceeds its bounds).
- Flawed mathematical calculation (makes the program yield wrong results).

## Practice

Pair up; pair programming is proven to increase productivity/learning if both parties involved are contributing!

Tests will require you to write code by hand!!! So better start practicing now!

You can pull up this worksheet on the CCLE site, under Week 1.