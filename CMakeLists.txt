cmake_minimum_required(VERSION 2.8.3)
project(g2o_ros)

# check c++11 / c++0x(https://svn.boost.org/trac/boost/ticket/9240)
include(CheckCXXCompilerFlag)
CHECK_CXX_COMPILER_FLAG("-std=c++11" COMPILER_SUPPORTS_CXX11)
CHECK_CXX_COMPILER_FLAG("-std=c++0x" COMPILER_SUPPORTS_CXX0X)
if(COMPILER_SUPPORTS_CXX11)
    set(CMAKE_CXX_FLAGS "-std=c++11  -fext-numeric-literals")
    #'-fext-numeric-literals' : https://svn.boost.org/trac/boost/ticket/9240
elseif(COMPILER_SUPPORTS_CXX0X)
    set(CMAKE_CXX_FLAGS "-std=c++0x")
else()
    message(FATAL_ERROR "The compiler ${CMAKE_CXX_COMPILER} has no C++11 support. Please use a different C++ compiler.")
endif()

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED)

#find_package(CSparse)

add_subdirectory(g2o)

include_directories(
        g2o
        ${PROJECT_BINARY_DIR}/g2o)
set(G2O_INCLUDE "g2o;../../build/g2o_ros/g2o" )

if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    set(LIB_POSTFIX ${CMAKE_DEBUG_POSTFIX})
elseif(CMAKE_BUILD_TYPE STREQUAL "Release")
    set(LIB_POSTFIX ${CMAKE_RELEASE_POSTFIX})
elseif(CMAKE_BUILD_TYPE STREQUAL "RelWithDebInfo")
    set(LIB_POSTFIX ${CMAKE_RELWITHDEBINFO_POSTFIX})
else(CMAKE_BUILD_TYPE STREQUAL "Debug")
    set(LIB_POSTFIX ${CMAKE_MINSIZEREL_POSTFIX})
endif(CMAKE_BUILD_TYPE STREQUAL "Debug")

set(G2O_LIBS
        g2o_types_data${LIB_POSTFIX}
        g2o_types_icp${LIB_POSTFIX}
        g2o_types_sba${LIB_POSTFIX}
        g2o_types_sclam2d${LIB_POSTFIX}
        g2o_types_sim3${LIB_POSTFIX}
        g2o_types_slam2d_addons${LIB_POSTFIX}
        g2o_types_slam2d${LIB_POSTFIX}
        g2o_types_slam3d_addons${LIB_POSTFIX}
        g2o_types_slam3d${LIB_POSTFIX}
        g2o_solver_structure_only${LIB_POSTFIX}
        g2o_solver_slam2d_linear${LIB_POSTFIX}
        g2o_solver_pcg${LIB_POSTFIX}
        g2o_solver_eigen${LIB_POSTFIX}
        g2o_solver_dense${LIB_POSTFIX}
        g2o_solver_cholmod${LIB_POSTFIX}
        g2o_solver_csparse${LIB_POSTFIX}
        g2o_csparse_extension${LIB_POSTFIX}
        g2o_core${LIB_POSTFIX}
        g2o_stuff${LIB_POSTFIX}
        )
message("LIBS : ${G2O_LIBS}")
LIST(APPEND CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/g2o_ros/g2o/cmake_modules)
message("modules : ${CMAKE_MODULE_PATH}")
find_package(CSparse)
IF(CSPARSE_FOUND)
    message("Using system CSparse package")
    set(G2O_INCLUDE "${G2O_INCLUDE};${CSPARSE_INCLUDE_DIR}")
    set(G2O_LIBS ${G2O_LIBS} ${CSPARSE_LIBRARY})
else(CSPARSE_FOUND)
    message("Using bundled CSparse package")
    set(G2O_INCLUDE "${G2O_INCLUDE};g2o/EXTERNAL/csparse")
    set(G2O_LIBS ${G2O_LIBS} csparse )
endif(CSPARSE_FOUND)



###################################
## catkin specific configuration ##
###################################
## The catkin_package macro generates cmake config files for your package
## Declare things to be passed to dependent projects
## INCLUDE_DIRS: uncomment this if you package contains header files
## LIBRARIES: libraries you create in this project that dependent projects also need
## CATKIN_DEPENDS: catkin_packages dependent projects also need
## DEPENDS: system dependencies of this project that dependent projects also need
catkin_package(
  INCLUDE_DIRS ${G2O_INCLUDE}
  LIBRARIES ${G2O_LIBS}
#  CATKIN_DEPENDS other_catkin_pkg
#  DEPENDS system_lib
)


