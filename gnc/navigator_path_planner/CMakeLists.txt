cmake_minimum_required(VERSION 2.8.3)
project(navigator_path_planner)
find_package(catkin
  REQUIRED
  COMPONENTS roscpp rospy std_msgs geometry_msgs nav_msgs actionlib actionlib_msgs 
)
catkin_python_setup()

find_package(Eigen3 REQUIRED)
include_directories(${Eigen_INCLUDE_DIRS})

add_action_files(
  DIRECTORY action
  FILES Move.action
)

generate_messages(
  DEPENDENCIES actionlib_msgs std_msgs geometry_msgs nav_msgs
)

include_directories(
    include
  SYSTEM
    ${catkin_INCLUDE_DIRS}
    ${Boost_INCLUDE_DIR}
    ${EIGEN_INCLUDE_DIRS}
)

find_package(PythonLibs REQUIRED)
include_directories(${PYTHON_INCLUDE_DIRS})


add_library(kdtree lqRRT/lqrrt/kdtree.cpp)
target_link_libraries(kdtree ${catkin_LIBRARIES})
target_link_libraries(kdtree ${PYTHON_LIBRARIES})
add_dependencies(kdtree ${catkin_EXPORTED_TARGETS})
add_dependencies(kdtree ${PROJECT_NAME}_generate_messages_cpp)
set_target_properties(kdtree PROPERTIES COMPILE_FLAGS "-O3 -std=c++0x")

