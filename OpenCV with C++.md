# OpenCV, with C++, and Mac OS X (Mojave)
Building from source, using vanilla `g++` command on terminal, without the fancy IDE. 

### 0. Preliminaries
OpenCV is implemented in C++ with a variety of wrappers offered. Which means, its library are all implemented with C++ and linkage must be done properly during build process or it won't compile.

### 1. Installation
Installing OpenCV more tedious than expected. The steps can be summarized as: first getting the source code and extracting it, and then proceed to configure the *build* with several parameters to suit one's need. Once configured, *make* process can be initiated and with the build done, installation of library can be made. 

Do note that this installation process is written in Mac OS X (Mojave) in mind, therefore it might have varies between different operating system.

This guide based largely on the [official guide](https://docs.opencv.org/3.4/d7/d9f/tutorial_linux_install.html) on installation for Linux, in combination with a lot of stack overflow queries due to some corner case issue.

#### 1.1 Getting the source
There are few ways of getting the source code. Since we are building it ourselves, it might be better to get the latest stable release directly from their github site. With `git` installed, perform the following command at your terminal

```bash
mkdir ocv_source
cd ocv_source
git clone https://github.com/opencv/opencv.git
git clone https://github.com/opencv/opencv_contrib.git
```

In the `ocv_source` directory, you will have two folder: `opencv` and `opencv_contrib`. 

#### 1.2 Configuring build
Directly from step 1.1, perform `cd` to go into the `opencv` folder 

```bash
cd opencv
```
and create an empty folder `build` to store the build configuration files and build files.

```bash
mkdir build
cd build
```

Now go into the `build` directory, and run the following cmake command in order to configure your build.

```bash
cmake -D CMAKE_BUILD_TYPE=Release -D CMAKE_INSTALL_PREFIX=/usr/local ..
```
*Note: the double dots at the end tells cmake our source are located at parent directory (recall `cd ..` goes back one folder up).*

For the list of build options, refers to the following [link](https://stackoverflow.com/questions/16851084/how-to-list-all-cmake-build-options-and-their-default-values) on how to list all parameters. 

Once it has been properly configured, we are ready to build it.

#### 1.3 Build 
To initiate the build after it has been configured, invoke the command
```bash
make -j4
```
The numeric value 4 means to try and utilise 4 CPU cores to build in parallel.

This build process takes a long time depending on your computer. 

#### 1.4 Installing the build
When that's done, simply invoke the following function to install the libraries
```bash
sudo make install
```

#### 1.5 Verifying the installation
At the end of the installation, you can verify whether the installation is successful by first checking the console output to ensure it did not raise any error.
Next, simply go into the installation directory (which is specified by user during the configuration stage, under the variable `CMAKE_INSTALL_PREFIX=/usr/local`). In that directory, you will have at least two folders: `include` and `lib`. 
##### Header files
In the `include` folder, you should be able to see a folder `opencv` (it can be `opencv3` or `opencv4` depending on the version you installed) and inside of which will contains all the header files `.h` that you will need to include in your C++ code. 
##### Library files
In the `lib` folder, you can see there are numerous `.dylib` files that are in the format of `libraryopencv_xxxxx.dylib`, which is the library files of implemented opencv functions.

#### 1.6 Installation FAQ

### 2. Compiling and Linking first OpenCV2 program

Since this guide aims at compiling C++ programs using the good old `g++` terminal option, proper linkage must be done so that libraries and headers are included properly. 

#### 2.1 Some Codes
To visualize the process, a toy example code has been written as the following and saved as **display_image.cpp**.
```cpp
#include <iostream>
#include <opencv2/opencv.hpp>

int main(){
	cv::Mat image = cv::imread("/Users/MJ/git/cpp/opencv/images/maxresdefault.jpg"); // User to change the directory to image to their own.
	if (iamge.empty()){
		std::cout<<"Failed to load image."<<std::endl;
		return -1;
	}
	std::string windowName = "opencv_test";
	cv::namedWindow(windowName);
	cv::imshow(windowName, image);
	cv::waitKey(0);
	cv::destroyWindow(windowName);
	return 0;
}
```

The code itself should be pretty straightforward: An image "maxresdefault.jpg" is being loaded using the function `cv::imread` into a `cv::Mat` object named `image`. Then a condition is set up to ensure image is properly loaded. To display the image, a window needs to be created using the function `cv::namedWindow` and to actually display it the function `cv::imshow` is invoked. `cs::waitKey(0)` is invoked so that the window with image displayed will stay up until user enter any keys, which will then cause the window to be destroyed by the function `cv::destroyWindow`

#### 2.2 Compiling and Linking
Recall that in C++, the header `.h` files only contains the function prototypes, and properly linkage must be establish to include the libraries files (in MacOSX they are `.dylib` files residing in the installation directory). Concretely, to compile the above code so that it can be executed, the following command should be used:

`g++ -std=c++11 -I /usr/local/opencv4 -L /usr/local/lib -lopencv_core -lopencv_highgui -lopencv_imgcodecs -o exec display_image.cpp`

That's one lengthy command, let's dissect it one by one:

##### `g++`
Basically invoke the GCC compilation tools.

##### `-std=c++11`
Telling the compiler to use C++11 standard when compiling.

##### `-I /usr/local/opencv4`
This option instruct the compiler to look for header file at the specified directory as well, since that's where I installed my header files.

##### `-L /usr/local/lib`
This option tell the compiler that this is the root directory that will contains the *library* files that we will want to include in this compilation.

##### `-lopencv_core, -lopencv_highgui -lopencv_imgcodecs`
This `-l` options tell compiler out of all the libraries in the directory specified by `-L`, which one do we want to include. If you head to the library installation path (in my case it is in `/usr/local/lib`), you will see a lot `.dylib` files, look for those with the general pattern of `libopencv_xxx.dylib`, those are the opencv libraries files to be included in your OpenCv codes. For instance, the library `opencv_imgcodecs.dylib` contains the implementation for function `cv::imread`. If that particular library is not specified explicitly during compilation like the command above did, error will be thrown:
> `Undefined symbols for architecture x86_64:
>  "cv::imread(std::__1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char> > const&, int)", referenced from:
> _main in display_img-d21f21.o
> ld: symbol(s) not found for architecture x86_64`

If the compilation runs successfully, executing the output file (in this case, a file named `exec`) will produce a window with image displayed.

### 3. References
[On the correct g++ command to link libraries and header files](http://answers.opencv.org/question/25642/how-to-compile-basic-opencv-program-in-c-in-ubuntu/)
[Automate the compilation process using CMake](https://docs.opencv.org/2.4/doc/tutorials/introduction/linux_gcc_cmake/linux_gcc_cmake.html#linux-gcc-usage)
[Writing a bash script to automate compilation process](https://help.ubuntu.com/community/OpenCV)
<!--stackedit_data:
eyJoaXN0b3J5IjpbNDUwNjUzNDU5XX0=
-->