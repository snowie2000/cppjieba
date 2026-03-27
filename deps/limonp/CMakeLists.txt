cmake_minimum_required(VERSION 3.10)
project(limonp LANGUAGES CXX VERSION 1.0.0)

# 1. Standard C++ Settings
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# 2. Find Sources
# Cleaned up the globbing logic
file(GLOB_RECURSE LIMONP_SOURCES
        "${CMAKE_CURRENT_SOURCE_DIR}/*.cpp"
        "${CMAKE_CURRENT_SOURCE_DIR}/*.c"
)

# 3. Define the target as STATIC
# This is the crucial change for Windows/MSVC compatibility
add_library(limonp STATIC ${LIMONP_SOURCES})

# 4. Include Directories
# Use CMAKE_CURRENT_SOURCE_DIR to be safe when used as a subproject
target_include_directories(limonp PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
        $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
)

# 5. Compiler Options
if(MSVC)
    target_compile_options(limonp PRIVATE /W4)
else()
    target_compile_options(limonp PRIVATE -Wall -O3)
endif()

# 6. Installation (Keep this if you need it, but usually not for subprojects)
include(GNUInstallDirs)
install(TARGETS limonp
        EXPORT limonpTargets
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)