include_directories(${CMAKE_CURRENT_SOURCE_DIR}/libnnp)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/libnnpif)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/libnnptrain)

# libnnp
file(GLOB libnnp_srcs "${CMAKE_CURRENT_SOURCE_DIR}/libnnp/*.cpp")
add_library(nnp SHARED "${libnnp_srcs}")

# libnnpif (LAMMPS interface library)
add_library(nnpif SHARED "${CMAKE_CURRENT_SOURCE_DIR}/libnnpif/InterfaceLammps.cpp")

# libnnptrain
file(GLOB libnnptrain_srcs "${CMAKE_CURRENT_SOURCE_DIR}/libnnptrain/*.cpp")
add_library(nnptrain SHARED "${libnnptrain_srcs}")
# This must be linked to Eigen3
if (MKL_FOUND)
    target_link_libraries(nnptrain nnp Eigen3::Eigen ${GSL_LIBRARIES} ${MKL_LIBRARIES})
else()
    target_link_libraries(nnptrain nnp Eigen3::Eigen ${GSL_LIBRARIES})
endif()
