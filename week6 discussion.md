# Week 6 Discussion

## Announcements

- **Project 4 due**: 9:00 PM Wednesday, **February 19th**.
- I put my notes online! Visit https://github.com/CapnSquirrel/CS31-w20 to read them.

## Questions

???

## Today's Topics

- Enums
- Structs

## Enums

Enum is short for Enumeration :O

With enums, we can declare our own "types". Albeit, these "types" are rather limited, since they will only be able to store a list of values. 

The syntax for declaring an enum is as follows:

```C++
enum <enum_name> {enum_value1, enum_value2, ...};
```

Here, we use the `enum` keyword to signify that we are defining an enum.

`<enum_name>` is the name we want to give to our new "type".

And the comma-separated entries inside of the curly braces are our values.

Here's a more concrete example:

```C++
enum FinalFantasyJob {
	PALADIN, 
	WARRIOR, 
	DARK_KNIGHT, 
	GUNBREAKER, 
	WHITE_MAGE, 
	SCHOLAR, 
	ASTROLOGIAN,
	MONK,
	DRAGOON,
	NINJA,
	SAMURAI,
	BARD,
	MACHINIST,
	DANCER,
	BLACK_MAGE,
	SUMMONER,
	RED_MAGE,
};
```

By convention, the enum name (the type we're defining) should be title-cased, and values inside should be in all capital letters.

An enum's values are of integral type (just like switch statements!), so we can only assign them ints and chars. If we don't define what their integer values are, they are default to be 0, 1, 2, ... 

In the example above, `PALADIN` is assigned 0, `WARRIOR` is assigned 1, ..., and `RED_MAGE` is assigned 16.

Here's what it's like if we assign our own:

```C++
enum UselessExample {
    HI, 		// 0
    THERE=3,	// 3
    HEHE, 		// 4
    UM, 		// 5
    YA=78, 		// 78
    A, 			// 79
    HAHA		// 80
};
```

Values we don't explicitly define get assigned the value of the previous entry + 1.

### Ok... How do we use enums?

Just like any type!

```C++
#include <iostream>
using namespace std;

// some kind of really basic drawing program:

enum Color {RED, GREEN, BLUE};

// pretend this function is defined elsewhere
void drawCircle(Color color, float radius, int x, int y);

int main() {

    Color paintColor = Red; // paintColor is a variable of type Color
    float radius = 5.0;
    int x = 0;
    int y = 2;
    
    drawCircle(paintColor, radius, x, y);
}
```

## Structs

Structs allow us to create more complex custom types than those which enums allow us. 

For those of you who have programmed before, structs are the C++ equivalent of objects!

With structs, we can create groupings of variables and functions into single, cohesive objects. 

We can call the variables grouped within structs by many names... (fields, attributes, members, etc.)

In general, these **Data Members** are variable components of a given struct; they can be of any variable type. 

When we define a struct, we've basically defined a new type for our program to use, and all of the rules we're familiar with (when dealing with types) now similarly apply. 

Here's the syntax to declare a struct:

```C++
struct <structName> {
	<member1_type> <member1_name>;
	<member2_type> <member2_name>;
	// ...etc.
}; // Remember the semicolon!
```

Let's create a struct that represents.... Animal Crossing villagers :D

```C++
struct Villager {
	string name;
	string species;
	string personality;
	int age;
    string likes[3];
};
```

Ok, great! We have a `Villager` type that defines villagers as having a name, species, personality, age, and three things that they like.

Let's make an instance of `Villager`.... I share a birthday with Pudge so I'll make him.

A struct **instance / object** is a particular object of a particular type of struct. 

```C++
#include <iostream>
using namespace std;

struct Villager {
	string name;
	string species;
	string personality;
	int age;
    string likes[3];
};

int main() {
	Villager pudge; // pudge is an instance of struct Villager
}
```

This is what he looks like, if you're curious.

<img src="https://nookipedia.com/w/images/thumb/d/dc/Pudge_NLa.png/175px-Pudge_NLa.png" alt="See the source image"  />

As it stands, our `Villager` named `pudge` is only a `Villager` by variable name... we have not assigned him any values. 

Upon creation of a struct instance, the data members in the struct are set to their default values as defined by their type. 

Let's take a look at how we can fill in Pudge's information.

The **Element / Member Selection Operator (.)** says, "Give me member `<dataMember>` of the object in my `<objectInstance>`..." That is, we select the member on the right of the period from the struct instance on the left.

```
objectInstance.dataMember
```

So if we want to set Pudge's attributes:

```C++
#include <iostream>
using namespace std;

struct Villager {
	string name;
	string species;
	string personality;
	int age;
    string likes[3];
};

int main() {
	Villager pudge;
    
    pudge.name = "Pudge";
    pudge.species = "Cub";
    pudge.personality = "lazy";
    pudge.age = 23; // I don't know how old Pudge is so... I'll use my age
   
    pudge.likes[0] = "blowing bubbles";
    pudge.likes[1] = "coffee";
    pudge.likes[2] = "Final Fantasy 14"; // wow pudge, me too!
    
    cout << pudge.name << endl;
    cout << pudge.species << endl;
    cout << pudge.personality << endl;
    cout << pudge.age << endl;
}
```

## Structs and Arrays 

In the Animal Crossing games, you play as the mayor of a town, and a bunch of villagers come live in your town if you're a good mayor. 

So if a town is sort of like a collection of villagers... we can try modeling a town as an array of villagers!

```C++
#include <iostream>
using namespace std;

struct Villager {
	string name;
	string species;
	string personality;
	int age;
    string likes[3];
};

int main() {
	Villager pudge;
    
    pudge.name = "Pudge";
    pudge.species = "Cub";
	// I'm too LAZY to fill in the rest for this example
    
    Villager apollo;
    
    apollo.name = "Apollo";
    apollo.species = "Eagle";
    
    Villager bones;
    
    bones.name = "Bones";
    bones.species = "Dog";
    
    // Now we move our villagers into our town with 10 houses:
    Villager myTown[10] = {pudge, apollo, bones};
    
    // And let's see who lives in myTown:
    for (int i = 0; i < 10; i++) {
        if (myTown[i].name != "") {
            cout << myTown[i].name << endl;
        } else {
            cout << "No villager lives here..." << endl;
        }
    }
}
```
