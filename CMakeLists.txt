cmake_minimum_required(VERSION 2.8.3)

include(cmake/HunterGate.cmake)
HunterGate(
    URL "https://github.com/lsolanka/hunter/archive/ros-hunter.tar.gz"
    SHA1 "8c31818fc407587f192a02ee6db87fd8f94c8f0b"
)

project(ros_comm)
hunter_add_package(Boost COMPONENTS regex system chrono signals thread)
hunter_add_package(lz4)
hunter_add_package(catkin)
hunter_add_package(roscpp_core)
hunter_add_package(ros)
hunter_add_package(message_generation)
hunter_add_package(ros_comm_msgs)

find_package(Boost CONFIG REQUIRED
    COMPONENTS regex system chrono signals thread)

add_subdirectory(utilities/xmlrpcpp)
add_subdirectory(tools/rostest)
add_subdirectory(utilities/roswtf)
add_subdirectory(utilities/roslz4)
add_subdirectory(tools/rosconsole)
add_subdirectory(clients/roscpp)
