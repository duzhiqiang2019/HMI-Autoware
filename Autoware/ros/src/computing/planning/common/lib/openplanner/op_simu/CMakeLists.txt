cmake_minimum_required(VERSION 2.8.3)
project(op_simu)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

# Get a ros version
execute_process(
	COMMAND rosversion -d
	OUTPUT_VARIABLE ROS_VERSION
	OUTPUT_STRIP_TRAILING_WHITESPACE
)

find_package(catkin REQUIRED COMPONENTS
		op_utility
		op_planner
)

find_package(OpenCV REQUIRED)
find_package(OpenGL REQUIRED)
find_package(GLEW REQUIRED)
find_package(GLUT REQUIRED)
find_package(Eigen3 REQUIRED)
find_package(X11 REQUIRED)
find_package(Threads REQUIRED)
#find_package(TinyXML REQUIRED)

###################################
## catkin specific configuration ##
###################################
catkin_package(
   INCLUDE_DIRS include
   LIBRARIES  op_simu
   CATKIN_DEPENDS op_utility op_planner
)

###########
## Build ##
###########

set(CMAKE_CXX_FLAGS "-std=c++11 -g -fPIC ${CMAKE_CXX_FLAGS}")

include_directories(${catkin_INCLUDE_DIRS}
		include
		${OpenCV_INCLUDE_DIRS}
		${OpenGL_INCLUDE_DIRS}
		${GLUT_INCLUDE_DIRS}
		${GLEW_INCLUDE_DIRS}
)

set(SIMUH_SRC
		src/AlternativeVisualizer.cpp
		src/CarState.cpp
		src/DrawObjBase.cpp
		src/DrawingHelpers.cpp
		src/Graph2dBase.cpp
		src/PlannerTestDraw.cpp
		src/SimpleTracker.cpp
		src/SimulatedTrajectoryFollower.cpp
		src/TrajectoryFollower.cpp
		src/glm.cpp
		src/MainWindowWrapper.cpp
)

## Declare a cpp library
add_library(${PROJECT_NAME}
	    ${SIMUH_SRC}
)

target_link_libraries(${PROJECT_NAME}
		${catkin_LIBRARIES}
		${OpenCV_LIBRARIES}
		${OPENGL_LIBRARIES}
		${GLUT_LIBRARIES}
		${GLEW_LIBRARIES}
		${X11_LIBRARIES}
		${TinyXML_LIBRARIES}
)
add_executable(Simu
	    ${SIMUH_SRC}
	    main.cpp
)

target_link_libraries(Simu
		${catkin_LIBRARIES}
		${OpenCV_LIBRARIES}
		${OPENGL_LIBRARIES}
		${GLUT_LIBRARIES}
		${GLUT_LIBRARY}
		${GLEW_LIBRARIES}
		${X11_LIBRARIES}
		${CMAKE_THREAD_LIBS_INIT}
		${TinyXML_LIBRARIES}
)


