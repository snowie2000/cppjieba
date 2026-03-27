cmake_minimum_required(VERSION 3.10)
project(cppjieba VERSION 5.0.3 LANGUAGES CXX)

# 1. Include the limonp project
# This will process deps/limonp/CMakeLists.txt
# It will create a target (likely named 'limonp')
add_subdirectory(deps/limonp)

# 2. Create the cppjieba INTERFACE library
add_library(cppjieba INTERFACE)
add_library(cppjieba::cppjieba ALIAS cppjieba)

# 3. Link cppjieba to limonp
# Note: Verify that the target name in deps/limonp/CMakeLists.txt is 'limonp'.
# If it's different, change 'limonp' here to that name.
target_link_libraries(cppjieba INTERFACE limonp)

# 4. Define include directories
# We need 'include' for cppjieba's own headers.
# We often need 'deps' because cppjieba headers do: #include "limonp/xxx.hpp"
target_include_directories(cppjieba INTERFACE
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/deps>
        $<INSTALL_INTERFACE:include>
)

# 5. Ensure C++11 is used across the board
target_compile_features(cppjieba INTERFACE cxx_std_11)