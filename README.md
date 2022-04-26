
<h1 align="center">Executable File Bootloader</h3>

<p align="justify"> 
The task was to implement an Executable File Bootloader as a dynamic library for UNIX. The loader would load the binary executable in the main memory, page by page, using the demand-paging mechanism. For the Bootloader to properly function, it has to take into account some of the following constraints: address space, page access privileges, the format of executable files, correctly implement demand-paging, correctly signal page-faults, and map files in the process address space (file mapping). Implemented in C, for POSIX & WIN32 API.
    <br> 
</p>

## 📝 Table of Contents
- [About](#about)
- [Getting Started](#getting_started)
- [Usage](#usage)
- [Built Using](#built_using)

## 🧐 About <a name = "about"></a>
The interface of the loader is presented in the header loader.h file. This has functions to initialize a loader (so_init loader) and execute a binary (so_execute)

* The so_init_loader function performs the library initialization. Within the function, the page fault action will be a routine for handling the **SIGSEGV** signal.
* The so_execute function performs the parsing of the binary specified in the path and executes the first entry point of the executable.

## 🏁 Getting Started <a name = "getting_started"></a>
These instructions will get you a copy of the project up and running on your local machine for development and testing purposes.

### Prerequisites
To use the loader.h library on your projects you must have:

* for UNIX based operating systems:
  * gcc - is a tool from the GNU Compiler Collection used to compile and link C programs

### Installing
This is a step by step series of examples that tell you how to get a development env running.

* Linux:
  * Start by updating the packages list
    ```bash
    $sudo apt-get update
    ```
  * Install the build-essential package(a package of new packages including gcc, g++ and make) by typing:
    ```bash
    $sudo apt-get install build-essential 
    $sudo apt-get install gcc-multilib  
    ```
## 🔧 Running the tests <a name = "tests"></a>
If you want to run the automated tests for Linux system you must follow the following steps:
* Clone the repository by copping the following command in your terminal:
  ```
  git clone https://github.com/the-sergiu/Executable-File-Bootloader-in-C
  ```

* Go into the repo directory and run the following commands:
```bash
cd src
make -f Makefile
make -f Makefile.checker

```
  
* The results of the tests will be shown in the terminal. To delete the newly created files, run:
```bash
make -f Makefile.checker clean
make -f Makefile clean
```

* Your source check test (for indentation and coding style) may fail due lack of setup. Don't worry, it's non-essential. 

## 🎈 Usage <a name="usage"></a>
If you want to use the ***libso_loader.so*** library in your projects then you must add the ***loader.h*** header in the desired source file and specify at the compile time the path to the libso_loader.so library.

* running the following command in the project director will generate the libso_loader.so:
  ```bash
  make build
  ```
## ⛏️ Built Using <a name = "built_using"></a>
- [Visual Studio Code](https://code.visualstudio.com/) - code editor
- [gcc](https://gcc.gnu.org/) - used to compile the library on my Linux machine

Link to assignment in Romanian: https://ocw.cs.pub.ro/courses/so/teme/tema-3
