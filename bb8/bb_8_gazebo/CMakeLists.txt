cmake_minimum_required(VERSION 2.8.3)
project(bb_8_gazebo)

find_package(gazebo REQUIRED)

find_package(catkin
  REQUIRED
    angles
    control_toolbox
    gazebo_ros
    robot_controllers
    robot_controllers_interface
)


include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${GAZEBO_INCLUDE_DIRS}
  ${Boost_INCLUDE_DIRS}
  ${SDFormat_INCLUDE_DIRS}
)

link_directories(
  /opt/ros/kinetic/lib
)

catkin_package(
  INCLUDE_DIRS include
  CATKIN_DEPENDS
    control_toolbox
    gazebo_ros
    robot_controllers
    robot_controllers_interface
  DEPENDS
    gazebo
    SDF
  LIBRARIES
	gazebo_bb_8_controller
)


add_library(gazebo_bb_8_controller src/gazebo_bb_8_controller.cpp)
target_link_libraries(gazebo_bb_8_controller gazebo_ros_utils ${GAZEBO_LIBRARIES} ${catkin_LIBRARIES} ${Boost_LIBRARIES})


#add_library(bb_8_gazebo_plugin src/plugin.cpp)
#target_link_libraries(gazebo_bb_8_controller
#  ${catkin_LIBRARIES}
#  ${GAZEBO_LIBRARIES}
#)


# add support for C++11
set(CMAKE_CXX_FLAGS "-std=c++11 ${CMAKE_CXX_FLAGS}")

#install(
#  TARGETS fetch_gazebo_plugin
#  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}  
#)

# Mark other files for installation (e.g. launch and bag files, etc.)
install(DIRECTORY
  launch
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)

install(
  DIRECTORY include launch worlds models
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)

