cmake_minimum_required(VERSION 2.8.3)
project(elas_ros)
set(CMAKE_BUILD_TYPE Release)

find_package(catkin REQUIRED COMPONENTS
  libelas
  roscpp
  sensor_msgs
  stereo_msgs
  nav_msgs
  message_filters
  image_transport
  cv_bridge
  image_geometry
  tf
  pcl_ros
  pcl_conversions
  geometry_msgs
  message_generation
)

find_package(OpenCV REQUIRED)
#find_package(Boost REQUIRED COMPONENTS signals)

add_message_files( DIRECTORY msg FILES ElasFrameData.msg)

generate_messages(
  DEPENDENCIES
    std_msgs
    sensor_msgs
)

add_definitions(-msse3)

include_directories(src ${libelas_INCLUDE_DIRS} ${catkin_INCLUDE_DIRS})
include_directories(${Boost_INCLUDE_DIRS})

add_executable(elas_ros src/elas.cpp)

add_dependencies(elas_ros ${catkin_EXPORTED_TARGETS})
add_dependencies(elas_ros ${PROJECT_NAME}_gencpp)

add_executable(pc_construction src/pc_construction.cpp)
add_dependencies(pc_construction ${PROJECT_NAME}_gencpp)

target_link_libraries(elas_ros ${OpenCV_LIBRARIES} ${Boost_LIBRARIES} ${catkin_LIBRARIES})
target_link_libraries(pc_construction ${OpenCV_LIBRARIES} ${Boost_LIBRARIES} ${catkin_LIBRARIES})


install(TARGETS elas_ros
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})

install(TARGETS pc_construction
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})
