# Week 2 Discussion

## Announcements

**Project 2** is due Friday, **January 24,** 9:00 PM

## Questions

Ask anything!

## Incremental Development 

Is a safe way to develop while making sure our program works:

- Write a small portion of your overall program.
- compile, run, and test it.
- Rinse and repeat.

Great way to easily fix bugs as you code, rather than letting your program build off of errors you made early on.

## Logical Expressions

These are what we use inside of if statements and loops; they are expressions which resolve to a singular Boolean (true or false) value.

#### Relational / Comparison Operators:

We've seen these, and we know that they compare two values.

```C++
// <, <=, >, >=, !=, ==
// Note that they also resolve to a singular Boolean value!
```

#### Logical Operators:

These operators work between Boolean values / expressions and evaluate to a single Boolean value:

```C++
!true; // evaluates to false

true && false; // evaluates to false
    
true || false; // evaluates to true

bool alive = true;
bool twoPlusTwoEqualsFour = (2 + 2 == 4);
!alive; // false
!!alive; // true
alive && twoPlusTwoEqualsFour; // true
!alive || !twoPlusTwoEqualsFour; // false
```

We sometimes call logical expressions conditional statements.

## If Statements

If statements direct the flow of our programs by evaluating a conditional statement as either true or false, and then taking appropriate action.

Here is the general structure for an if-statement:

```C++
if (condition)
	statement;
```

This piece of code says "determine whether or not the expression in the condition is true or false, then execute the statement if it is true."

Statements can also be blocks of code, so we can fit as much of the program as we need inside of the body of our if statements.

```c++
if (condition) {
	statement;
	// ...
	statement;
}
```

We can also execute code when the condition evaluates to false:

```C++
if (condition) {
	// Executes when condition evaluates true
} else {
	// Executes when condition evaluates false
}
```

We can also chain if-statements together to form if-ladders:

```C++
if (condition1) {
	// Execute this code if 
	// the condition1 evaluated true
} else if (condition2) {
	// Execute this code if 
	// condition1 evaluated false and
	// condition2 evaluated true
} else {
	// Execute this code if
	// condition1 evaluated false and
	// condition2 evaluated false
}
```

Notice that only one of the three blocks of code will be executed per evaluation of the if's condition.

## Increment / Decrement Operators 

These are operators we use as shorthand for increasing / decreasing the value of variables by 1. We have these shortcuts because adding 1 to a variable is such a common operation.

```C++
int x = 0;
x++; // Equivalent to x = x + 1;
// The value of x is now 1

x--; // Equivalent to x = x - 1;
// The value of x is back to 0;
```

We also have this shorthand for other operations:

```C++
int x = 10;

x += 5; // Equivalent to x = x + 5;
// The value of x is now 15.

x -= 5; // Equivalent to x = x - 5;
// Now x's value is back at 10.

x /= 2; // Equivalent to x = x / 2;
// x is now 5;

x *= 5; // Equivalent to x = x * 5;
// x is now 25;

x %= 2; // Equivalent to x = x % 2
// x is now 1
```

## Loops

It's not unusual for us to want to repeat some kind of logic or operation over and over again to reach some result or conclusion. Since it would be incredibly weird and wasteful to write 1000 lines of code that look almost exactly the same, we have built in structures made specially for looping. We've got 3 kinds of loops to choose from in C++, and all work slightly different, but all can achieve the same things.

### While Loop

The while loop executes its code block over and over until its condition is false.

```C++
while (condition) {
// Code block
}
```

The while loop follows these steps:

1. **Check Condition:** If the condition is true, we execute the code block, otherwise, we break out of the loop and continue executing with whatever comes next in the program.
2. **Execute Code Block:** If the conditional was true in (1), we will execute everything in the code block, then go back to (1).

### Do-While Loop

Just like a while loop, except we always execute the code block before checking the continuation condition.

```C++
do {
// Code block
} while (condition);
```

The do-while loop follows these steps:

1. **Execute Code Block:** Execute everything in the code block, and check the continuation condition at the end.
2. **Check Condition:** If the condition is true, we execute the code block again, otherwise, we break out of the loop.

Note that the body of a do-while loop will always execute at least once.

### For Loop

Just like a while loop, except we initialize an iterator, define a condition, and define the post-iteration behavior all in the signature. The syntax is:

```C++
for (initialization; conditional; post_loop_action) {
	// Code block
}
```

The for loop follows these steps:

1. **Declare Iterator:** Typically, we declare and initialize our iterator in the first part of the for syntax. This step is performed ONLY ONCE.
2. **Check Condition:** If the condition is true, we execute the code block, otherwise, we break out of the loop.
3. **Execute Code Block:** If the condition was true in (2), we will execute everything in the code block.
4. **Perform Post Loop Action:** If we executed the code block in (3), we perform the post loop action, then go back to (2).

Remember: variables declared in the for loop declaration are only visible to the loop's code block. We can't use them outside of the context of our for loop!