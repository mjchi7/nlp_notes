# Functions
This chapter dives into the *function* concept. In it, concept like *storage class*, *scopes*, *call by reference, values* and etc. are also covered. 

### Storage Classes
Storage classes determine the characteristic of a variable in term of its lifetime in memory throughout the execution of program.
#### `auto`
Example:
```cpp
auto int myVar;
```
The modifier `auto` are only allowed to be placed in front of a *local* variable. `auto` variable will be intialized when the function or scope it resides in are being access, and memory will be released when the function or scope return. 
*note: by default all the local variable in a functions are `auto`.*
###### second meaning of keyword `auto`
The qualifier `auto` has another meaning in different context:
```cpp
auto varName = 8;
```
It allows compiler to deduce the *type* of variable from its initialization method. In the case above, `varName` will be an *integer* type due to the initialization value.

#### `register`
Example: 
```cpp
register int counter = 10;
```
The `register` qualifiers will recommend the compiler to put the said variable into the computer high speed register. This is particularly useful for variables that are frequently accessed. However, as compilers nowadays does this on their own, this qualifiers are seldom used.

#### `static`
Example:
```cpp
void incCount(){
	static int count = 0;
	count += 1;
	return count;
}
```

The qualifier `static` when used on variables in function, will cause the value of that variable to be *static* during different call of that function. In another word:
```cpp
int main(){
	int countVal;
	countVal = incCount();
	cout << "countVal: " << countVal << endl; // This will give 1;
	countVal = incCount();
	cout << "countVal: " << countVal << endl; // This will give 2;
```
Notice how if there isn't a `static` qualifier for the variable `count` in the `incCount` function, two separate call to that function will only yield the value 1.

### Pass by Reference v.s. Pass by Value

#### Pass by value
Consider the following code snippet:
```cpp
void varMod(int n){
	n = n * 100;
}

int main(){
	int n = 2;
	varMod(n);
	cout << "n after function: << n << endl; // This will be 2;
```

Notice how after we pass the variable `n` into function, the value of `n` outside of the scope of the function remains the same when it was first intialized. That's because whenever we pass a variable into a function in that manner, we are actually **passing by value**, meaning that the value of the variable will be *copied* and then pass to the function, without exposing the variable to the function's operation.

#### Pass by reference
Consider the same code snippet with a slight modification: 
```cpp
void varMod(int &n){
	n = n * 100;
}

int main(){
	int n = 2;
	varMod(n);
	cout << "n after function: << n << endl; // This will be 200;
```

Notice that in the function `varMod` now, the parameter `n` is being preceded by a `&` operator. This operator means to tell the compiler the variable `n` should be **pass by reference**; In another word, the value of `n` is not being copied, but the entire variable of `n` (actually the pointer of `n`) is being supplied to the function, so that any modification on the variable will be reflected on the original variable even after the function return. That is because the operation is being operated on the original variable.

#### Pass by reference, safely
Again consider the same code snippet with more modification:

```cpp
void varMod(const int &n){
	n = n * 100;
}

int main(){
	int n = 2;
	varMod(n);
	cout << "n after function: << n << endl; // This will be 200;
```

Notice that a keyword `const` is preceding the parameter `n` in the said function. This tell the compiler that the parameter `n` should be **constant** in the scope of the function, meaning that no operation should be allowed to modify the value of `n`.  
But why is this useful? Why don't we just pass by value? The reason behind this will become self evident when you are dealing with objects that requires a large amount of memory to hold. Recall that when you **pass by value**, a *copy* of that parameter will be made and pass to the function. If your object has rather large memory footprint, a copy of it will definitely take up twice the space. Therefore, we can use this method to **pass by reference in a safe manner**, which prevents the function from manipulating the content of variable.


### Default Arguments
In C++, the default arguments for different variables are being declared in the function prototype. For instance:

```cpp
int fixedMultiply(int n, int multiplier = 2);

int main(){
	int res = fixedMultiplier(3);
	// res = 3 * 2 = 6;
}

int fixedMultiply(int n, int multiplier){
	return n*multiplier;
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3MjY5ODg5MTFdfQ==
-->