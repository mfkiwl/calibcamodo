CMAKE_MINIMUM_REQUIRED(VERSION 2.8.3)
PROJECT(calibcamodo)

LIST(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/CMakeModules/)

# c++ 11 support for g2o, multi-thread and shared_ptr
SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x -Wall")

# Find third party packages
FIND_PACKAGE(Eigen REQUIRED)
FIND_PACKAGE(OpenCV REQUIRED)
FIND_PACKAGE(CSparse REQUIRED)
FIND_PACKAGE(Cholmod REQUIRED)
FIND_PACKAGE(G2O REQUIRED)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
FIND_PACKAGE(catkin REQUIRED COMPONENTS
    geometry_msgs
    nav_msgs
    roscpp
    rospy
    tf
    roslib
    cv_bridge
    cmake_modules
    image_transport
)

INCLUDE_DIRECTORIES(
    src/
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${catkin_INCLUDE_DIRS}
    ${EIGEN_INCLUDE_DIRS}
    ${OpenCV_INCLUDE_DIRS}
    ${CSPARSE_INCLUDE_DIR}
    ${Cholmod_INCLUDE_DIR}
    ${G2O_INCLUDE_DIR}
)

LIST(APPEND LINK_LIBS
    ${catkin_LIBRARIES}
    ${OpenCV_LIBS}
)

LIST(APPEND G2O_LIBS
    cxsparse
    cholmod
    g2o_cli g2o_ext_freeglut_minimal g2o_simulator
    g2o_solver_slam2d_linear g2o_types_icp g2o_types_slam2d
    g2o_core g2o_interface g2o_solver_csparse g2o_solver_structure_only
    g2o_types_sba g2o_types_slam3d g2o_csparse_extension
    g2o_opengl_helper g2o_solver_dense g2o_stuff
    g2o_types_sclam2d g2o_parser g2o_solver_pcg
    g2o_types_data g2o_types_sim3
)

## Declare a C++ executable
AUX_SOURCE_DIRECTORY(./src SRC_DIR )
AUX_SOURCE_DIRECTORY(./src/aruco SRC_DIR_ARUCO)
AUX_SOURCE_DIRECTORY(./src/g2o SRC_DIR_G2O)
ADD_EXECUTABLE(calibcamodo ${SRC_DIR} ${SRC_DIR_ARUCO} ${SRC_DIR_G2O})

# message("SRC_DIR: ${SRC_DIR}")
# message("SRC_DIR_ARUCO: ${SRC_DIR_ARUCO}")
# message("FILE_INCLUDE: ${FILE_INCLUDE}")

## Specify libraries to link a library or executable target against
TARGET_LINK_LIBRARIES(calibcamodo ${LINK_LIBS} ${G2O_LIBS} )
