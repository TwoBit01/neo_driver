cmake_minimum_required(VERSION 2.8.3)
project(neo_relayboard)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
  roscpp
  sensor_msgs
  std_msgs
  trajectory_msgs
  neo_msgs
)


catkin_package(
  INCLUDE_DIRS common/include
  LIBRARIES ${PROJECT_NAME} SerialIO
  CATKIN_DEPENDS roscpp sensor_msgs std_msgs trajectory_msgs neo_msgs
#  DEPENDS system_lib
)

###########
## Build ##
###########

include_directories(common/include ${Boost_INCLUDE_DIRS} ${catkin_INCLUDE_DIRS})

add_library(SerialIO common/src/SerialIO.cpp)
add_library(${PROJECT_NAME} common/src/SerRelayBoard.cpp common/src/StrUtil.cpp common/src/neo_relayboard_lib.cpp)

add_dependencies(${PROJECT_NAME} ${PROJECT_NAME}_gencpp)
add_dependencies(SerialIO ${PROJECT_NAME}_gencpp)

target_link_libraries(${PROJECT_NAME} SerialIO)

add_executable(neo_relayboard_node src/neo_relayboard_node.cpp)
target_link_libraries(neo_relayboard_node ${PROJECT_NAME})
target_link_libraries(neo_relayboard_node ${catkin_LIBRARIES})
add_dependencies(neo_relayboard_node ${PROJECT_NAME}_gencpp)

#############
## Install ##
#############

## Mark executables and/or libraries for installation
install(TARGETS ${PROJECT_NAME} SerialIO neo_relayboard_node
    ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
    LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
    RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

## Mark cpp header files for installation
install(DIRECTORY common/include/
 DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
 FILES_MATCHING PATTERN "*.h"
 PATTERN ".svn" EXCLUDE
)


#############
## Testing ##
#############


