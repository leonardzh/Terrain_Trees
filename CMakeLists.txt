cmake_minimum_required(VERSION 2.8)

project(Terrain_Trees)
set(terrain_trees_VERSION_MAJOR 1)
set(terrain_trees_VERSION_MINOR 0)

#~ SET(CMAKE_MODULE_PATH ${PROJECT_BINARY_DIR}/cmake/Modules/)
#~ MESSAGE(STATUS "local cmake root: " ${CMAKE_MODULE_PATH})

set(LANGUAGE C++)
#set(CMAKE_VERBOSE_MAKEFILE on)

#~ set(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib )

set(CMAKE_BUILD_TYPE Release)

#~ find_package(Boost REQUIRED)
#~ find_package(Doxygen REQUIRED)

#if(CMAKE_COMPILER_IS_GNUCXX) #do not work on mac
if(CMAKE_CXX_COMPILER_ID MATCHES "Clang")
    set(CMAKE_CXX_FLAGS "-O3 -std=c++11 -I/sources/core_library/sources") ## Optimize
#    set(CMAKE_EXE_LINKER_FLAGS "-s -lrt") ## Strip binary
elseif(CMAKE_CXX_COMPILER_ID MATCHES "GNU")
#    set(CMAKE_CXX_FLAGS "-O3 -march=native -std=c++11 -I/sources/core_library/sources") ## Optimize
    set(CMAKE_CXX_FLAGS "-O3 -march=native -std=c++11") ## Optimize
    set(CMAKE_EXE_LINKER_FLAGS "-s") ## Strip binary
endif()

include_directories(sources)
add_subdirectory(sources)

### MAIN OF THE LIBRARY
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin )
add_executable(terrain_trees sources/targets/main.cpp sources/utilities/utility_functions.h sources/utilities/utility_functions.cpp)
target_link_libraries(terrain_trees _terrain_trees_apps)


### TEST THE LIBRARY
#set(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/test )
add_executable(test_index_generation EXCLUDE_FROM_ALL sources/tests/test_spatial_index_generation.cpp sources/utilities/utility_functions.h sources/utilities/utility_functions.cpp)
target_link_libraries(test_index_generation _terrain_trees_core)

add_executable(test_curvature EXCLUDE_FROM_ALL sources/tests/test_curvature.cpp sources/utilities/utility_functions.h sources/utilities/utility_functions.cpp)
target_link_libraries(test_curvature _terrain_trees_apps)

add_executable(test_soup EXCLUDE_FROM_ALL sources/tests/test_soup.cpp sources/utilities/utility_functions.h sources/utilities/utility_functions.cpp)
target_link_libraries(test_soup _terrain_trees_apps)

add_executable(test_batched_queries EXCLUDE_FROM_ALL sources/tests/test_batched_queries.cpp sources/utilities/utility_functions.h sources/utilities/utility_functions.cpp)
target_link_libraries(test_batched_queries _terrain_trees_apps)

add_executable(test_terrain_feature_extraction EXCLUDE_FROM_ALL sources/tests/test_terrain_feature_extraction.cpp sources/utilities/utility_functions.h sources/utilities/utility_functions.cpp)
target_link_libraries(test_terrain_feature_extraction _terrain_trees_apps)

add_executable(test_spatial_queries EXCLUDE_FROM_ALL sources/tests/test_spatial_queries.cpp sources/utilities/utility_functions.h sources/utilities/utility_functions.cpp)
target_link_libraries(test_spatial_queries _terrain_trees_apps)

add_custom_target(tests DEPENDS test_index_generation test_curvature test_soup test_batched_queries test_terrain_feature_extraction test_spatial_queries)


