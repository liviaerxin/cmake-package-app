# A CMake Example to Build, Install and Packaging Your Application

A streamline workflow to demonstrate how CMake helps to build, install and package an executable program.

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

cpack --config ./build/CPackConfig.cmake -G ZIP -C debug
```


[c++ - What install command does in cmake? - Stack Overflow](https://stackoverflow.com/questions/53121491/what-install-command-does-in-cmake)