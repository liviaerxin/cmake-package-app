# A CMake Example to Package Standalone Application in Windows, OSX

A streamline workflow to demonstrate how CMake helps to build, install and package an executable in windows, osx.

Tags: CMake, CPack, MacOS X bundle.

In OSX: install an executable as bundle, and then package as *.dmg.

In Windows: install an executable as exe, and then package as *.zip.

The project folder structures are seen as an "Interface". Then there's three interfaces:

**source and build interfaces:**

```sh
project_root
├── CMakeLists.txt
├── src
│   └── CMakeLists.txt
├── build
│   └── CMakeCache.txt
```

**install interface:**

When the `CMAKE_INSTALL_PREFIX` is not provided and this project will use the default `${project_root}/install`.

```sh
# use the default CMAKE_INSTALL_PREFIX=${project_root}/install
cmake --install build
```

Also, you can override it by specifying `-DCMAKE_INSTALL_PREFIX` when running `cmake -B build -S .` or `--prefix` when running `cmake --install`.

This argument path must be absolute path.

```sh
cmake --install build --prefix "/where/I/want/it/to/go"
```

It's recommended to use a default install layout using `GNUInstallDirs`.

```sh
project_root
├── CMakeLists.txt
├── src
│   └── CMakeLists.txt
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

windows:

```sh
cmake -B build -S .

cmake --build build --config debug

cmake --install build --config debug

# windows
cpack --config ./build/CPackConfig.cmake -G ZIP -C debug

# osx
cpack --config ./build/CPackConfig.cmake -G DragNDrop -C debug
```


[c++ - What install command does in cmake? - Stack Overflow](https://stackoverflow.com/questions/53121491/what-install-command-does-in-cmake)

[c - CMAKE - How to properly copy static library's header file into /usr/include? - Stack Overflow](https://stackoverflow.com/questions/10487256/cmake-how-to-properly-copy-static-librarys-header-file-into-usr-include)

[c++ - How to configure CMakeLists.txt to install public headers of a shared library? - Stack Overflow](https://stackoverflow.com/questions/54271925/how-to-configure-cmakelists-txt-to-install-public-headers-of-a-shared-library)

[C++ project structure and CMake for cross-platform build | The Startup](https://medium.com/swlh/c-project-structure-for-cmake-67d60135f6f5)