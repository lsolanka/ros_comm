cmake_minimum_required(VERSION 2.8.3)

project(rosbag_storage)

if(NOT WIN32)
  set_directory_properties(PROPERTIES COMPILE_OPTIONS "-Wall;-Wextra")
endif()

find_package(console_bridge REQUIRED)
find_package(catkin REQUIRED COMPONENTS cpp_common roscpp_serialization roscpp_traits rostime roslz4)
find_package(Boost CONFIG REQUIRED COMPONENTS date_time filesystem program_options regex)
find_package(BZip2 CONFIG REQUIRED)

set(PROJECT_INSTALLSPACE_LIBRARIES ros::rosbag_storage)
catkin_package(
  INCLUDE_DIRS include
  LIBRARIES rosbag_storage
  CATKIN_DEPENDS roslz4
  DEPENDS console_bridge "Boost COMPONENTS date_time filesystem program_options regex"
)

# Support large bags (>2GB) on 32-bit systems
add_definitions(-D_FILE_OFFSET_BITS=64)

include_directories(include ${catkin_INCLUDE_DIRS} ${console_bridge_INCLUDE_DIRS} ${Boost_INCLUDE_DIRS} ${BZIP2_INCLUDE_DIR})

add_library(rosbag_storage
  src/bag.cpp
  src/bag_player.cpp
  src/buffer.cpp
  src/bz2_stream.cpp
  src/lz4_stream.cpp
  src/chunked_file.cpp
  src/message_instance.cpp
  src/query.cpp
  src/stream.cpp
  src/view.cpp
  src/uncompressed_stream.cpp
)

target_link_libraries(rosbag_storage ${catkin_LIBRARIES} ${Boost_LIBRARIES} BZip2::bz2 ${console_bridge_LIBRARIES})

target_include_directories(rosbag_storage PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>
)

install(TARGETS rosbag_storage
  EXPORT ${PROJECT_NAME}Targets
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)
install(EXPORT ${PROJECT_NAME}Targets
    DESTINATION share/${PROJECT_NAME}/cmake
    NAMESPACE ros::
    FILE ${PROJECT_NAME}Targets.cmake
)

install(DIRECTORY include/
  DESTINATION ${CATKIN_GLOBAL_INCLUDE_DESTINATION}
  FILES_MATCHING PATTERN "*.h"
)
