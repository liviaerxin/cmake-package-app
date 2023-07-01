# CMake + CPack + CI/CD Standalone Application on Windows, OSX

Package a standalone and relocatable application on Windows and OSX, and use Github actions CI/CD

A streamline workflow demonstrate how CMake helps to build, install and package an relocatable executable in windows, osx.

Keys:

- CMake
- CPack
- MacOS X bundle
- Shared Library
- Static Library
- Standalone Application

Features:

- In OSX: install an executable as bundle, and then package it as *.dmg.
- In Windows: install an executable as exe, and then package it and its dependencies as *.zip.

## How CMake Build, Install and Package

We see a project folder structures as an `Interface`.

Then there's three `Interfaces`:

**source and build interfaces:**

```sh
project_root
├── CMakeLists.txt
├── simple_example.cpp
├── simple_lib.cpp
├── simple_lib.hpp
└── build
    └── CMakeCache.txt
```

**install interface:**

This interface is located in the `CMAKE_INSTALL_PREFIX`, and its default value in this project is  `${project_root}/install`.

Also, you can override it by specifying `-DCMAKE_INSTALL_PREFIX` when running `cmake -B build -S .` or `--prefix` when running `cmake --install`.

This argument path can be relative path.

```sh
cmake --install build --prefix "/where/I/want/it/to/go"
```

It's recommended to use a default install layout using `GNUInstallDirs`.

```sh
project_root
├── CMakeLists.txt
├── simple_example.cpp
├── simple_lib.cpp
├── simple_lib.hpp
├── build
│   └── CMakeCache.txt
└── install
    ├── bin
    │   └── executables
    ├── sbin
    │   └── sysadmin executables
    ├── lib
    │   ├── compiled libraries (*.so (unix) or *.dll (windows))
    │   └── library archive files (*.lib (windows))
    ├── libexec
    │   └── executables not directly invoked by user
    ├── include
    │   └── header files
    └── doc
       └── documentation
```

## Get Started

```sh
# 1. Configure
# BUILD_SHARED_LIBS default is `ON`, `CMAKE_BUILD_TYPE` default is `debug`
cmake -B build -S .
cmake -B build -S . --fresh
cmake -B build -S . -DBUILD_SHARED_LIBS=OFF

# 2. Build artifacts in `./build` folder
cmake --build build --config debug

# 3. Install artifacts into default folder `./install`, or change it via `--prefix`
cmake --install build --config debug

# 4. Package
# Windows: produce a zip for the standalone app
cpack --config ./build/CPackConfig.cmake -G ZIP -C debug

# OSX: produce a *.dmg for the standalone bundle
cpack --config ./build/CPackConfig.cmake -G DragNDrop -C debug
```

## References

[c++ - What install command does in cmake? - Stack Overflow](https://stackoverflow.com/questions/53121491/what-install-command-does-in-cmake)

[c - CMAKE - How to properly copy static library's header file into /usr/include? - Stack Overflow](https://stackoverflow.com/questions/10487256/cmake-how-to-properly-copy-static-librarys-header-file-into-usr-include)

[c++ - How to configure CMakeLists.txt to install public headers of a shared library? - Stack Overflow](https://stackoverflow.com/questions/54271925/how-to-configure-cmakelists-txt-to-install-public-headers-of-a-shared-library)

[C++ project structure and CMake for cross-platform build | The Startup](https://medium.com/swlh/c-project-structure-for-cmake-67d60135f6f5)

[c++ - Overriding a default option(...) value in CMake from a parent CMakeLists.txt - Stack Overflow](https://stackoverflow.com/questions/3766740/overriding-a-default-option-value-in-cmake-from-a-parent-cmakelists-txt)