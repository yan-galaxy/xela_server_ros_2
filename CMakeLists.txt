cmake_minimum_required(VERSION 3.5)
project(xela_server_ros)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  rospy
  std_msgs
  geometry_msgs
  message_generation
  visualization_msgs  # 添加对visualization_msgs的依赖
  tf                  # 添加对tf的依赖
)
################################################
## Declare ROS messages, services and actions ##
################################################
add_message_files(
  FILES
  "SensorFull.msg"
  "SensStream.msg"
  "Taxel.msg"
  "Forces.msg"
)

add_service_files(
  FILES
  "XelaSensorStream.srv"
)

generate_messages(
  DEPENDENCIES
  std_msgs
  geometry_msgs

)
catkin_package(
  CATKIN_DEPENDS 
    rospy 
    std_msgs 
    geometry_msgs
    message_runtime
)

###########
## Build ##
###########

include_directories(
  ${catkin_INCLUDE_DIRS}
)

#添加pthread
set(CMAKE_CXX_FLAGS -pthread)
message(STATUS "CMAKE_CXX_FLAGS = ${CMAKE_CXX_FLAGS}")

# 添加可执行目标
add_executable(subscriber_sensor src/subscriber_sensor.cpp)

# 链接ROS包
target_link_libraries(subscriber_sensor ${catkin_LIBRARIES} )
#############
## Install ##
#############

install(PROGRAMS
  scripts/xela_server
  scripts/xela_service
  launch/service.launch
  DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)