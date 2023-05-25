# CMake simple example
cmake_minimum_required(VERSION 3.1...3.26)

project(
    ModernCMakeExample
    VERSION 1.0
    LANGUAGES CXX)

# If you set any CMAKE_ variables, that can go here. (But usually don't do this, except maybe for C++ standard)

# Set default cmake-variables if they are not passed/set from CLI. see in `CMakeCache.txt`
# (When using `option(), these variables must not be set as a normal or cache variable)
# set(BUILD_SHARED_LIBS ON CACHE BOOL "Build libfoo shared library")
option(BUILD_SHARED_LIBS "Default BUILD_SHARED_LIBS to ON" ON)

if(MSVC)
    # Create dlls on Windows
    option(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS "Default CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS to ON" ON)
endif()

if(APPLE)
    option(CMAKE_MACOSX_BUNDLE "Default CMAKE_MACOSX_BUNDLE to ON" ON)
endif()

# Set default `CMAKE_INSTALL_PREFIX` to ${PROJECT_SOURCE_DIR}/install as this is an application-based standalone project.
# (`option()` will not work as `CMAKE_INSTALL_PREFIX` is already cached!)
if(CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)
    set(CMAKE_INSTALL_PREFIX ${PROJECT_SOURCE_DIR}/install CACHE PATH "Force path to set CMAKE_INSTALL_PREFIX" FORCE)
endif()

message(STATUS "BUILD_SHARED_LIBS=${BUILD_SHARED_LIBS}")
message(STATUS "CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS=${CMAKE_WINDOWS_EXPORT_ALL_SYMBOsLS}")
message(STATUS "CMAKE_MACOSX_BUNDLE=${CMAKE_MACOSX_BUNDLE}")
message(STATUS "PROJECT_BINARY_DIR=${PROJECT_BINARY_DIR}")
message(STATUS "CMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX}")

# Find packages go here.

# You should usually split this into folders, but this is a simple example
# Pass library type in CLI(STATIC, SHARED, and MODULE)

# Add library
add_library(MyLibExample)

target_include_directories(MyLibExample PRIVATE ${PROJECT_SOURCE_DIR})

target_sources(MyLibExample
    PRIVATE simple_lib.cpp
    PUBLIC FILE_SET HEADERS # For copy header files to `include/`
    BASE_DIRS ${PROJECT_SOURCE_DIR}
    FILES simple_lib.hpp
)

install(TARGETS MyLibExample
    LIBRARY DESTINATION lib # No need actually
    ARCHIVE DESTINATION lib # No need actually
    RUNTIME DESTINATION bin # No need actually
    FILE_SET HEADERS DESTINATION include # Key need actually
)

# Add executable
add_executable(MyExample simple_example.cpp)

target_link_libraries(MyExample MyLibExample)

install(TARGETS MyExample
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
    RUNTIME DESTINATION bin
    BUNDLE DESTINATION .
)

# Detect `.app` bundles on OSX and make it standalone after install
if(APPLE)
    # Note Mac specific extension .app
    set(APPS ${CMAKE_INSTALL_PREFIX}/MyExample.app)

    # Directories to look for dependencies
    set(DIRS ${CMAKE_INSTALL_PREFIX}/lib)

    message(STATUS "APPS=${APPS}")
    message(STATUS "DIRS=${DIRS}")

    install(CODE [[
        include(BundleUtilities)
        fixup_bundle("${APPS}" "" "${DIRS}")
    ]])
endif()

# Package management
include(CPack)