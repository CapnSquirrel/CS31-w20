## Week 10 

Hi there! Most of these notes are taken from [Andrew Forney](http://forns.lmu.build/)'s website. 

You can find the week 10 discussion recording over in CCLE.

## Dynamic Memory

**Dynamic Memory Allocation** is the runtime-capable allocation of memory that is managed by the programmer. It is used for allocating some amount of memory not necessarily known at compile time.

The memory location in which we've previously been storing variables is called the stack.

The **stack**, more formally known as the call stack, pushes local variables onto its top as they are declared, and then "pops" them from the top when they fall out of scope.

This act of "popping" a variable from memory is done automatically with scope when variables are stored in the stack, but is NOT done without programmer intervention with dynamic memory.

So if dynamic variables aren't kept in the stack like with our previously experienced variables, where are they kept?

The **heap**, by contrast, is where dynamic memory is allocated, and does not operate in the same "stacked" fashion that the... well... stack does.

So let's just review some key differences between the two:

|                                    | Stack                                                        | Heap                                                         |
| ---------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **What variables live here?**      | Local variables, functions, function arguments, etc.         | Dynamically allocated memory reserved by the programmer      |
| **How can variables be accessed?** | By any type of identifier defined in scope                   | Only through pointers!                                       |
| **Best for storing:**              | Local variables that are specific to limited scopes          | Variables whose size is not known at compile-time            |
| **Memory is allocated:**           | Whenever a variable is declared in scope                     | Whenever the `new` keyword is used to initialize a variable and call a constructor |
| **Memory is freed / deallocated:** | Whenever a variable disappears from scope (e.g., local variables in a function after returning from that function) | Only after the delete keyword is used!                       |

So in brief: dynamic memory gives us the luxury of allocating memory on-the-fly during runtime, but at the cost of some performance and the necessity to keep track of deallocating memory ourselves.

#### Memory Allocation Syntax

To dynamically allocate memory for a given object, we use the `new` keyword. The new keyword invokes a constructor for a given object based on that constructor's expected parameters, and returns a pointer to the object created:

```C++
  // For any number of expected constructor
  // parameters (including none)
  <type>* <name> = new <type>(parameter1, parameter2, ...);
```

Here are some examples:

```C++
  #include <iostream>
  #include <string>
  using namespace std;
  
  struct dullExample {
      int i;
      string s;
  
      // Default constructor
      dullExample () {
          i = 5;
          s = "wow!";
      }
  
      // Constructor 2
      dullExample (int j, string str) {
          i = j;
          s = str;
      }
  };
  
  int main () {
      // Note two things:
        // laaame is a POINTER
        // We're calling the default constructor here
      dullExample* laaame = new dullExample();
      cout << laaame->i << endl;
      cout << laaame->s << endl;
  
      // HERE, we're using constructor 2 in our dynamic allocation
      dullExample* uuugh = new dullExample(3, ":(");
      cout << uuugh->i << endl;
      cout << uuugh->s << endl;
  
      // WARNING: We should deallocate the memory here so as not to have
      // a leak, but we'll come back and do it later...
  }
```

You should know that, although it's often rather silly in most cases to do so, we can even dynamically allocate memory for primitive types:

```C++
  // Stick i in the heap and initialize it to 2
  int* i = new int(2);
  cout << *i << endl;
  
  // Of course, would need to deallocate here still... derp
```

Using the struct definition from above, will the following code compile? Also, is `bore` on the stack or heap?

```C++
  int main () {
      dullExample bore(5, ">_> <_<");
      cout << bore.i << endl;
      cout << bore.s << endl;
  
      // Also, do we need to deallocate bore here?
  }
```

Will the following code compile? If so, what will it print out?

```C++
int main () {
    dullExample dynamic = new dullExample();
    dullExample duo = dynamic;

    cout << dynamic.i << endl;
    cout << duo.i << endl;

    // TODO: What needs to be de-allocated here?
}
```

Will the following code compile? If so, what will it print out?

```C++
  int main () {
      dullExample* dynamic = new dullExample();
      dullExample* duo = dynamic;
  
      cout << dynamic->i << endl;
      duo->i = 3;
      cout << dynamic->i << endl;
      
      // TODO: What needs to be deallocated here?
  }
```

#### Memory Deallocation Syntax

**Rule of thumb:** whenever we dynamically allocate memory using the `new` keyword, we must later release it using the `delete` keyword.

If we fail to do so, we get a memory leak:

A **memory leak** is defined as a failure in a program to release discarded memory, causing impaired performance or failure.

Memory leaks become problems when we repeatedly forget to free dynamically allocated memory, and we have large, discarded objects that are still allocated but can no longer be reached.

These memory leaks add up until our code may not have enough dynamic memory left to allocate, or doing so slows it substantially.

Here are some properties of the **delete** keyword:

- delete effectively says, "I'm done with this object's dynamically allocated memory; feel free to do with it what you please!" We call delete on the pointer to said object.
- NOTE: This does NOT say, "Zero out the memory it was pointing to" nor does it say "Set the pointer that was pointing there to NULL or nullptr."
- delete called on an object that was already deleted is undefined behavior!
- delete called on a `nullptr` is fine, and does nothing.
- delete only NEEDS to be called on objects that were initialized using the `new` operator.

Let's try it on the example from earlier:

```C++
  #include <iostream>
  #include <string>
  using namespace std;
  
  struct dullExample {
      int i;
      string s;
  
      // Default constructor
      dullExample () {
          i = 5;
          s = "wow!";
      }
  
      // Constructor 2
      dullExample (int j, string str) {
          i = j;
          s = str;
      }
  };
  
  int main () {
      dullExample* laaame = new dullExample();
      cout << laaame->i << endl;
      cout << laaame->s << endl;

      dullExample* uuugh = new dullExample(3, ":(");
      cout << uuugh->i << endl;
      cout << uuugh->s << endl;
  
      // Be free memory! Be free!
      delete laaame;
      delete uuugh;
  }
```

Where will `laaame` and `uuugh` point **after** we call delete on them above? 

- Exactly where they were beforehand! That is, they'll point to the same memory address as they did before, but that memory will now be deallocated.

Find the memory leak!

```C++
  #include <iostream>
  #include <string>
  using namespace std;
  
  struct superDull {
      int lame;
      double doubleLame;
  
      superDull () {
          lame = -100;
      }
  };
  
  struct dullExample {
      int i;
      string s;
      // or is it evenMoreDull?
      superDull* evenDuller;
  
      dullExample () {
          i = 5;
          s = "wow!";
          evenDuller = new superDull();
      }
  };
    
  int main () {
      dullExample* faucet = new dullExample();
      cout << faucet->evenDuller->lame << endl;
  
      // Done... :|
      delete faucet;
  }
```

Will the following code compile? If so, what will it print out?

```C++
  #include <iostream>
  #include <string>
  using namespace std;
  
  struct dullExample {
      int i;
      string s;
  
      dullExample () {
          i = 5;
          s = "wow!";
      }
  };
    
  int main () {
      dullExample dullAgain;
      cout << dullAgain.i << endl;
  
      delete dullAgain;
  }
```

Will the following code compile? If so, will it experience any undefined behavior?

```C++
  #include <iostream>
  #include <string>
  using namespace std;
  
  struct dullExample {
      int i;
      string s;
  
      dullExample () {
          i = 5;
          s = "wow!";
      }
  };
    
  int main () {
      dullExample* dullAgain = new dullExample();
      dullExample* dittoDull = dullAgain;
      
      delete dullAgain;
      delete dittoDull;
      cout << (dullAgain == dittoDull) << endl;
  }
```

Assuming we removed one of the two delete statements in the above example, what would the program print out?

- 1 (true) because although we've deallocated the memory that the two pointers point to, they still point to the same spot!

### More On Structs

We can define multiple constructors for a given type as long as we follow certain rules. Starting with the basics:

The **Default Constructor** is the constructor that expects no arguments.

There are some special properties about the default constructor:

- If you do not specify one, the compiler will generate one for you... this generated default will initialize NO primitive members, but will call the default constructors of all other object members.
- You cannot define more than one default constructor
- If YOU do not define a default constructor, but instead define another constructor (expecting some number of arguments), the compiler will no longer generate one for you.

Will the following code compile?

```C++
  #include <iostream>
  #include <string>
  using namespace std;
  
  struct dullExample {
      int i;
      string s;
  
      dullExample (int j, string s) {
          i = 5;
          s = "wow!";
      }
  };
    
  int main () {
      dullExample* oopsies = new dullExample();
  
      cout << oopsies->i << endl;
      cout << oopsies->s << endl;
  
      delete oopsies;
  }
```

WARNING: You cannot have two constructors that expect the same number and type of arguments **in the same order**.

For example, the following is OK because the parameters, although the same type between constructors, are in a different order.

```C++
  struct dullExample {
      int i;
      string s;
  
      dullExample (int j, string str) {
          i = j;
          s = str;
      }
  
      dullExample (string str, int j) {
          i = j;
          s = str;
      }
  };
```

But the following is NOT OK:

```C++
  struct dullExample {
      int i;
      string s;
  
      dullExample (int j, string str) {
          i = j;
          s = str;
      }
  
      dullExample (int stuff, string derp) {
          i = stuff;
          s = derp;
      }
  };
```

Just like we can define a constructor to specify initial values and behavior, we can define what we want to happen when we are done with our objects.

**destructors** are special functions defined on a struct / class that are called whenever (1) that object was locally defined and fell out of scope, or (2) that object was dynamically defined and had a form of `delete` called upon it.

```C++
  struct someStruct {
    ...
    // Constructor
    someStruct (...) {
      ...
    }
    
    // Destructor
    ~someStruct () {
      ...
    }
  };
```

Notice that the destructor syntax has the following properties:

- Shares the same name of the struct / class
- Starts with a tilde (~)
- Has no return type
- Expects no arguments

Here's an example:

```C++
  #include <iostream>
  #include <string>
  using namespace std;
  
  struct SuperInterestingMember {
      string actuallyNotThatInteresting;
  
      SuperInterestingMember () {
          actuallyNotThatInteresting = "Meh...";
      }
  };
  
  struct SuperInterestingExample {
      int i;
      string s;
      SuperInterestingMember* ob;
  
      // Constructor dynamically allocates member
      // ob of type SuperInterestingMember
      SuperInterestingExample () {
          i = 5;
          s = "wow that's interesting!";
          ob = new SuperInterestingMember();
      }
  
      // DESTRUCTOOOOOOR
      ~SuperInterestingExample () {
          cout << "[!] Destructor called!" << endl;
          delete ob;
      }
  };
  
  // [!] What gets printed in the main function and in what order?
  int main () {
      // NOTICE: sup is not a pointer, just an object declaration
      SuperInterestingExample sup;
    
      cout << sup.ob->actuallyNotThatInteresting << endl;
  }
```

What if we had dynamically allocated `sup`?

```C++
  #include <iostream>
  #include <string>
  using namespace std;
  
  struct SuperInterestingMember {
      string actuallyNotThatInteresting;
  
      SuperInterestingMember () {
          actuallyNotThatInteresting = "Meh...";
      }
  };
  
  struct SuperInterestingExample {
      int i;
      string s;
      SuperInterestingMember* ob;
  
      SuperInterestingExample () {
          i = 5;
          s = "wow that's interesting!";
          ob = new SuperInterestingMember();
      }
  
      // DESTRUCTOOOOOOR
      ~SuperInterestingExample () {
          cout << "[!] Destructor called!" << endl;
          delete ob;
      }
  
  };
  
  // [!] What gets printed in the main function and in what order?
  int main () {
      // NOTICE: sup is now a dynamically allocated var
      SuperInterestingExample* sup = new SuperInterestingExample();
    
      cout << sup->ob->actuallyNotThatInteresting << endl;
    
      // Destructor will get called here so I
      // don't have to worry about releasing ob
      delete sup;
      
      cout << "Just at the end of main here... nothing to see!" << endl;
  }
```

There's a very subtle error here...

```C++
  #include <iostream>
  #include <string>
  #include <cstring>
  using namespace std;
  
  struct SuperInterestingMember {
      string actuallyNotThatInteresting;
  
      SuperInterestingMember () {
          actuallyNotThatInteresting = "Meh...";
      }
  };
  
  struct SuperInterestingExample {
      int i;
      string s;
      SuperInterestingMember* ob;
  
      // NOTICE: change in the constructor...
      SuperInterestingExample () {
          i = 5;
          s = "wow that's interesting!";
      }
  
      ~SuperInterestingExample () {
          delete ob;
      }
  
  };
  
  int main () {
      SuperInterestingExample* sup = new SuperInterestingExample();
    
      cout << sup->ob->actuallyNotThatInteresting << endl;
    
      delete sup;
  }
```

So how can we avoid this issue of not initializing a member object (maybe we don't want to until we need it) and having our destructor try to clean it up?

- Initialize it to the nullptr in the constructor!

### Practice

Talk through these examples with your neighbor/s!

Find the error in this program:

```C++
  #include <iostream>
  #include <string>
  using namespace std;
    
  struct dullExample {
      int i;
      string s;
      
      // or is it "evenMoreDull"?
      dullExample* evenDuller;
      
      dullExample () {
          i = 5;
          s = "wow!";
          evenDuller = new dullExample();
      }
  };
  
  int main () {
      dullExample* faucet = new dullExample();
      cout << faucet->evenDuller->i << endl;
      
      // Done... :|
      delete faucet;
  }
```

Find the memory leak!

```C++
  #include <iostream>
  #include <string>
  using namespace std;
  
  struct SuperInterestingMember {
      string actuallyNotThatInteresting;
  
      SuperInterestingMember () {
          actuallyNotThatInteresting = "Meh...";
      }
  };
  
  struct SuperInterestingExample {
      int i;
      string s;
      SuperInterestingMember* ob;
  
      // NOTICE: change in the constructor...
      SuperInterestingExample () {
          i = 5;
          s = "wow that's interesting!";
          ob = new SuperInterestingMember();
      }
  
      ~SuperInterestingExample () {
          delete ob;
      }
  
  };
  
  int main () {
      SuperInterestingExample* sup = new SuperInterestingExample();
    
      cout << sup->ob->actuallyNotThatInteresting << endl;
  }
```

Find the memory leak!

```C++
  #include <iostream>
  #include <string>
  using namespace std;
  
  struct superOb {
      int i[3];
  
      superOb () {
          for (int j = 0; j < 3; j++) {
              i[j] = j;
          }
      }
  };
  
  struct superEx {
      int i;
      string s;
      superOb* ob;
  
      superEx () {
          i = 5;
          s = "wow that's interesting!";
          ob = new superOb();
      }
  
      ~superEx () {
          delete ob;
      }
  };
  
  superEx* generateCoolness () {
      superEx* coolness = new superEx();
      return coolness;
  }
  
  int main () {
      superEx* sup = generateCoolness();
    
      for (int j = 0; j < 3; j++) {
          cout << sup->ob->i[j] << endl;
      }
  }
```

Will the following code compile? Will there be any undefined behavior? Will it print anything out?

```C++
  #include <iostream>
  #include <string>
  using namespace std;
  
  struct superOb {
      string s;
  
      superOb () {
          string s = "watch out!";
      }
  };
  
  struct superEx {
      int i;
      string s;
      superOb* ob;
  
      superEx () {
          i = 5;
          s = "wow that's interesting!";
          ob = new superOb();
      }
  
      ~superEx () {
          delete ob;
      }
  };
  
  int main () {
      superEx* sup = new superEx();
  
      cout << sup->ob->s << endl;
  
      delete sup->ob;
      delete sup;
  }
```
