cmake_minimum_required(VERSION 2.8.3)
project(navigator_path_planner)
find_package(catkin
  REQUIRED
  COMPONENTS roscpp rospy std_msgs geometry_msgs nav_msgs actionlib actionlib_msgs 
)
catkin_python_setup()

find_package(PythonLibs REQUIRED)
find_package(Boost REQUIRED COMPONENTS python)
include_directories(${PYTHON_INCLUDE_DIRS})

find_package(Eigen3 REQUIRED)
include_directories(${Eigen_INCLUDE_DIRS}
					${PYTHON_INCLUDE_DIRS})

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
    ${Boost_INCLUDE_DIRS}
    ${EIGEN_INCLUDE_DIRS}
)

link_directories(${Boost_LIBRARY_DIRS})

# get python
find_package(PythonLibs REQUIRED)
include_directories(${PYTHON_INCLUDE_DIRS})
link_directories(${PYTHON_LIBRARIES})



add_library(kdtree SHARED lqRRT/lqrrt/kdtree.cpp)
target_link_libraries(kdtree ${Boost_LIBRARIES} ${PYTHON_LIBRARIES} ${catkin_LIBRARIES})
add_dependencies(kdtree ${catkin_EXPORTED_TARGETS})
add_dependencies(kdtree ${PROJECT_NAME}_generate_messages_cpp)

set_target_properties(kdtree 
	PROPERTIES 
	COMPILE_FLAGS "-lboost_numpy -lboost_python -O3 -std=c++0x " 
	LIBRARY_OUTPUT_DIRECTORY ${CATKIN_DEVEL_PREFIX}/lqrrt
)

