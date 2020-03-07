# Week 9 Discussion

## Announcements

- **Project 7 due**: 9:00 PM Friday, **March 13th**.
- Instructor evals are out! Please fill out an evaluation for me and tell me how I can improve!

## Questions

Ask!!!

## Today's Topics

- Operator... Overload!
- But before that.... friends

## Friend functions

These are a special kind of function that, as the name implies, share a special connection with a class...

Friend functions are declared within classes, like this:

```C++
class <some_class_to_befriend> {
    friend <return_type> <friend_function>(<parameters>);
};
```

Some characteristics of friend functions:

- They are not considered methods of the class they are declared in.
- They are public no matter where you define them.
- They have the power to access private members of objects passed into them!

friendship in action:

```C++
#include <iostream>
using namespace std;

class classmate {
    // biggestDarkestSecret is a private member
	string biggestDarkestSecret = "cries himself to sleep every night.";
public:
    // this SHOULD be the only way to get the secret out...
    string tellSecret() {
        return biggestDarkestSecret;
    }
    // but everyone has that one friend who can't keep a secret...
    friend string spill(classmate a);
};

// notice how we don't have to use :: to define it...
string spill(classmate a) {
    return a.biggestDarkestSecret;
}

int main() {
	classmate Alejandro;
    cout << "Alejandro " << spill(Alejandro) << endl;
}
```

Ok finally, onto the cool stuff.

## Operator... OVERLOAD

So far you've used all kinds of operators; +, -, /, *, %, <<, ==, and a whole lot more.

Most of these make perfect sense when using them with numbers, like `2 + 2` or `103 % 2`...

But have you ever thought about _why_ we can use some of these operators for more complicated classes - like strings?

```C++
cout << "Hi" + " " + "there";
```

Turns out, you can define what happens when two objects of the same class are used with specific operators. This means, we can create custom definitions for what we think it should mean to add, subtract, or even print out objects!

We'll be overloading operators for our classes by using friend functions, which will allow us to access private members outside of function method definitions.

Inside our objects we'll need to define the operator we're overloading like this:

```C++
class your_class {
	... // business as usual, anything that normally goes in a class
    
    friend <return_type> operator<operator_to_overload>(<parameters>);
};
```

Some commonly overloaded operators, and general examples for them:

**Addition** 

```C++
class your_class {
	...
    friend your_class operator+(your_class obj1, your_class obj2);
};
```

Notice that the return type for the addition operator is of type `your_class`, and that the parameters are two `your_class` objects. That's because by overloading the `+` operator, we'll be able to take two objects of type `your_class` and add them together to form some new object of type `your_class`.  Assuming we define the operator to actually do something later, we would make use of the overloaded operator like this:

```C++
// we declare two objects of type your_class
your_class some_obj1;
your_class some_obj2;

// obj1Plusobj2 is the result of adding the two above
your_class obj1Plusobj2 = some_obj1 + some_obj2;
```

Subtraction would work pretty similarly.

**Equality**

```C++
class your_class {
	...
    friend bool operator==(your_class obj1, your_class obj2);
};
```

This time, the return type for the operator we are overloading is `bool`, because we are writing our own logic to easily check that two objects of type `your_class` are equal - the answer to which should be `true` or `false`.

```C++
your_class some_obj1;
your_class some_obj2;

bool areTheyEqual =(some_obj1 == some_obj2);
```

**Insertion (the one we use to print stuff out, <<)**

This one is a bit weirder:

```C++
class your_class {
	...
    friend ostream& operator<<(ostream& outs, your_class obj);
};
```

How it's used:

```C++
your_class some_obj;

cout << some_obj;
```

Let's break this down...

The return type for this operator is `ostream&`. `ostream` is the type of our friend, `cout`. As such, `ostream&` is a reference to an output stream. We can think of it as `cout` itself. 

The parameters are `ostream& outs` and `your_class obj`. What this means, is that to use the `<<` operator with objects of our class, we need to provide an output stream and a `your_class` object. `outs` is just the name we give this parameter so that we can use it inside the definition later.

##### A few more things about operator overloading:

You cannot overload `::` (scope resolution) and `.` (member accessor).

Don't try to overload `=`, and possibly others. Do some research before you blindly try to overload certain operators.

This is a pretty informative Stack Overflow thread if you want to learn more: https://stackoverflow.com/questions/4421706/what-are-the-basic-rules-and-idioms-for-operator-overloading.

Example from class:

```C++
#include <iostream>
using namespace std;

class star {
    float density;
public:
    string name;
    
    star() {
        name = "";
        density = 0.0;
    }
    
    star(string n, float d) {
        name = n;
        density = d;
    }
    
    friend star operator+(const star &left, const star &right) {
        float combined = left.density + right.density;
        star s3;
        if (combined >= 5000.0) {
            s3.name = "Black Hole";
        } else {
            s3.name = "Super " + left.name + "X" + right.name;
        }
        s3.density = combined;
        
        return s3;
    }
    
    friend ostream& operator<<(ostream& outs, star s) {
        outs << "star name: " << s.name << '\n';
        outs << "star density: " << s.density;
        
        return outs;
    }
};

int main() {
    star s1("Polaris", 3000.0);
    star s2("Hraesvelgr", 1000.0);
    
    star s3 = s1 + s2;
    cout << s3 << endl;
    
    cout << (s3 + s2);
}
```

