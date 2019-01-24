# Arrays and Vectors

### Array initialization
A simple array initialization follows the following format:

*type variable_name[size];*

For example:
```cpp
int intArr[12];
```
This will initialize `intArr` as an array of integers with 12 elements.

### Dynamic array size
In normal circumstances, dynamic array size must be achieved by using `malloc` or `new`. However, in C99 standard, the following is acceptable

```cpp
int main(){
	int size;
	cin >> size;
	int intArr[size];
}
```

Notice how the array size is not determined at compilation time, but during runtime. In the book "How to Program C++ 8th Edition" by Paul and Harvey Deitel, it mention that this should be a compilation error (pg.307). 
However as I was trying to compile this code using [online compiler](https://www.onlinegdb.com/online_c++_compiler), it surprisingly compiled and ran without any issue. Further digging into this topic leads to the following [discussion](https://stackoverflow.com/questions/737240/array-size-at-run-time-without-dynamic-allocation-is-allowed), which stated that 

> "This is known as VLAs (variable length arrays). It is standard in c99, but gcc allows it in c++ code as an extension. "

### Passing array to function
The primitive array (which was inherited from C language), when passed to function, requires user to pass its number of element as input as well, so the function know the bounds of array. For instance:

```cpp
void arrayMod(int array[], int noElems){
	while(noElems--){
		array[noElemns] += 1;
	}
}
```

The first parameter is the array we want to modify, and the second parameter is the number of elements exists in this array. Notice how without the second parameter, the function will not know when to stop processing.

Also note that passing array to function is actually **pass by reference**. To prevent function code from modifying the value in an array, the *safe* way of passing by reference can be used as well, which is:

```cpp
void arrayMod(const int array[], int noElems){

}
```

### std::vector
Vectors is known officially as a "contiguous dynamic array" that act somewhat like a "list" item in Python. To use it user need to include the following header
```cpp
#include <vector>
```

#### Initialization
The initialization of vector takes the following form,

```cpp
/* Initialization 1: Empty vector */
vector<int> listOfIntegers1;
/* Initialization 2: Vector of 4 elements, each element has value of 0 */
vector<int> listOfIntegers2 (4, 0);
/* Initialization 3: listOfIntegers3 is a copy of listOfIntegers2 */
vector<int> listOfIntegers3 (listOfIntegers2);

/* Initialization 4: Vector based on existing array */
int[] array = {1,2,3,4,5};
int endOfArray = array + sizeof(array) / sizeof(int);
vector<int> listOfIntegers4 (array, endOfArray); 
```

#### Addressing 
There are two type of addressing mechanism, 
- Square bracket `[ ]`
- Member function `.at()`
For instance
```cpp
vector<int> myVec (4,0);

/* Accessing the element at index 1 */
int elemAt1;
elemAt1 = myVec[1]; 
elemAt1 = myVec.at(1);
```

The code snippet above demonstrate the two different way of addressing the element at index = 1 at our vector. 
The real difference between the two operators lie in the way they handle **out of bound** index. 
- Square bracket `[ ]` operator do not raise an error when you attempt to access out of bound index.
- The function `.at()` will raise an error if there is an attempt to access out of bound error.

Again using the same example:
```cpp
vector<int> myVec (4,0);

/* Accessing the element at index 1 */
int elemAt1;
elemAt1 = myVec[5]; // No error raised.
elemAt1 = myVec.at(5); // Will raise error.
```

#### Appending
To append the vector, simply use the member function `.push_back()`.
Concretely,

```cpp
vector<int> myVec (4,0);

myVec.push_back(1);

int elemsAt4 = myVec.at(4);
// elemsAt4 is equals to 1.
```

#### Clearing a vector
Clearing a vector is not as trivial in C++ if memory footprint is of utmost importance to you. 

##### `.clear()`
The official member function to clear a vector is its member function `.clear()`

```cpp
vector<int> myVec (4,0);
myVec.clear()
```

At this point, the elements in the vector `myVec` will be "destroyed", but the memory allocated for this particular vector has not been freed. To truly release the memory reserve, it needs to be follow up by a call to another member function `.shrink_to_fit();`
Concretely,

```cpp
vector<int> myVec (4,0);
myVec.clear()
myVec.shrink_to_fit();
```

As the gist of it, you need to first "destroy" the elements of the vector with function `.clear()`, and then call the function `.shrink_to_fit()` in order to freed up the memory held by the vector previously.

##### `.swap()`
Alternatively, a clever trick used by the community involve "swapping" the content of the vector with the content of another "temporary" vector, which is left unused and its content/memory will be collected by garbage collector mechanism.

```cpp
vector<int> myVec (4, 0);
vector<int>().swap(myVec);
```

This one step function call first create a temporary vector of same type (`int`), and then it "swap" its content with the content of `myVec`. 

### Iterating
To iterate the element in the vector, the primitive C++ `for` loop can be used, where the number of elements can be obtained using member function `.size()`. However, a simpler approach (similar to Python) to iterating elements in vector is called *"Range Based For Loop"*. 
Concretely, instead of 
```cpp
vector<int> myVec (4,0);
for (int i = 0; i < myVec.size(); i++){
	cout << "Current element: " << myVec[i] << endl;
}
```

We can do

```cpp
vector<int> myVec (4,0);
for (int currElem : myVec){
	cout << "Current element: " << currElem << endl;
}
```


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTg0MDE3NzkyNF19
-->