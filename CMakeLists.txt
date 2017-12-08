cmake_minimum_required(VERSION 2.8.3)

include(cmake/HunterGate.cmake)
HunterGate(
    URL "https://github.com/lsolanka/hunter/archive/ros-hunter.tar.gz"
    SHA1 "d4495d186db78814578908e3835cbfc15b9429b7"
)

project(ros_comm)
hunter_add_package(catkin)
hunter_add_package(roscpp_core)

add_subdirectory(utilities/xmlrpcpp)
