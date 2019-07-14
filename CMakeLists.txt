cmake_minimum_required(VERSION 3.10)
project(dso_ros)

find_package(catkin REQUIRED COMPONENTS
  geometry_msgs
  roscpp
  sensor_msgs
  cv_bridge
)

##### DSO #####
## find DSO root path
set(DSO_PATH $ENV{DSO_PATH})
if("${DSO_PATH}" STREQUAL "")
  message(WARNING "\n\nyou have to set DSO_PATH to your root dso path\n$ set DSO_PATH=<your path>")
  ## set default path if no environment
  set(DSO_PATH C:/dso)
endif()
set(DSO_LIBRARY ${DSO_PATH}/build/lib/${CMAKE_BUILD_TYPE}/dso.lib)
find_library(DSO_LIBRARY dso ${DSO_PATH}/build/lib/${CMAKE_BUILD_TYPE})
message("\n\n---- LOOKING FOR DSO at\n\"${DSO_PATH}\"")
message("\n### DSO_LIBRARY = ${DSO_LIBRARY}")

##### Pangolin #####
find_package(Pangolin 0.2 REQUIRED)
message("\n\n---- FOUND Pangolin Headers at\n\"${Pangolin_INCLUDE_DIRS}\"")
message("---- FOUND Pangolin Libs at\n\"${Pangolin_LIBRARIES}\"")

##### Eigen #####
find_package(Eigen3 REQUIRED)

##### OpenCV #####
## don't use find_package to avoid refer ROS OpenCV package
#find_package(OpenCV 3.4 REQUIRED)
## find opencv root path
set(OpenCV_ROOT $ENV{OPENCV_PATH})
if("${OpenCV_ROOT}" STREQUAL "")
  message(WARNING "\n\nyou have to set OPENCV_PATH to your root opencv path\n$ set OPENCV_PATH=<your path>")
  ## set default path if no environment
  set(OpenCV_ROOT C:/opencv-3.4.6-vc14_vc15/build)
endif()
set(OpenCV_INCLUDE_DIRS ${OpenCV_ROOT})
set(OpenCV_LIBS "${OpenCV_LIBS};${OpenCV_ROOT}/x64/vc15/lib/opencv_world346.lib")
message("\n\n---- FOUND OpenCV Headers at\n\"${OpenCV_INCLUDE_DIRS}\"")
message("---- FOUND OpenCV Libs at\n\"${OpenCV_LIBS}\"")


##### Boost #####
## don't use find_package to avoid refer ROS OpenCV package
#find_package(Boost 1.70.0 REQUIRED COMPONENTS system thread)
set(BOOST_MSVC msvc-14.2)
set(BOOST_ROOT $ENV{BOOST_PATH})
if("${BOOST_ROOT}" STREQUAL "")
  message(WARNING "\n\nyou have to set BOOST_PATH to your root boost path\n$ set BOOS_PATH=<your path>")
  ## set default path if no environment
  set(BOOST_ROOT C:/boost_1_70_0)
endif()
set(BOOST_LIBRARYDIR ${BOOST_ROOT}/lib64-${BOOST_MSVC})
set(BOOST_INCLUDEDIR ${BOOST_ROOT})
set(Boost_INCLUDE_DIR ${BOOST_INCLUDEDIR})
set(Boost_LIBRARY_DIR ${BOOST_LIBRARYDIR})
string(REPLACE "." "" BOOST_MSVC_LIB ${BOOST_MSVC})
string(REPLACE "msvc-" "vc" BOOST_MSVC_LIB ${BOOST_MSVC_LIB})
set(Boost_LIBS "${BOOST_LIBRARYDIR}/libboost_system-${BOOST_MSVC_LIB}-mt-x64-1_70.lib;${BOOST_LIBRARYDIR}/libboost_thread-${BOOST_MSVC_LIB}-mt-s-x64-1_70.lib;${BOOST_LIBRARYDIR}/libboost_date_time-${BOOST_MSVC_LIB}-mt-s-x64-1_70.lib;${BOOST_LIBRARYDIR}/libboost_chrono-${BOOST_MSVC_LIB}-mt-s-x64-1_70.lib;${BOOST_LIBRARYDIR}/libboost_atomic-${BOOST_MSVC_LIB}-mt-s-x64-1_70.lib;")
message("\n\n---- FOUND Boost Headers at\n\"${BOOST_INCLUDE_DIRS}\"")
message("---- FOUND Boost Libs at \n\"${BOOST_LIBRARY_DIRS}\"")
message("### BOOST_ROOT = ${BOOST_ROOT}")
message("### BOOST_INCLUDEDIR = ${BOOST_INCLUDEDIR}")
message("### BOOST_LIBRARYDIR = ${BOOST_LIBRARYDIR}")
message("### Boost_LIBRARIES = ${Boost_LIBRARIES}")
message("### Boost_FIND_VERSION = ${Boost_FIND_VERSION}")
message("### MSVC_VERSION = ${MSVC_VERSION}")
message("### _boost_COMPILER = ${_boost_COMPILER}")

