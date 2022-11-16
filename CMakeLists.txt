
cmake_minimum_required(VERSION 3.24)

project(libsofa LANGUAGES CXX)

#option(BUILD_SHARED_LIBS "Building libsofa by default as STATIC" OFF)
add_subdirectory(src)

# If we are in the root, define additional targets 
# See https://www.foonathan.net/2022/06/cmake-fetchcontent/ 
if (CMAKE_CURRENT_SOURCE_DIR STREQUAL CMAKE_SOURCE_DIR)
    option(LIBSOFA_BUILD_UTILS  "whether or not sofa2json, sofaexamples, etc should be built" ON)
endif()

if (LIBSOFA_BUILD_UTILS)
    add_subdirectory(utils)
endif()

