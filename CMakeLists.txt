# TODO:
# I kept the old makefiles for reference, but they won't be included in future commits
# - doc (via doxygen)
# - python module/package (?)

cmake_minimum_required(VERSION 3.0)
project(n2p2)

# Add local 'cmake' folder to module path
set(CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)

message(STATUS "Current CMAKE_MODULE_PATH: ${CMAKE_MODULE_PATH}")

# CXX language standard
set(CMAKE_CXX_STANDARD 14)

# Bins & lib folders
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)

# ====== Check if some dependencies are there ======
# (1) MPI
# (2) Eigen template library
# (3) GSL
# (4) MKL (if option USE_MKL=ON)

# Options
option(USE_MKL "Search and link against MKL. Fail if not found" OFF)

# MPI
find_package(MPI)
if(MPI_CXX_FOUND)
    set(CMAKE_CXX_COMPILER ${MPI_CXX_COMPILER})
else(MPI_CXX_FOUND)
    message(FATAL_ERROR "MPI not found on this machine! Abort.")
endif(MPI_CXX_FOUND)

# Eigen
# The following aborts compilation if not found
find_package(Eigen3 3.0 REQUIRED NO_MODULE)

# GSL
find_package(GSL REQUIRED)
message(STATUS "Found GNU Scientific Library. Version: ${GSL_VERSION}")
include_directories(${GSL_INCLUDE_DIRS})

# MKL libraries
if(USE_MKL)
    message(STATUS "Searching for Intel MKL")
    find_package(MKL REQUIRED MODULE)
    if(MKL_FOUND)
        include_directories(${MKL_INCLUDE_DIR})
    endif(MKL_FOUND)
else(USE_MKL)
    message(STATUS "Skipping search for Intel MKL")
endif(USE_MKL)

# ===================================================

# Add subdirectory for libraries
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/src/libs)

# Adding application executables
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include)
file(GLOB binaries ${CMAKE_CURRENT_SOURCE_DIR}/src/application/*.cpp)
foreach(bin ${binaries})
    get_filename_component(bin_name ${bin} NAME_WE)
    get_filename_component(bin_dir ${bin} DIRECTORY)
    #message(WARNING "Adding exe ${bin_name}")
    add_executable(${bin_name} ${bin})
    #target_include_directories(${bin_name} PUBLIC ${bin_dir})
    target_link_libraries(${bin_name} nnptrain)
endforeach(bin)
