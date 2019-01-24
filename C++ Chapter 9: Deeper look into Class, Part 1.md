# Deeper look into Classes

### Class scope and Accessing class members

There are several ways to access class members
##### Private class members
Can only be accessed internally by other class members

##### Public class members
1. Dot member selection operator
Dot member selection operator is used directly on an object to access its public member. For example, 
	```cpp
	someObject obj;
	obj.var1 = 10;
	```
	As it can be seen from the code snippet above, to access the class member `var1`, we use the *dot* operator on the instantiated object `obj`.
	
2. Arrow member selection operator
Arrow operator is used when a pointer to that object is given. Concretely,
	```cpp
	someObject* objPtr;
	objPtr->var1 = 10;
	```
	Here the `objPtr` is a *pointer* pointing to the instantiated object of the class `someObject`. Using the arrow operator, we can straightaway access the class member `var1`, which is also equivalent to the more verbose operator:
	```cpp
	(*objPtr).var1 = 10;
	```
	Both are valid and equivalent.
	

### Constant objects
Objects of any classes can be declared *const* to indicate that its value shouldn't be modified. For instance,
```cpp
const Time noon(12,0,0);
```
The code snippet above declared a constant `Time` object, `noon` which have class member variables hour = 12, minute = 0, second = 0. 

Problem with constant object is that invoking non-constant member function, **despite it not changing any member value (such as `.get`)** is a compilation error. 

Functions such as `.getHour()`, `printTime()` are all not modifying the value of an object, therefore should be allowed to be invoked by even constant objects. To overcome this, we need to declare *explicitly* those function is **constant** using the keyword `const`. 
Concretely,

```cpp
/* Protoypes */
int getHour() const;
int getMinute() const;
int getSecond() const;
void printUniversal() const;

/* Definitions */
int getHour() const{
	return hour;
}
```

Notice how the `const` keyword appears after the function parameters list and both *prototype* and *definition* should have that keyword.

*Note: constructors and deconstructors must be non-const function as they needs to be able to modify the values of member function, despite the object being a const object.*

It is usually a good practice to declare all the member functions that are not modifying the class member as `const`. 

### Constant class members
Constant class members and references (generally variable with unchangeable values) requires **member initializer syntax**. Recall that we have several ways to initialize variables:
1) Copy initialization
	```cpp
	int a = 1;
	```
2) Direct initialization (a.k.a. *member initializer syntax) 
	```cpp
	int a (1);
	```
3) Uniform initialization
	```cpp
	int a {1};
	```

If there is a constant member in a class, the constructor needs to initialize the member with the *direct initialization* method. Concretely,

```cpp
class GradeBook{
private:
	const string courseName;
	const string instructorName;
public:
	GradeBook(string name, string instructor){
		courseName = name;
		instructorName = instructor;
	}
}
```

The code snippet above is invalid, because technically, assignment operator cannot be used on a `const` variable if it is not in the same line of assignments. 
```cpp
const int i = 10; // correct
const int g;
g = 100; // in valid
```

Instead, the constructor need to initialize the constant member with *direct initialization*. Concretely,

```cpp
class GradeBook{
private:
	const string courseName;
public:
	GradeBook(string name, string instructor) : courseName(name), instructorName(instructor) {
	// empty block because we don't need anything else here.
	} 
}
```

Notice the *direct initializations* are done outside of the constructor code block, at the end of parameter list with a colon in between the parameter list and the *direct initialization*.


<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQwMjA1MTAxM119
-->