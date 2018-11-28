cmake_minimum_required(VERSION 3.12)
project(n2p2)

set(CMAKE_CXX_STANDARD 14)

# Binaries
set(N2P2_BIN_DIR "${CMAKE_CURRENT_SOURCE_DIR}/bin")

# ====== Check if some dependencies are there ======
# (1) MPI
# (2) Eigen template library
# (3) GSL
#
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

# ===================================================

include_directories(src/libnnp)
include_directories(src/libnnpif)
include_directories(src/libnnptrain)

add_library(nnp STATIC
        src/libnnp/Atom.cpp
        src/libnnp/Atom.h
        src/libnnp/CutoffFunction.cpp
        src/libnnp/CutoffFunction.h
        src/libnnp/Element.cpp
        src/libnnp/Element.h
        src/libnnp/ElementMap.cpp
        src/libnnp/ElementMap.h
        src/libnnp/Log.cpp
        src/libnnp/Log.h
        src/libnnp/Mode.cpp
        src/libnnp/Mode.h
        src/libnnp/NeuralNetwork.cpp
        src/libnnp/NeuralNetwork.h
        src/libnnp/Prediction.cpp
        src/libnnp/Prediction.h
        src/libnnp/Settings.cpp
        src/libnnp/Settings.h
        src/libnnp/SetupAnalysis.cpp
        src/libnnp/SetupAnalysis.h
        src/libnnp/Stopwatch.cpp
        src/libnnp/Stopwatch.h
        src/libnnp/Structure.cpp
        src/libnnp/Structure.h
        src/libnnp/SymmetryFunction.cpp
        src/libnnp/SymmetryFunction.h
        src/libnnp/SymmetryFunctionAngularNarrow.cpp
        src/libnnp/SymmetryFunctionAngularNarrow.h
        src/libnnp/SymmetryFunctionAngularWide.cpp
        src/libnnp/SymmetryFunctionAngularWide.h
        src/libnnp/SymmetryFunctionGroup.cpp
        src/libnnp/SymmetryFunctionGroup.h
        src/libnnp/SymmetryFunctionGroupAngularNarrow.cpp
        src/libnnp/SymmetryFunctionGroupAngularNarrow.h
        src/libnnp/SymmetryFunctionGroupAngularWide.cpp
        src/libnnp/SymmetryFunctionGroupAngularWide.h
        src/libnnp/SymmetryFunctionGroupRadial.cpp
        src/libnnp/SymmetryFunctionGroupRadial.h
        src/libnnp/SymmetryFunctionGroupWeightedAngular.cpp
        src/libnnp/SymmetryFunctionGroupWeightedAngular.h
        src/libnnp/SymmetryFunctionGroupWeightedRadial.cpp
        src/libnnp/SymmetryFunctionGroupWeightedRadial.h
        src/libnnp/SymmetryFunctionRadial.cpp
        src/libnnp/SymmetryFunctionRadial.h
        src/libnnp/SymmetryFunctionStatistics.cpp
        src/libnnp/SymmetryFunctionStatistics.h
        src/libnnp/SymmetryFunctionWeightedAngular.cpp
        src/libnnp/SymmetryFunctionWeightedAngular.h
        src/libnnp/SymmetryFunctionWeightedRadial.cpp
        src/libnnp/SymmetryFunctionWeightedRadial.h
        src/libnnp/utility.cpp
        src/libnnp/utility.h
        src/libnnp/Vec3D.h
        src/libnnp/version.h
        )

add_library(nnpif SHARED
        src/libnnpif/InterfaceLammps.cpp
        src/libnnpif/InterfaceLammps.h
        src/libnnpif/mpi-extra.h)

add_library(nnptrain STATIC
        src/libnnptrain/Dataset.cpp
        src/libnnptrain/Dataset.h
        src/libnnptrain/GradientDescent.cpp
        src/libnnptrain/GradientDescent.h
        src/libnnptrain/KalmanFilter.cpp
        src/libnnptrain/KalmanFilter.h
        src/libnnptrain/mpi-extra.h
        src/libnnptrain/Training.cpp
        src/libnnptrain/Training.h
        src/libnnptrain/Updater.cpp
        src/libnnptrain/Updater.h)
# This must be linked to Eigen3
target_link_libraries(nnptrain nnp Eigen3::Eigen ${GSL_LIBRARIES})

# Test first executable: nnptrain
add_executable(nnp-train
        src/application/nnp-train/main.cpp)
target_link_libraries(nnp-train nnptrain)

# nnp-comp2
add_executable(nnp-comp2
        src/application/nnp-comp2/main.cpp)
target_link_libraries(nnp-comp2 nnptrain)
