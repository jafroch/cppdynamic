cmake_minimum_required(VERSION 2.8.7)
project(core_dynamic)

IF(BIICODE)
  include(${CMAKE_HOME_DIRECTORY}/biicode.cmake)
  # Initializes block variables
  INIT_BIICODE_BLOCK()
  SET(BII_CREATE_LIB TRUE)
  # Include base block dir
  ADD_BIICODE_TARGETS()
  TARGET_INCLUDE_DIRECTORIES(${BII_LIB_TARGET} PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include)
IF(APPLE)
        SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x -stdlib=libc++")
ELSEIF (WIN32 OR UNIX)
        SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
ENDIF(APPLE)
ELSE()	
  # Previous CMakeLists.txt goes here


if(CMAKE_COMPILER_IS_GNUCXX)
	add_definitions(-std=c++0x)
endif()
file(GLOB_RECURSE files include/**.h)
message("files ${files}")
include_directories(include)
add_executable(test test.cpp ${files})
ENDIF()

