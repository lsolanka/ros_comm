cmake_minimum_required(VERSION 2.8.3)

include(cmake/HunterGate.cmake)
HunterGate(
  URL "https://github.com/ruslo/hunter/archive/v0.22.1.tar.gz"
  SHA1 "3d87bd3c7a54126cb0822a59d3789970c000ab19"
  LOCAL
)

project(ros_comm)

hunter_add_package(Boost
    COMPONENTS date_time filesystem program_options regex system chrono signals thread)
hunter_add_package(BZip2)
hunter_add_package(lz4)
hunter_add_package(catkin)
hunter_add_package(rosconsole)
hunter_add_package(roscpp_core)
hunter_add_package(ros)
hunter_add_package(ros_message_generation)
hunter_add_package(ros_comm_msgs)
hunter_add_package(ros_pluginlib)

find_package(Boost CONFIG REQUIRED
    COMPONENTS date_time filesystem program_options regex system chrono signals thread)

add_subdirectory(tools/rosgraph)
add_subdirectory(tools/roslaunch)
add_subdirectory(tools/rosmaster)
add_subdirectory(tools/rosparam)
add_subdirectory(utilities/xmlrpcpp)
add_subdirectory(clients/roscpp)
add_subdirectory(clients/rospy)
add_subdirectory(utilities/roslz4)
add_subdirectory(tools/rostest)
add_subdirectory(tools/rosnode)
add_subdirectory(tools/rosbag_storage)
add_subdirectory(tools/topic_tools)
add_subdirectory(tools/rosbag)
add_subdirectory(tools/rostopic)
add_subdirectory(tools/rosmsg)
add_subdirectory(tools/rosout)
add_subdirectory(tools/rosservice)
add_subdirectory(utilities/roswtf)
add_subdirectory(utilities/message_filters)
add_subdirectory(ros_comm)

add_subdirectory(test/test_roscpp)
add_subdirectory(test/test_rosbag)
add_subdirectory(test/test_rosbag_storage)

add_subdirectory(test/test_rosgraph)
add_subdirectory(test/test_roslaunch)
add_subdirectory(test/test_roslib_comm)
add_subdirectory(test/test_rosmaster)
add_subdirectory(test/test_rosparam)
add_subdirectory(test/test_rospy)
add_subdirectory(test/test_rosservice)
add_subdirectory(test/test_rostopic)
