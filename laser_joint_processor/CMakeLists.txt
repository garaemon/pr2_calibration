cmake_minimum_required(VERSION 2.4.6)
include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)

# Set the build type.  Options are:
#  Coverage       : w/ debug symbols, w/o optimization, w/ code-coverage
#  Debug          : w/ debug symbols, w/o optimization
#  Release        : w/o debug symbols, w/ optimization
#  RelWithDebInfo : w/ debug symbols, w/ optimization
#  MinSizeRel     : w/o debug symbols, w/ optimization, stripped binaries
#set(ROS_BUILD_TYPE RelWithDebInfo)

rosbuild_init()

#set the default path for built executables to the "bin" directory
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set the default path for built libraries to the "lib" directory
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

find_package(OpenCV)
include_directories(SYSTEM ${OpenCV_INCLUDE_DIRS})
rosbuild_add_library(${PROJECT_NAME} src/joint_imager.cpp
                                     src/laser_joint_processor.cpp
                                     src/joint_image_interpolator.cpp)
target_link_libraries(${PROJECT_NAME} ${OpenCV_LIBRARIES})

rosbuild_add_boost_directories()
rosbuild_add_executable(laser_joint_processor_node src/laser_joint_processor_node.cpp)
target_link_libraries(laser_joint_processor_node ${PROJECT_NAME})
rosbuild_link_boost(laser_joint_processor_node thread signals)

#common commands for building c++ executables and libraries
#rosbuild_add_library(${PROJECT_NAME} src/example.cpp)
#target_link_libraries(${PROJECT_NAME} another_library)
#rosbuild_add_executable(example examples/example.cpp)
#target_link_libraries(example ${PROJECT_NAME})

add_subdirectory(test EXCLUDE_FROM_ALL)
