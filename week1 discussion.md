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

  - We do not want to upset Mr. X:

  ![Image result for mr x](https://vignette.wikia.nocookie.net/residentevil/images/8/80/Tyrant_%28Remake%29_4.png/revision/latest?cb=20190210203921)

In this class you are free to use whatever code editor you like as long as your code compiles correctly in Visual Studio C++ and g31

- Project instructions will likely refer heavily to Visual Studio 2019 functionality. 

## Let's Write Something Together !!!!!

### Writing C++

What kind of programmers are we if we don't start with a Hello World program !!!!!

#### Hello.cpp

This program sends the string "Hello World !!!!!" to standard output: the terminal.

```c++
#include <iostream>
using namespace std;

int main() {
  cout << "Hello World !" << endl;
}
```

We'll compile and run this inside of Visual Studio and then send it over to the Linux server to compile and run with g31.

I interact with my remote Linux machine entirely through the terminal, because it's faster for me and doesn't require me to download / set up a bunch of stuff...

Come to my office hours if you want help setting this process up for yourself, with whatever method you prefer.  

## Errors

C++ is a powerful language designed to give developers a large degree of control over their programs. 

One of the main ways it does this is by allowing us access to pointers (Covered later in the course).

We have direct access to memory! (scary)

This makes C++ dangerous in the hands of a careless developer, and confusing for newcomers. 

### Syntax / Compilation errors

Violations of the language's syntax. Much like English, Spanish, Japanese; languages have grammatical rules to which we must abide. 

These prevent the code from compiling.

Are usually easy to spot and fix.

What are some common syntax errors?
- Missing semicolons at ends of statements.
- Missing brackets around blocks.
- Missing namespace or #include definitions
- Misspelled variables or names

### Runtime / Logic Errors

Errors in the code that may allow it to compile successfully but will cause the program to break while running or produce unexpected (wrong) results. 

What are some common runtime errors?
- Division by 0.
- overflow (e.g. trying to hold a really big number in an int variable that exceeds its bounds).

## Practice

Pair up; pair programming is proven to increase productivity/learning if both parties involved are contributing!

Tests will require you to write code by hand!!! So better start practicing now!

You can pull up this worksheet on the CCLE site, under Week 1.