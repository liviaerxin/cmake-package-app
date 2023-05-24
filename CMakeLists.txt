# CMake simple example

# # [main]

cmake_minimum_required(VERSION 3.1...3.26)

project(
    ModernCMakeExample
    VERSION 1.0
    LANGUAGES CXX)

# Set using shared libs
set(BUILD_SHARED_LIBS TRUE)

if(MSVC)
    # Create dlls on Windows
    set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS TRUE)
endif()

if(APPLE)
    set(CMAKE_MACOSX_BUNDLE ON)
endif()

message(STATUS, "CMAKE_MACOSX_BUNDLE=${CMAKE_MACOSX_BUNDLE}")

# If you set any CMAKE_ variables, that can go here.
# (But usually don't do this, except maybe for C++ standard)

# Set default `CMAKE_INSTALL_PREFIX` to ${PROJECT_SOURCE_DIR}/install as this is an application-based standalone project.
if(CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)
    cmake_path(SET install_path ${PROJECT_SOURCE_DIR}/install)
    set(CMAKE_INSTALL_PREFIX ${install_path} CACHE PATH "Force path to set CMAKE_INSTALL_PREFIX" FORCE)
    message(STATUS "CMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX}")
endif()

message(STATUS, "PROJECT_BINARY_DIR=${PROJECT_BINARY_DIR}")

# Find packages go here.

# You should usually split this into folders, but this is a simple example

# This is a "default" library, and will match the *** variable setting.
# Other common choices are STATIC, SHARED, and MODULE
# Including header files here helps IDEs but is not required.
# Output libname matches target name, with the usual extensions on your system

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

# This part is so the Modern CMake book can verify this example builds. For your code,
# you'll probably want tests too
enable_testing()

add_test(NAME MyExample COMMAND MyExample)

install(TARGETS MyExample
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
    RUNTIME DESTINATION bin
    BUNDLE DESTINATION .
)

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