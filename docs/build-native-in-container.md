# LINUX ONLY - documentation in progress

# Native build in container

Build your project using VSCode on your computer in a docker image.  

---
Pros:  

* Easy to use  
* Ready for development with IDE  

Cons:

* Must install docker dependencies on your computer and must hve permission to build docker image.

---

## Dependencies

Install the following packages:

* git
* vscode
* docker

The docker image is based on fedora 35.
ARM compiler version is 11.2 in the container for enabling the latest C++20 features. 
JLink version is V768c.
STLink and OpenOCD are from fedora repository. 

**NOTE**. If your cmake fails to "compile a simple test program" while running the example, then you might not have `newlib` installed (one of the errors I have encountered). Some distribution packages carry the package separately.  

## Workflow

All actions are executed with the first part of the provided [Makefile](../Makefile).  
Following targets are available:  

**NOTE**. Before using the vscode in devcontainer, perform the make command in the terminal then one can program in vscode as usual.

`make -j<threads>`: build project.  
`make cmake`: (re)run cmake.  
`make build -j<threads>`: same as `make`.  
`make clean`: remove build folder.  

The included Makefile is used more as a helper script to avoid long repetitive commands. Plus you can add other useful targets, such as for formatting and flashing, where flashing part depends on a finished build. Quite nice.  

If you want to use `CMake` only, let's say for an IDE, then all you need to do is:  

* Generate cmake project (minimum):

    ```shell
    cmake -G "Unix Makefiles" -B build -DPROJECT_NAME=firmware -DCMAKE_TOOLCHAIN_FILE=gcc-arm-none-eabi.cmake -DCMAKE_BUILD_TYPE=Debug
    ```

* Compile project:

    ```shell
    # Directly with make
    make -C ./build -j<threads>

    # Indirectly with cmake
    cmake --build ./build -j<threads>
    ```

The indirect build command using cmake is convenient, as you don't need to know which build system is behind. CMake will call the one you have configured with first command. In this repository, this means `gnumake`.  
