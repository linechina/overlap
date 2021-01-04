cmake_minimum_required(VERSION 3.10)
project(overlap LANGUAGES CXX)

list(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake)

# Header-only library.
include_directories(${PROJECT_SOURCE_DIR})

# Add compiler flags for C++11.
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Set up the Eigen3 library.
find_package(Eigen3 3.2.0 NO_MODULE)
if(NOT TARGET Eigen3::Eigen)
    message(STATUS "Failed to find Eigen3 >= 3.2.0, using internal version")

    add_library(Eigen3::Eigen IMPORTED INTERFACE)
    set_property(TARGET Eigen3::Eigen PROPERTY INTERFACE_INCLUDE_DIRECTORIES
                 "${PROJECT_SOURCE_DIR}/third_party/eigen")
endif()

# Activate the unit tests.
option(OVERLAP_TESTS "Build the tests" ON)
if(OVERLAP_TESTS)
    enable_testing()
    add_subdirectory(test)
endif()

# Build the Python bindings using pybind11.
option(OVERLAP_PYTHON "Build the Python bindings" ON)
if(OVERLAP_PYTHON)
    add_subdirectory(python)
endif()