##### flags #####
add_definitions("-D__SSE3__")
add_definitions("-DNOMINMAX")
string(REPLACE "/MD" "/MT" CMAKE_CXX_FLAGS_DEBUG ${CMAKE_CXX_FLAGS_DEBUG})
string(REPLACE "/MD" "/MT" CMAKE_CXX_FLAGS_RELEASE ${CMAKE_CXX_FLAGS_RELEASE})
string(REPLACE "/MD" "/MT" CMAKE_CXX_FLAGS_RELWITHDEBINFO ${CMAKE_CXX_FLAGS_RELWITHDEBINFO})
if (MSVC)
     set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /EHsc")
endif (MSVC)
message("### CMAKE_BUILD_TYPE = ${CMAKE_BUILD_TYPE}, BUILD_TYPE = ${BUILD_TYPE}")
message("\n\n### CMAKE_CXX_FLAGS_DEBUG = ${CMAKE_CXX_FLAGS_DEBUG}")
message("\n\n### CMAKE_CXX_FLAGS_RELEASE = ${CMAKE_CXX_FLAGS_RELEASE}")
message("\n\n### CMAKE_CXX_FLAGS_RELWITHDEBINFO = ${CMAKE_CXX_FLAGS_RELWITHDEBINFO}")

##### print ALL CMAKE VARIABLES #####
message(STATUS "*** dump start cmake variables ***")
get_cmake_property(_variableNames VARIABLES)
foreach(_variableName ${_variableNames})
## not print
# message(STATUS "${_variableName}=${${_variableName}}")
endforeach()
message(STATUS "*** dump end ***")

## exclude ROS Boost to avoid error
set(catkin_LIBRARIES "C:/opt/ros/melodic/x64/lib/roscpp.lib;;C:/opt/ros/melodic/x64/lib/xmlrpcpp.lib;C:/opt/ros/melodic/x64/lib/cv_bridge.lib;C:/opt/rosdeps/x64/lib/opencv_core341.lib;C:/opt/rosdeps/x64/lib/opencv_imgproc341.lib;C:/opt/rosdeps/x64/lib/opencv_imgcodecs341.lib;C:/opt/ros/melodic/x64/lib/rosconsole.lib;C:/opt/ros/melodic/x64/lib/rosconsole_log4cxx.lib;C:/opt/ros/melodic/x64/lib/rosconsole_backend_interface.lib;C:/opt/rosdeps/x64/lib/log4cxx.lib;C:/opt/ros/melodic/x64/lib/roscpp_serialization.lib;C:/opt/ros/melodic/x64/lib/rostime.lib;C:/opt/ros/melodic/x64/lib/cpp_common.lib;optimized;C:/opt/rosdeps/x64/lib/console_bridge.lib")

## exclude ROS Boost to avoid error
set(catkin_INCLUDE_DIRS "C:/opt/ros/melodic/x64/include;C:/opt/ros/melodic/x64/share/xmlrpcpp/cmake/../../../include/xmlrpcpp;C:/opt/rosdeps/x64/include;C:/opt/rosdeps/x64/include/opencv")

catkin_package(
  CATKIN_DEPENDS
  geometry_msgs
  roscpp
  sensor_msgs
  cv_bridge
  )

###########
## Build ##
###########

include_directories(
  ${BOOST_INCLUDEDIR}
  ${PROJECT_SOURCE_DIR}/src
  ${DSO_PATH}/src
  ${DSO_PATH}/thirdparty/Sophus
  ${Pangolin_INCLUDE_DIRS}
  ${EIGEN3_INCLUDE_DIR}
  ${catkin_INCLUDE_DIRS}
)

message("\n\n#### catkin_LIBRARIES = ${catkin_LIBRARIES}")
message("\n\n#### catkin_INCLUDE_DIRS = ${catkin_INCLUDE_DIRS}")
message("\n\n#### DSO_LIBRARY = ${DSO_LIBRARY}")
message("\n\n#### Pangolin_LIBRARIES = ${Pangolin_LIBRARIES}")
message("\n\n#### OpenCV_LIBS = ${OpenCV_LIBS}")

message(STATUS "\n\n#### add_executable dso_live src/main.cpp")
## Declare a C++ executable
add_executable(dso_live src/main.cpp)

target_link_libraries(dso_live
  ${Boost_LIBS}
  ${DSO_LIBRARY}
  ${Pangolin_LIBRARIES}
  ${OpenCV_LIBS}
  ${catkin_LIBRARIES}
  )
