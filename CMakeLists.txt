cmake_minimum_required(VERSION 2.4.6)
if(COMMAND cmake_policy)
  cmake_policy(SET CMP0003 NEW)
endif(COMMAND cmake_policy)

project(openni2_multicamera)

find_package(catkin REQUIRED openni2_camera camera_info_manager dynamic_reconfigure image_transport  nodelet sensor_msgs nav_msgs roscpp message_generation cv_bridge)

find_package(Boost REQUIRED COMPONENTS system thread)

find_package(OpenCV)
include_directories(${OpenCV_INCLUDE_DIRS})
set(LIBS ${LIBS} ${OpenCV_LIBRARIES} tinyxml)

find_package(PkgConfig)

set(CMAKE_CXX_FLAGS -std=c++11)
set(CMAKE_BUILD_TYPE Debug)

catkin_package(
  LIBRARIES multicamera_driver_lib
  CATKIN_DEPENDS openni2_camera camera_info_manager dynamic_reconfigure image_transport nodelet sensor_msgs roscpp message_runtime cv_bridge
  # DEPENDS libopenni2
)

include_directories(include
                    ${catkin_INCLUDE_DIRS}
                    ${Boost_INCLUDE_DIRS}
                    ${PC_OPENNI2_INCLUDE_DIRS}
                    )

link_directories(${PC_OPENNI2_LIBRARY_DIRS})

# add_library(multicamera_driver_lib
#    src/multicamera_driver.cpp
# )
# target_link_libraries(multicamera_driver_lib ${catkin_LIBRARIES} ${Boost_LIBRARIES} )
# add_dependencies(openni2_driver_lib ${PROJECT_NAME}_gencfg)


add_executable(show_info src/show_info.cpp)
target_link_libraries(show_info ${catkin_LIBRARIES} ${Boost_LIBRARIES} ${LIBS})

add_executable(generate_launch_file src/generate_launch_file.cpp)
target_link_libraries(generate_launch_file ${catkin_LIBRARIES} ${Boost_LIBRARIES} ${LIBS})
