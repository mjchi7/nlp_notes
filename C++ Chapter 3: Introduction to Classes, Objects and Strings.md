# Chapter 3: Introduction to Classes, Objects and Strings

### Access Specifiers
There are two types of access specifiers. 

##### Public
Accessible by external party.
##### Private
Accessible only by the class members function itself.

By default, all class members are **private**, except explicitly defined by using the keyword **public**.
```cpp
class GradeBook{
	int scores;
	char grades;

public:
	int getScores(){
		return scores;
	}
	char getGrades(){
		return grades;
	}
}
```
In the example above, both *data members* (more accurately known as *attributes*) are **private**; on the other hand, the *member functions* declared right after the **public** specifier is **public**.


### Members Function
Functions declared within a class definition.

### Data Members
Attributes that are associated with a class.


### Data hiding
Grants `private` specifiers to *data members* of a class and expose `set` and `get` functions for external user to access the *data members*
Why?
- We can impose certain rules on the `set` function so that our *data members* will only take up values of certain form (e.g. length constraints)
- We can store our *data members* in one form, and then present it in other form when user retrieve through `get` function. (e.g. storing time as integer, return it as string)

### Constructors
All class are implicitly given a *default* constructor by the compiler that takes no parameter. For classes with *data member* that are object of *another class*, the default constructor for that class will be called.
When you provide a constructor to a class, the compiler will *not* produce the default constructor anymore, therefore it is up to user to redefine the default constructor which takes the following form
```cpp
class GradeBook{
	/* Default Constructor */
	GradeBook(){};
	/* Constructor 2 */
	GradeBook(string name){
		courseName = name;
	}
}
```
When the object initialization calls the constructor, do take note that there are different way to call the default constructor or other constructor

```cpp
	GradeBook GBDefaultConst;
	GradeBook GBCustomConst(name);
```

Note how in the first initialization, there are no parentheses at the end of `GBDefaultConst`. That's because if there are parentheses after that object name, compiler will misinterpret that statement as a *function* declaration of name `GBDefaultConst`, which takes 0 parameters with return type `GradeBook`. For more information, refers to [here](https://stackoverflow.com/questions/877523/error-request-for-member-in-which-is-of-non-class-type).

### Header Files
We can separately develop files for different classes and function and storing them into a *header file* (with extension `.h`). To include it in our main logic, we can use the following preprocessor:
```cpp
#include "GradeBook.h"
```

##### Double quote  " "
Using a *double quote (" ")* `include` preprocessor as shown in the snippet above, will instruct compiler to look for that specific header file in *current* directory first, if the file is not found, only then it will proceed to find the file at the location for C++ standard library. 

##### Angle bracket < >
Using a *angle bracket (< >)* in the `include` preprocessor will instruct compiler to find that particular file in the  C++ standard library directory.

### Binary Scope Resolution Operator, :: 
Let's say we use this operator in the following context:
```cpp
someClass::someFunction(string name){
}
```
We are telling the compiler to *tie*  function `someFunction`  to the class `someClass`. Without the operator,
```cpp
someFunction(string name){

}
``` 
will be treated as "free" functions (a.k.a global functions), like main by the compiler, which will **not** be able to access the private member of the class `someClass`.

### Linking, Compiling
If you have more than two `.cpp` file, you need to include them in your `g++` invocation
```bash
g++ -std=c++11 -o main main.cpp GradeBook.cpp
```
see how we are explicitly saying that you need to compile both `GradeBook.cpp` and `main.cpp` in order for it to work. If you forgot to compile `GradeBook.cpp` and the header for for `GradeBook.cpp` are being used at `main.cpp`, the compiler will complains thing such as 
*ld: symbol(s) not found for architecture x86_64*

#### Compiling object file
To hide the implementation of member functions, we can precompile the file that implements the member function into an *object* file. With this object file, other implementor programmer and straightaway link its implementation with this *object* file in order for its application to compile. Concretely, say we have 3 files: `main.cpp`, `GradeBook.h`, and `GradeBook.cpp`. We can first compile `GradeBook.cpp` into an *object* file as such:
```bash
g++ -std=c++11 -c -o GradeBook GradeBook.cpp
```
the argument `-c` is telling the compiler to **compile** the source file without linking other files. 

After that to compile `main.cpp` (which make use of some functions defined in `GradeBook.cpp`) we can use the following command

```bash
g++ -std=c++11 -o main main.cpp GradeBook
```

See how we include `GradeBook` object file (compiled in step 1) in order for `main.cpp` to compile.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3NTI2MjMxMDNdfQ==
-->