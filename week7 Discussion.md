# Week 7 Discussion

## Announcements

- **Project 5 due**: 9:00 PM Wednesday, **February 26th**.

## Questions

Ask!!!

## Today's Topics

- Class
- Public/Private
- Simple constructors 
- Methods and Data Members

## C++ Classes

Classes and structs are almost the same exact thing. There is one difference between the two; how they default label their data members.

Let's model a struct to represent UCLA students. 

```C++
struct Bruin {
	string name;
    string major;
    int age;
    int UID;
};
```

By default, every data member in a struct is labeled `public`, so we have access to them via the accessor operator. 

```C++
struct Bruin {
	string name;
    string major;
    int age;
    int UID;
};

int main() {
	Bruin Alejandro;
    Alejandro.name = "Alejandro";
    Alejandro.major = "CS";
    Alejandro.age = 23;
    Alejandro.UID = 123456789;
    
    cout << Alejandro.UID; // we have unrestricted access to Alejandro's personal info
}
```

But in classes, every data member in the class is labeled `private` by default, and we will not be able to access any of them directly.

```C++
class Bruin {    // notice we use the keyword class, but nothing else changes
	string name;
    string major;
    int age;
    int UID;
};

int main() {
	Bruin Alejandro;
    Alejandro.name = "Alejandro"; // We can't even set my name! 
    // I will remain but an empty husk...
}
```

## Public / Private

We've seen that structs and classes have default behavior with how they use `public` and `private`, but we haven't seen how we can make use of the tags *ourselves*.

When using `public` or `private` tags, we simply place them above the data members that we want to be public or private, followed by a `:`.

```C++
struct Bruin {
	string name;	// public by default
    string major;	// public by default
private:
    int age; 		// data member is now private
    int UID; 		// data member is now private
    float GPA;		// data member is now private
};
```

```C++
class Bruin {  
    int age;		// private by default
    int UID;		// private by default
    float GPA;		// private by default
public:
	string name;	// data member is now public
    string major;	// data member is now public
};
```

By labeling the appropriate data with the appropriate labels, we can create objects with more interesting behavior... Although right now, we just have data members that are completely inaccessible. We'll see how to set and access their data soon. 

## Constructors

A constructor is an optional way of defining how users of your struct or class can initialize the data members of objects. They look a lot like functions, and we can define zero or more of them.

There are two types of constructors:

**The Default Constructor:** Is a constructor which we define with no parameters; in other words it takes no arguments.

There are some special properties about the default constructor:

- If you do not specify one, the compiler will generate one for you... this generated default will initialize NO primitive members, but will call the default constructors of all other object members.
- You cannot define more than one default constructor
- If YOU do not define a default constructor, but instead define another constructor (expecting some number of arguments), the compiler will no longer generate one for you.

```C++
struct Bruin {
    Bruin();		// We could define it up here, or just give the prototype
	string name;
    string major;	
private:
    int age; 		
    int UID; 		
    float GPA;		
};

// :: is called the scope resolution operator
// We'll see it again in a better example below
Bruin::Bruin() {
    age = 0;
    UID = 0;
    GPA = 0.0;
}

int main() {
    Bruin Alejandro();	// Once again I am nothing...
}
```

In the example above, we defined a default constructor for Bruin which sets the age, UID, an GPA data members to "zero" equivalent values.

But when a new Bruin is born, they aren't assigned an age of 0, or UID of 0... much less an empty name and major... So let's write a constructor which will allow our users to create student objects with all fields filled in with appropriate values. This time with a class, for variety.

```C++
class Bruin {
	int m_age;
	int m_UID;
	int m_GPA;
public:
	Bruin(string name, string major, int age, int UID);
    string m_name;
    string m_major;
};

Bruin::Bruin(string name, string major, int age, int UID) {
    m_name = name;
    m_major = major;
    m_age = age;
    m_UID = UID;
}

int main() {
    Bruin alejandro("Alejandro", "CS", 23, 123456789);
    
    cout << alejandro.m_name << '\n';
    cout << alejandro.m_major << '\n';
}
```

## Methods

A **method** is simply a function within a class (or struct). 

Adding methods to our objects is how we create functionality for them. They are also how we can set and get the values in our data members!

```C++
class Bruin {
	int m_age;
	int m_UID;
	int m_GPA;
    string m_name;
    string m_major;
public:
	Bruin(string name, string major, int age, int UID) {
        m_name = name;
        m_major = major;
        m_age = age;
        m_UID = UID;
	}
    
    void setName(string name) {
		m_name = name;
    }
    
    string getName() {
        return m_name;
    }
    
    void changeMajor(string major) {
        m_major = major;
    }
    
    string getMajor() {
        return m_major;
    }
    
    void updateAge(int age) {
        m_age = age;
    }
    
    int getAge() {
        return m_age;
    }
    // so on and so forth, for whatever functionality you want...
};

int main() {
    Bruin alejandro("Alejandro", "CS", 23, 123456789);
    
    cout << "Student's name is " << alejandro.getName() << '\n';
    alejandro.changeMajor("animation");
    cout << "Student's major is " << alejandro.getMajor() << '\n';
}
```

By using methods and limiting user's access to data members, we can create interfaces that guarantee an object will (almost) never be used incorrectly or break.

## Bonus Round: C++ Exceptions

You know how the compiler often tells you what you did wrong? It usually looks something like

```
 line 13: error: expected ‘;’ before ‘}’ token,
```

Turns out we can make our own kind of errors, and even specify what should happen when these errors arise!

The syntax for creating an error, and then reporting it is as follows:

```C++
logic_error <error_name>( “custom error message here!” );
throw(<error_name>);
```

You can create the error anywhere in your code (notice we created it just like we would create a variable or object), but you should only `throw` the error once you've caught something that should not happen (e.g. the user attempted to create an object with bad values).

Now, here's how you catch those errors from a scope outside of where you threw the error:

```C++
try {
	// some code which may throw an error in here
} catch( <error_type> ) {
	// custom behavior for when the block above "throws" an error.
}
```

We call the above structure a "try-catch". Creative, right?

Here's a more concrete example: