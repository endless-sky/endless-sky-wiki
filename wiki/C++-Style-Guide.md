# Table of Contents

* [Background](#background)
* [Files](#files)
* [Namespaces](#namespaces)
* [Classes](#classes)
* [Functions](#functions)
* [Variables](#variables)
* [Comments](#comments)
* [Formatting](#formatting)
* [Best practices](#best-practices)
* [C++11](#c11)

# Background

Each section of this style guide can be read as a "how to": how to create source code files, how to create classes, etc. The sections are ordered hierarchically, from biggest "units" (files, namespaces, classes) to increasingly smaller ones (variables, comments, formatting). Our style choices are based on a small set of principles:

- Readability is achieved by laying out text in a clear visual hierarchy.
- Code should not rely on features of a particular editor or compiler.
- Formatting should be consistent to avoid confusing or distracting the reader.
- Reducing the risk of bugs is important even if it takes more work.


# Files

This section deals with naming and encoding of source code files and the items that appear at the start of each file.

## Line endings

All files should have Unix-style line endings.
That is, `\n` alone, not `\r\n` (Windows style). If an editor changes Unix line endings to Windows style, this causes issues with source control diffs because those lines appear to be changed even though their actual text is the same.

## File encodings

All files should be plain ASCII.
That is, nothing beyond the 127-character ASCII set should be used. This is to ensure maximum cross-compatibility.

## File extensions

All C++ files should use ".h" and ".cpp" for their extensions.
This is a somewhat arbitrary choice, but these seem to be the most commonly used extensions.

## One class per file

Each class "MyClass" should have its own .h and .cpp files, and they should be named "MyClass.h" and "MyClass.cpp".
This is so that it is easy to find the implementation of any class you are interested in.

## Names for files with no classes

If a file does not contain any classes (such as "main.cpp" or "shift.h"), its name should be lower-case.
This is just to make it clear that the file is not defining a class.

## Header file guards

All header files should have #pragma once guards.
The C++ standard technically does not allow #pragma once, but all major compilers support it. It is also much easier to use than regular header guards.

## Header and copyright statement

Each file should begin with a header which specifies the file name, copyright, and licensing information, e.g.:
```c++
/* Key.h
Copyright (c) 2014 by Michael Zahniser

Endless Sky is free software: you can redistribute it and/or modify it under the
terms of the GNU General Public License as published by the Free Software
Foundation, either version 3 of the License, or (at your option) any later version.

Endless Sky is distributed in the hope that it will be useful, but WITHOUT ANY
WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A
PARTICULAR PURPOSE. See the GNU General Public License for more details.

You should have received a copy of the GNU General Public License along with
this program. If not, see <https://www.gnu.org/licenses/>.
*/
```
If you make substantial additions or changes to a file, please add your name to the copyright list.

## Placement of #includes

All #includes in a file should be placed prior to anything else except for the #ifndef guards.
This is so it is immediately clear which files a given file includes.

## Order of #includes in a .h file

Within a .h file, the #includes should be broken into "paragraphs" each separated by a single blank line:
1. Classes that this file's class inherits from.
2. Other files in the "endless-sky" code base.
3. Non-standard third-party libraries.
4. Standard libraries.
5. Optionally, forward declarations of anything that can be forward-declared.

For example, a header file might contain the following #includes:
```c++
#include "MyBaseClass.h"

#include "Point.h"
#include "Shader.h"

#include <GL/glew.h>

#include <vector>

class Angle;
class Sprite;
```
It is helpful but not required to alphabetize the lines within each "paragraph" of #includes.
Forward-declaring classes instead of #including their headers reduces compilation time somewhat because it eliminates dependencies. But, compilation times are so short for this project anyways that using forward declarations is not an absolute requirement.

## Order of #includes in a .cpp file

Within a .cpp file, the #includes should be broken into "paragraphs" each separated by a single blank line:
1. This file's corresponding .h.
2. Other files in the "endless-sky" code base.
3. Non-standard third-party libraries.
4. Standard libraries.

For example, a .cpp file might contain the following #includes:
```c++
#include "MyClass.h"

#include "Angle.h"
#include "Sprite.h"

#include <SDL/SDL.h>

#include <set>
```
It is helpful but not required to alphabetize the lines within each "paragraph" of #includes.



# Namespaces

This is not a large enough project to require splitting it into multiple namespaces.

## Use the global namespace

Put all classes except for local helpers in the global namespace.
This project is never going to grow to a size where multiple namespaces are needed.

## Unnamed namespaces

Put local helper classes, functions, and variables in an unnamed namespace.
This allows you to define static classes, functions, and variables that are guaranteed to only be visible from within a single compilation unit. (You should not be using globals that are visible from more than one .cpp!)

## Using declarations

Do not put any using declarations in any header file.
This is so that a file cannot end up dependent on a using declaration provided by a file that it #includes and suddenly failing to compile if that other file changes.
This includes general using-directives (e.g. `using namespace std;`) in addition to specific using-declarations (e.g. `using std::string;`).

## The std namespace

Each .cpp file should put `using namespace std;` immediately after the #includes.
Because our class and function names are capitalized, there is no risk of name collision, and it is simply cleaner to not have to use the std:: prefix everywhere.



# Classes

For clarity and simplicity, nearly every function should be part of a class, and every class should be contained in its own files named after that class.

## Class names

Class names should be CapitalizedCamelCase. Do not use Hungarian notation.
That is, for example, do not put a "C" at the start of all your class names to mark the fact that they are classes. Since we do not define any global functions, a capitalized name by itself is pretty much guaranteed to be a class, so we do not need any further markers to help the reader.

## Order of declarations

A class definition should contain the following sections (most of which will be omitted for most classes) in this order, with each section introduced by a `public:`, `protected:`, or `private:` marker:
1. public enums and nested classes
2. public static constants
3. public static functions
4. public functions
5. protected functions
6. private nested classes
7. private functions
8. private variables

Anything not listed here (e.g. public non-constant variables, protected variables, etc.) should not be used.

Here is an example class definition, with extra comments added to point out aspects of the formatting:
```c++
class MyClass : public MyBaseClass { // Brace on the same line.
public: // Do not indent the tags for "public," etc.
    MyClass(); // Default constructor always comes first.
    explicit MyClass(int size); // Avoid unexpected implicit conversion.
    
    // These comments summarize the following block of functions, with enough
    // detail for anyone using this interface (but who does not care about the
    // implementation details).
    bool IsEmpty() const; // Use "const" whenever possible.
    size_t Size() const; // Use unsigned types for sizes.
    
    // Another set of descriptive comments.
    void SetLabel(int index, const std::string &label);
    const std::string &GetLabel(int index) const;
    void GetIndex(const std::string &label) const;
    
    // This class behaves like an array; you can look up elements by index, or
    // by label (if a given label has been assigned to an index).
    double operator[](int index) const; // Only use obvious operator overloads.
    double operator[](const std::string &label) const;
    
    
private: // Two blank lines separating each section.
    // Clear any index previously associated with the given label.
    void ClearLabel(const std::string &label);
    
    
private: // Separate sections for functions and for variables.
    std::vector<double> values;
    std::vector<std::string> labels;
    std::map<std::string, size_t> indices;
};
```

## Inline functions

Do not define any functions within the class definition.
The class definition should provide a concise definition of the interface of the class, so that someone using the class can scan through it quickly. Defining functions inline makes this more difficult.
If you have determined that making a function inline will substantially improve performance, or if it is a templated function, define it in the header after the end of the class definition.

## Initialization order

Initializer lists in constructors should list the variables in the same order they occur in the class.
Variables are initialized in the order they occur in the class definition, not in the initializer list. Most compilers will report a warning if you change the order because it means your initializer list might be written to depend on a variable that has not been initialized yet.

## Explicit constructors

Constructors with only one argument should be labeled `explicit`.
This is to avoid defining implicit type conversions that might then be used by the compiler in surprising ways.

## Operator overloading

Use operator overloading only if implementing an arithmetic class and only if the operator has its well-established mathematical sense.
For example, the `Point` class can define Point + Point, Point * scalar, etc., but it should not define Point * Point because it is ambiguous whether that should be an element-by-element product, a dot product, or a cross product.

## Use of class vs. struct

Do not use the `struct` keyword; just define a class with public data members instead.
Exception: occasionally when using the POSIX libraries you need the `struct` keyword to define that a typename is a struct and not a function of the same name, e.g.:
```c++
struct stat sb;
stat("path/to/file", &sb);
```

## Inheritance

Use inheritance only for an "is a" relationship. Do not use protected or private inheritance.
Use composition if MyClass "has" an OtherClass instead of "is" an OtherClass.

## Public data members

Avoid public non-constant data members except in classes that are used like structs.
Occasional exceptions are allowed if they lead to much clearer code.

## Mutable

Use `mutable` only for class members not related to the outwardly visible state of the class.
For example, if a class caches the result of a complex calculation, that may be stored in a mutable variable. Similarly, if a class owns a worker thread that it communicates with, a const function checking on the worker's status may need to use a mutable mutex.

## Nested classes

Nested classes are allowed, but use them sparingly.
For example, a nested class representing a single node of a data structure or a complex return type might make sense.

## Runtime type information

Do not make use of runtime type information.
That includes using dynamic_cast<>().

## Friends

Use friends only when absolutely necessary.
Using friends is better than making private class data public just so that one other class can use it, but much worse than just refactoring to avoid the need for data sharing in the first place.

## Reinventing the wheel

Do not create your own classes where you can just use the STL.
The STL will generally be as fast as anything you can write yourself, and also much less buggy.



# Functions

This section describes how functions should be named and also lays out some widely-accepted design principles for factoring code into functions.

## Function names

Function names should be CapitalizedCamelCase.
There is no single standard in C++ for whether or not function names are capitalized. But, constructor names must be capitalized, so it makes sense to capitalize the others too. Also, it is easy to distinguish class names from function names or variables based solely on context, but the only easy way to distinguish a function from a function object is by capitalization.
Exception: function names that match the standard library (such as begin() and end()) may be lower-case.

## Definition order

Functions in the .cpp file should appear in the same order as they are declared in the .h file.
This makes it easier to scan through the files to find a function, which is often faster than going to the trouble of doing a Find... within the file. It also means that related functions are grouped together instead of appearing in an arbitrary order.

## Const correctness

Any function or function parameter that can be const, should be const.
It takes a little bit of extra work when defining a class interface, but makes all sorts of subtle errors impossible to commit.
Note that this means that if a class provides begin() and end() iterators, you need to define both a const and a non-const copy of those functions.

## Exception safety

Use the RAII idiom whenever possible so that code is automatically exception-safe.
That is, use objects like ofstream, vector, lock_guard, etc. that automatically do whatever cleanup is necessary (closing a file, freeing memory, releasing a mutex) when they are destroyed. That produces cleaner code anyways, and has the advantage that you don't even have to worry about where in the function an exception might get thrown.

## Avoid copy-and-pasted code

If nearly identical code appears in two places, factor it out into a separate function.
Otherwise, if you make a change or a bug fix, you're likely to make it in one of the copies but not in the other. And, copy-and-paste is often a sign of quickly written throw-away code.

## Non-member functions

All functions should either be a member of a class or of an unnamed namespace.
This implies that some classes may consist entirely of static functions, and never actually be intended to be instantiated.
This is so that it is immediately clear to anyone reading the code what file they need to look at to see the declaration or definition of that function. That is, if I see `MyClass::DoStuff(myData);`, I know to look at MyClass.h for details. If I see `DoStuff(myData);`, I have to read through all the #includes to see which one defined DoStuff().
Functions in an unnamed namespace are only visible in a single .cpp file, so they will generally just be helper functions for that particular class.

## Void

Do not use `void` to denote a function with no arguments.
An empty list of arguments is much clearer than a list of one element that happens to be void. The only reason people use `void` is as a carry-over from C, where these two function declarations mean different things:
```c
void F(void); // has zero arguments.
void F(); // has an undefined number of arguments.
```

## Default arguments

Use default arguments only when it is self-evident what the default ought to be.
Using default arguments is preferable to defining several overloads of a function which all duplicate the same code.



# Variables

This section deals with naming of variables, scoping, initialization, etc.

## Lower-case camelCase

Variable names should be in camelCase, starting with a lower-case letter.
This is so that variables can be easily distinguished from class and function names.

## Constants in ALL_CAPS

Constants may be in ALL_CAPITALS_WITH_UNDERSCORES.
Or they can use the same format as normal variables.

## Use const rather than #define

Constants should be defined as `static const` rather than using macros.
This avoids unintended side effects and is also more efficient if the variable in question has a non-trivial constructor, because this way one instance of it can be reused.

## No Hungarian notation

Do not use Hungarian notation (prefixes or suffixes on variable names just to represent their type).
Hungarian notation makes variable names harder to read and harder to remember, because instead of English words they include random character sequences like `lptsz`. Also, because the compiler is checking for type safety anyways, these prefixes do nothing to improve code quality.
This also includes not putting `m_` or `_` before or after member variable names. If your functions and classes are well-factored, you should not encounter many situations where it is unclear to a reader whether a variable is a local variable or a member variable.

## Point of first use

Declare variables at the point of first use, not at the top of the function.
This avoids having variables be declared but not yet initialized. It also avoids requiring someone who is reading the middle of a function to keep looking back to the start of the function to figure out what variables are.

## Avoid reusing variables

Rather than using one vaguely named temporary variable in many places in a function, just define new variables for each new value you are storing.
The compiler will be able to figure out when each variable is no longer in use and can reuse whatever register it is in; you do not have to manage that.

## Smallest possible scope

Put local variables in the smallest scope possible.
This goes along with the previous two points, and also helps avoid name collisions. It is also sometimes useful to define a scope purely for the sake of limiting a variable's lifetime:
```c++
void MyClass::DoSomethingSafely(const Data &newData)
{
    CheckSomeStuff();
    {
        lock_guard<mutex> lock(myMutex);
        mySafeData = newData;
    }
    DoSomeMoreStuff();
}
```

## No globals

Use a Singleton class or a similar construct instead of global variables.
A Singleton is almost as dangerous as a global, but has the advantage that it is easy to see when it is being used.

## Be careful with static locals

Do not use static local variables unless you have thought through the thread safety implications.
A static local is initialized the first time the flow of control passes through a function, so if two threads call the same function at the same time, it can be initialized twice. Use a static variable in an anonymous namespace instead, to be safe.



# Comments

Exceedingly verbose comments can be as harmful as no comments at all; this section provides some guidelines for finding a happy medium.

## Headers are for quick reference

As a general principle, the header file for a class should be usable as a quick reference to that class's interface.
That is, someone who needs to use a class but does not need to know implementation details should be able to get all the information that they need from the header. This also implies that you should not put implementation details or other excess comments into the header, since it should be as concise as possible. In particular, it is not required to have individual comments for every function and variable in the header.

## Comments should not reiterate code

Comments should provide additional information needed to interpret the code, rather than reiterating every step the code performs.
That is, avoid this sort of thing:
```c++
int temp; // Define an integer called "temp".
temp = y; // Copy "y" into "temp".
y = x; // Copy "x" into "y".
x = temp; // Copy the previous value of "y" into "x".
```
The following is more concise, and also much clearer:
```c++
// Swap x and y.
int temp = y;
y = x;
x = temp;
```
Of course, in this case using std::swap() is a better alternative than either.

## Comment paragraphs, not lines

Put comments at the start of each non-trivial code paragraph explaining what it does.
That is, by scanning through the comments, someone should be able to get an outline of what a function does. This also means that clarity is improved by taking out comments that are too low-level, as in the first example in the previous point.

## Comments for classes

Put comments before each class definition giving an overview of the class.
The overview should explain where and why you would use a class; comments on the individual functions will explain how.

## Dead code

Do not leave any "dead code" when checking code in.
This does not include code-like comments, e.g. equations.

## Multi-line comments

Multi-line comments should only be used for the copyright header.
This is mainly so that multi-line comments can be used as a quick and dirty way to comment out blocks of code while developing, but it also makes it much easier to write automated programs that extract all the comment text, e.g. for spell checking the comments.

## Decorations

Fancy, decorated comments should not be used.
For example, perhaps your editor has a "feature" that lets you automatically insert comments like this:
```c++
/*************************************
 * My fancy comment.
 * This is a comment that could have
 * been a single line without these
 * fancy decorative elements.
 *************************************/
```
But, those two lines of asterisks are eye-grabbing and completely unnecessary on any editor that supports syntax coloring, and more importantly anyone using a different editor would have to manually type an asterisk at the beginning of every comment line and figure out exactly how many asterisks are in those rows at the top and bottom, in order to be consistent with your code.

## SVN commit comments

When committing code to SVN, include comments describing everything that is changing.
If you are committing multiple changes, it is best to group them into separate commits that each change one thing, rather than committing them all together.



# Formatting

Proper formatting creates a visual hierarchy, e.g. of a set of functions (each separated by large vertical gaps) composed of a set of code paragraphs (separated by small vertical gaps) each composed of lines, which are composed of individual words (separated by horizontal space). If any of that white space is left out, objects in the hierarchy merge together and readability is harmed. Also, if code is inconsistently formatted it can be confusing and distracting, and looks sloppy and unprofessional.

## Tabs

Use tabs instead of spaces so that the code can be edited in any editor easily.
Some editors support "smart spaces" where pressing TAB inserts some number of spaces and pressing DELETE deletes the proper number of spaces if there is nothing before the cursor on the previous line but white space, but most do not, requiring you to press DELETE exactly the right number of times to get to your desired indentation level.

## Line length

Try to keep lines to about 80 characters in length, and wrap anything over 120 characters.
Keeping the code all on one line is preferable in most cases, even if you have to rewrite the line into two separate statements to make it short enough. Wrapped lines are hard to read, but so are super-long lines.

## Large vertical breaks

Put three blank lines between function definitions, and before and after the class definition in a .h file.
Aside from files themselves, these are the largest "units" the code is broken up into. If your code is well-written, the reader should not need to keep looking back and forth between multiple functions, so it does not matter that the extra space will make it so fewer functions fit on the screen at once.



## Medium vertical breaks

Put two blank lines between sections in a class definition, or to split up sections of code paragraphs in a large function.
Of course, if a function is that long it might need to be refactored anyways.

## Small vertical breaks

Use single blank lines to group lines of code into "paragraphs," both within class definitions and within functions.
The rules here are analogous to paragraphs in prose: start with an introductory sentence (e.g. a comment summarizing what follows) and make sure that each "paragraph" is only dealing with one thing.

## Indentation in blank lines

Blank lines should not be indented.
This is to avoid conflicts between different editors, and to make style checking easier.

## One expression per line

Do not put multiple expression on the same line.
Except, of course, for the three individual clauses contained in a for loop.

## Parentheses

Do not put a space before or after the parentheses for functions and control statements.
There is no consensus on this among programmers. But, the mathematical convention is to never put a space, i.e. you never write `sin (x)`. Much of our code is mathematical in nature, so it makes sense to follow that convention.
As part of that convention, also don't put spaces inside the parentheses (like `sin( x )`). The only exceptions from this rule are macro calls in the unit tests:
```c++
SCENARIO( "Creating an Account" , "[Account][Creation]" ) {
    GIVEN( "an account" ) {
        Account account;
        WHEN( "money is added" ) {
            ...
        }
    }
}
```
If you are used to putting spaces before the parentheses, rather than trying to adjust your typing you might find it easier to just do a find/replace before committing your code, e.g. replacing `if (` with `if(`, etc. You can also grep the svn diff of your changes for `^+.*\ (` to check if you missed anything.

# Template declarations

Similarly to the above, do not put a space before or after the angle brackets when declaring or using templated classes and functions, or with type casting (static_cast<>(), reinterpret_cast<>(), etc.) operators.
Put the `template` keyword with the parameter list on a separate line before the name of the class or function:
```c++
template<class T>
class TemplatedContainer {
public:
	template<class D>
	void Convert(TemplatedContainer<D> &other);
	// ...
```

## Return is not a function

The `return` keyword does not require parentheses; use them only when what follows is a complex statement, and put a space after `return` in that case.
For example:
```c++
return x;
return (x != 3 && x > 0);
```

## Loop bodies go on a separate line

Always put the body of an if or loop on a separate line.
That makes debugging easier, and also makes the code easier to read.

## Use continue rather than empty loop bodies

If defining a loop with an empty body, put `continue;` as its body.
All three of these are legal C++:
```c++
int i = 0;
for( ; i < str.length() && str[i] > ' '; ++i) ;
for( ; i < str.length() && str[i] > ' '; ++i) {}
```
```c++
for( ; i < str.length() && str[i] > ' '; ++i)
    continue;
```
but the third option makes it most clear that the loop has an empty body.

## Const

Write `const int x`, not `int const x`.
This just makes more sense in English usage.

## Pointers and references

The & or * when defining a variable should be adjacent to the variable name, not the type.
This is mainly as a reminder that `const int *x` means that *x is a const int, not that x is a `const int*`. It also avoids problems like this:
```c++
int* x, y; // x is a pointer, but y is an int! Probably not what you intended.
```

## Unary operators

Do not put spaces between unary operators and their operands.
This makes it clearer which object the unary operator is operating on, i.e.:
```c++
bool check = *x || !y;
```
which is a whole lot clearer than:
```c++
bool check = * x || ! y;
```

## Commas

Put a space after commas.
This makes lists easier to read.

## Binary operators

Put a space before and after all binary operators except for `,`, `.`, `::`, and `->`
This is for readability, so that the things being operated on are visually parsed as separate words.

## Ternary operators

Put a space before and after `?` and `:`.
This is the same as for binary operators.

## Initializer lists

Start initializer lists on a new line, indented once.
That is:
```c++
MyClass::MyClass()
    : x(0), y(0)
{
    // Any further initialization goes here...
}
```

## Function definitions

Put the return type on the same line as the function name.
That is:
```c++
int Fibonacci(int x)
{
    if(x < 2)
        return 1;
    return Fibonacci(x - 1) + Fibonacci(x - 2);
}
```
rather than:
```c++
int
Fibonacci(int x)
{
    // ...
```

## Single-line loop bodies

You should not use braces if a loop or conditional body is a single expression.
That is:
```c++
if(theThing != done)
    DoTheThing();
```
You should also forgo the braces for something like this:
```c++
for(int i = 0; i < 10; ++i)
    if(vec[i])
        sum += i;
```
And, you should also do this:
```c++
for(int i = 0; i < 10; ++i)
    if(vec[i])
    {
        sum += i;
        DoSomeMoreStuff();
    }
```
But, you should not do this, even though it is legal C++:
```c++
for(int i = 0; i < 10; ++i)
    if(vec[i])
        sum += i;
    else
        sum -= i;
```

## Closing braces

Always put closing braces on a separate line, except in a do...while loop.
That is, do not do this:
```c++
if(!y)
{
    x = 1;
    continue;
} else {
    x = 2;
}
```

## Opening braces

Put opening braces on a new line if and only if they come after a `)`.
That is:
```c++
class MyClass {
public:
    MyClass();
};

do {
    ++x;
} while(x < 10);

for(int i = 0; i < 10; ++i)
{
    x += i;
}

while(true)
{
    DoStuff();
}

if(theThing != done)
{
    DoTheThing();
    return;
}
```
The reasoning here is that because we do not require braces for single-line bodies, the braces should be clearly visible when they are present.
The only exceptions are macro calls that are only found in the unit tests:
```c++
SCENARIO( "Creating an Account" , "[Account][Creation]" ) {
    GIVEN( "an account" ) {
        ...
    }
}
```


# Best practices

This section deals with other "best practices" that do not fit cleanly into one of the categories above.

## Learn new things

If you encounter something (a mathematical concept, a syntactical construct, etc.) that you do not understand, use the Internet to learn how it works.
Consider the difference between the "Basic English Wikipedia" and the ordinary English Wikipedia. The former uses much simpler grammar and a much more limited vocabulary... and because of that is wordy, unclear, and ambiguous. A non-native English speaker would prefer the Basic English, but a native speaker will find the "Basic" English harder to read and more time-consuming to comprehend. The same is true in software: using more complex syntax and a wider variety of libraries can, if done well, produce code that is less buggy and easier for an experienced program to read.
For example, an affine transform takes one line to write out in matrix form, or half a page of equations if you are not using linear algebra. In general, when writing code you should assume that anyone reading it has at least a high school level of mathematical ability, so matrix equations, logarithms, trigonometry, calculus, etc. are all fair game.

## No compiler warnings

Code should compile with -Wall without any warnings.
Some of the warning are rather pedantic, but the effort required to eliminate them is worth it compared to the effort of tracking down subtle bugs that might otherwise exist.
The "comparing signed to unsigned value" warning is particularly common, since the sizes of STL containers are unsigned, but it's common practice to use plain old `int` whenever an integer is needed, instead of using `unsigned`. Assuming that your indices are never going to be larger than the maximum value of a signed int, instead of this:
```c++
if(index < 0 || index >= vec.size())
```
you can do this, which silences the warning and is also more efficient:
```c++
if(static_cast<size_t>(index) >= vec.size())
```
(If you don't understand what that statement is doing, just think of what the 2's complement binary representation of a signed number is, and what happens when that gets converted to an unsigned number.)

## Avoid multiple side-effects

Try not to include multiple side-effects on a single line unless it's in a context where they are expected.
For example, consider these two for loops:
```c++
for(it = vec.begin(); it != vec.end(); )
    *out++ = *it++;
```
```c++
for(it = vec.begin(); it != vec.end(); ++it, ++out)
    *out = *it;
```
The first option is very slightly more concise, but putting the increments into the for statement is more standard practice, and the compiler will almost certainly be able to optimize it equally well. A third, equally good option for the above would be:
```c++
for(it = vec.begin(); it != vec.end(); ++it)
{
    *out = *it;
    ++out;
}
```
This has the advantage that the `for` clauses now deal only with the `it` variable; that is, it is now a "normal" for loop.

## C-style typecasts

Do not use C-style typecasts.
That is, use static_cast<>() or reinterpret_cast<>(). Try to avoid using dynamic_cast<>() or const_cast<>().

## Pointers vs. references

In function parameters, it may be helpful to pass constant parameters by reference, and non-constant parameters by pointer, so that it is immediately clear which parameters are being modified.
The current code does not do this, but it seems like a good practice to move towards.

## No manual memory management

If at all possible, use wrapper classes instead of explicitly allocating or freeing memory.
This makes code exception-safe, and also avoids all sorts of memory management errors. Note that std::vector always stores its contents in a contiguous block of memory, which you can access with its data() function, so there is almost never any reason to allocate your own memory instead of using vector.
If you must use explicit memory management, always use the C++ keywords `new` and `delete`, rather than the C-style `malloc()` and `free()` functions. This is because creating memory with one set of functions and deleting it with the other results in undefined behavior. Also, unlike `malloc()`, `new` ensures that an object is properly constructed.

## No assumptions about variable sizes

Do not assume that sizeof(int) or sizeof(void *) will be a certain value.
Use explicit types like int64_t when necessary, or just use a `long long` if all you care about is that a variable is at least 64 bits, not exactly 64 bits.



# C++11

C++11 brings substantial additions to the C++ language; in fact, one of the main reasons I started this project was to have an opportunity to see what it is like to design a large project from the ground up using the new features of C++11.

## nullptr

Use `nullptr` for pointers rather than `NULL` or `0`.
This has clear type-safety benefits, and also avoids needing to use a macro.

## auto

Use `auto` only for variables with a very small scope, and only when writing out the full type name would make the code less readable.
That is, do not use `auto` just to save yourself some typing. Code is written once, but read many times, and in general it is going to be helpful to have type names spelled out.

## Range-based for

Prefer range-based for loops whenever possible.
Range-based for is more clear and concise than using an index or an iterator.

## Default assignment and constructors

Use the `= delete` syntax to disable copying of a class if necessary.
In general, just let the compiler automatically generate the copy constructor and assignment operator; very few classes will require custom copying.

## Lambdas

Lambdas are an acceptable alternative to simple function objects.
In particular, use lambdas when working with STL functions that require a function object, because defining the function immediately before using it is clearer than defining it in a class in, say, an anonymous namespace at the top of the file.
