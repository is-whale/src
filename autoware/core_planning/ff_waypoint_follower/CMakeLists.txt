cmake_minimum_required(VERSION 2.8.3)
project(ff_waypoint_follower)

find_package(autoware_build_flags REQUIRED)

find_package(OpenCV REQUIRED)

find_package(
  catkin REQUIRED COMPONENTS
  autoware_msgs
  geometry_msgs
  libwaypoint_follower
  map_file
  op_planner
  op_simu
  op_utility
  roscpp
)

catkin_package(
  INCLUDE_DIRS include
  CATKIN_DEPENDS
    autoware_msgs
    geometry_msgs
    libwaypoint_follower
    map_file
    op_planner
    op_simu
    op_utility
)

SET(CMAKE_CXX_FLAGS "-O2 -g -Wall -Wno-unused-result -DROS ${CMAKE_CXX_FLAGS}")

include_directories(
  include
  #  /usr/local/include/zmp/minivan
  ${catkin_INCLUDE_DIRS}
)

link_directories(lib)
add_executable(
  ff_waypoint_follower
  nodes/ff_waypoint_follower.cpp
  nodes/ff_waypoint_follower_core.cpp
)
target_link_libraries(
  ff_waypoint_follower
  ${catkin_LIBRARIES}
  ${OpenCV_LIBS}
)

#link_directories(/usr/local/lib/zmp/minivan)
#add_executable(ff_waypoint_follower nodes/ff_waypoint_follower.cpp nodes/ff_waypoint_follower_core.cpp nodes/HevCnt.cpp nodes/HevComm.cpp)
#target_link_libraries(ff_waypoint_follower utilityh plannerh simuh CANUSB hevcontrol ${catkin_LIBRARIES})

add_dependencies(
  ff_waypoint_follower
  ${catkin_EXPORTED_TARGETS}
)

install(
  TARGETS ff_waypoint_follower
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(
  DIRECTORY launch/
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/launch
)

install(
  DIRECTORY params/
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/params
)
