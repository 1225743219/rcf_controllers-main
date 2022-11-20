cmake_minimum_required(VERSION 3.10)
project(rcf_chassis_controllers)

## Use C++17
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_definitions(-Wall -Werror -Wno-enum-compare)

# set ROS_DEPENDENCIES , it will be convenient in find_package and catkin_package
set(ROS_DEPENDENCIES
        roscpp
        std_msgs

        rcf_msgs
        rcf_common

        pluginlib
        hardware_interface
        controller_interface
        realtime_tools
        control_toolbox
        effort_controllers
        tf2
        tf2_geometry_msgs
        )

find_package(catkin REQUIRED COMPONENTS ${ROS_DEPENDENCIES})
find_package(Eigen3 REQUIRED)

catkin_package(
        INCLUDE_DIRS
        include
        ${EIGEN3_INCLUDE_DIR}
        LIBRARIES
        ${PROJECT_NAME}
        CATKIN_DEPENDS
        ${ROS_DEPENDENCIES}
)

include_directories(
        include
        ${catkin_INCLUDE_DIRS}
        ${EIGEN3_INCLUDE_DIR}
)

###########
## Build ##
###########

add_library(${PROJECT_NAME}
        src/chassis_base.cpp
        src/balance_pid.cpp
        src/mecanum.cpp
        src/balance_lqr.cpp
        src/omni.cpp)
target_link_libraries(${PROJECT_NAME} ${catkin_LIBRARIES})