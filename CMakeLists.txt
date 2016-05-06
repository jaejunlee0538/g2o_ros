cmake_minimum_required(VERSION 2.8.3)
project(g2o_ros)

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

set(G2O_LIBS
        types_data
        types_icp
        types_sba
        types_sclam2d
        types_sim3
        types_slam2d_addons
        types_slam2d
        types_slam3d_addons
        types_slam3d
        solver_structure_only
        solver_slam2d_linear
        solver_pcg
        solver_eigen
        solver_dense
        solver_cholmod
        solver_csparse
        csparse_extension
        core
        stuff
        )

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


