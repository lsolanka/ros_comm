cmake_minimum_required(VERSION 2.8.3)

project(roslz4)

find_package(catkin REQUIRED)
find_package(lz4 CONFIG REQUIRED)

catkin_python_setup()

set(PROJECT_INSTALLSPACE_LIBRARIES ros::roslz4)
catkin_package(
  INCLUDE_DIRS include
  LIBRARIES roslz4
  DEPENDS lz4)

include_directories(include ${lz4_INCLUDE_DIRS} ${catkin_INCLUDE_DIRS})
add_library(roslz4 src/lz4s.c src/xxhash.c)
set_source_files_properties(
  src/lz4s.c
PROPERTIES COMPILE_FLAGS "-Wno-sign-compare")
set_source_files_properties(
  src/lz4s.c src/xxhash.c
PROPERTIES COMPILE_DEFINITIONS "XXH_NAMESPACE=ROSLZ4_")
target_link_libraries(roslz4 lz4::lz4 ${catkin_LIBRARIES})
target_include_directories(roslz4 PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>
)


if(NOT ANDROID)
# Python bindings
set(Python_ADDITIONAL_VERSIONS "${PYTHON_VERSION_MAJOR}.${PYTHON_VERSION_MINOR}")
find_package(PythonLibs "${PYTHON_VERSION_MAJOR}.${PYTHON_VERSION_MINOR}" REQUIRED)
include_directories(${PYTHON_INCLUDE_PATH})

add_library(roslz4_py SHARED src/_roslz4module.c)
set_source_files_properties(
  src/_roslz4module.c
PROPERTIES COMPILE_FLAGS "-Wno-missing-field-initializers -Wno-unused-variable -Wno-strict-aliasing")
set_target_properties(
  roslz4_py PROPERTIES OUTPUT_NAME roslz4 PREFIX "_" SUFFIX ".so"
  LIBRARY_OUTPUT_DIRECTORY ${CATKIN_DEVEL_PREFIX}/${PYTHON_INSTALL_DIR}/roslz4
  DEBUG_POSTFIX "" RELEASE_POSTFIX ""
)
target_link_libraries(roslz4_py roslz4 ${catkin_LIBRARIES} ${PYTHON_LIBRARIES})
endif()

install(TARGETS roslz4
  EXPORT ${PROJECT_NAME}Targets
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

if(NOT ANDROID)
install(TARGETS roslz4_py
  LIBRARY DESTINATION ${CATKIN_PACKAGE_PYTHON_DESTINATION})
endif()

install(DIRECTORY include/${PROJECT_NAME}
  DESTINATION ${CATKIN_GLOBAL_INCLUDE_DESTINATION}
  FILES_MATCHING PATTERN "*.h"
)

install(EXPORT ${PROJECT_NAME}Targets
    DESTINATION share/${PROJECT_NAME}/cmake
    NAMESPACE ros::
    FILE ${PROJECT_NAME}Targets.cmake
)

# Testing
if (CATKIN_ENABLE_TESTING)
  catkin_add_gtest(test_roslz4 test/roslz4_test.cpp)
  if (TARGET test_roslz4)
    target_link_libraries(test_roslz4 roslz4 ${catkin_LIBRARIES})
  endif()
endif()
