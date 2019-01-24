# Some notes, here and there (C++)

This particular documents contains scribbles and notes that is important, but does not fit nicely into other documents.

### Processing a line of input

```cpp
std::vector<float> scores;
float s;
std::string inputLine;

std::cout << "Different scores are separated by whitespace and termination of input can be made by hitting 'Enter'" << endl;

std::getline(std::cin, inputLine);
std::istringstream stringStream(inputLine);

while (stringStream >> s){
		scores.push_back(s)
	}
}
```
The important thing in this code snippet is the way it parses the "scores" input from a single line.  

First the entire line is obtained and placed into variable `inputString` using function `std::getline(std::cin, inputString)`.

Next, an "Input String Stream" is then created using `std::istringstream` and the input line captured as a string in `inputString`. 

The "Input String Stream" can then be looped over so that all the parse-able "scores" are extracted, and pushed into the vector.

### On compiling `.cpp` and `.h` files and linking them
>"In C++ programming as a general practice we separate development into two file types. One is with an extension of  **.h**  and we call this a "header file." They usually provide a declaration of functions, classes, structs, global variables, typedefs, preprocessing macros and definitions, etc. Basically, they just provide you with information about your code. Then we have the  **.cpp**  extension which we call a "code file." This will provide definitions for those functions, class members, any struct members that need definitions, global variables, etc. So the  **.h**  file declares code, and the  **.cpp**  file implements that declaration. For this reason, we generally during compilation compile each  **.cpp**  file into an object and then link those objects (because you almost never see one  **.cpp**  file include another  **.cpp**  file).
>
>How these externals are resolved is a job for the linker. When your compiler processes  **main.cpp**, it gets declarations for the code in  **class.cpp**  by including  **class.h**. It only needs to know what these functions or variables look like (which is what a declaration gives you). So it compiles your  **main.cpp**file into some object file (call it  **main.obj**). Similarly,  **class.cpp**  is compiled into a  **class.obj**  file. To produce the final executable, a linker is invoked to link those two object files together. For any unresolved external variables or functions, the compiler will place a stub where the access happens. The linker will then take this stub and look for the code or variable in another listed object file, and if it's found, it combines the code from the two object files into an output file and replaces the stub with the final location of the function or variable. This way, your code in main.cpp can call functions and use variables in  **class.cpp**  IF AND ONLY IF THEY ARE DECLARED IN  **class.h**." 

\- Summerlin. 
[full explanation](https://stackoverflow.com/questions/3246803/why-use-ifndef-class-h-and-define-class-h-in-h-file-but-not-in-cpp)

### `.setfill(char c)`
This function modify the stream filler character to "c" (by default, the filler character is whitespace (recall the function `.setw(2)`that allocate 2 places for the string to the left of this operator and to the right, if no sufficient string to fill up, it will fill it with whitespace instead). 
This function is also a type of *sticky* setting.

### Terminology notes
In the book "How to program C++ 8th Edition" by Paul and Harvey Deitel. 
##### Class Definition
Refers to the header file, `.h` file of a class.

### Size of objects
The size of object of any class, is only contributed by its class members such as public and private variables. The reason why member functions of a class do not contribute to an object's size is due to the fact that member functions are *unmodifiable*, therefore regardless of how many objects of a class are instantiated, only a copy of the member functions will be made and shared among different objects instantiated; however the class member variables are totally modifiable and may be unique to each object instantiated, hence they requires their own copy. 
Concretely, given a class `class A`, 
```cpp
class A{
	int var1, var2, var3;

	void func1(){
		cout << "Function." << endl;
	}
}
```
let's say we create 10 objects of `class A`, each object will have their own copy of `var1`, `var2` and `var3` (so in memory sense, we have allocated $$10 \times 3 \times sizeof(int)$$ amount of memory), however the member function `func1` will only be declared once and reside somewhere in the memory, each subsequent call to the function by each objects of `class A` will refer to that particular `func1`.

### Ampersand Operator (&)
The ampersand operators are commonplace in C++ programs, its meaning varies depending on its location in the code
1. **In front of a variable name**
This particular scenario will refers to the address of that variable. Concretely:
	```cpp
	int g;
	int* gPtr = &g;
	```
	The operation of `&g` will return the address of the variable `g`.
	
2. **In variable initialization**
Ampersand is also used in variable initialization, in particular it is being placed in between the variable type and variable name during initialization to indicate the initialized variable is a *reference* to another variable of the same type. Concretely
	```cpp
	int g;
	int& gReference = g;
	```
	At the second line, the ampersand operator tells the compiler that the variable `gReference` is a *reference* to a variable of type `int`, which is actually variable `g`.

3. **In function parameters**
Ampersand can also be seen in functions parameter list. In this case, they are asking the compiler to pass value by reference. For example:
	```cpp
	void func1(int& g){
		g = 1;
	}

	int main(){
		int x = 100; // x is 100
		func1(x); 
		// x is 1 after `func1` returns.
	}
	```
	In this case, the function will modify the value of variable `g` and the changes will reflect to the original passed variable, which is essentially what a "pass by reference" is. Notice also how it is different from passing the pointer of a variable, in that we don't have to use the operator star (*) again to get the content of the variable `g`.

### On initializing
[Comprehensive explanation](https://www.learncpp.com/cpp-tutorial/8-5a-constructor-member-initializer-lists/)
There are several ways to initialize variables:
1) **Copy initialization**
	```cpp
	int a = 1;
	```
2) **Direct initialization** (a.k.a. *member initializer syntax*) 
*note: this initialization is needed for constant class member*
	```cpp
	int a (1);
	```
3) **Uniform initialization**
	```cpp
	int a {1};
	```

### Translation Units, Internal Linkage, External Linkage

**Translation unit** is "any preprocessed source file". In our `.cpp` implementation, we will have some preprocessing command such as `#include`, `#ifndef`, `#define` and etc at the top most of our `.cpp` implementation. At the end of linking, a *translation unit* will have all the contents that are modified by the preprocessing step in the `.cpp` file. For more information, refers to this [link](https://www.tutorialspoint.com/What-is-a-translation-unit-in-Cplusplus).

**Internal Linkage** refers to everything only in scope of a *translation unit*

**External Linkage** refers to things that exist beyond a particular translation unit, which is **accessible through the whole program**; in other word, the combination of all translation unit.

### On the difference between `static const int a = 5;` and `const int a = 5;`

TODO: Read and digest [this](http://www.goldsborough.me/c/c++/linker/2016/03/30/19-34-25-internal_and_external_linkage_in_c++/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjEwNDk4ODE3MV19
-->