cmake_minimum_required(VERSION 2.8)

set(CMAKE_BUILD_TYPE Release CACHE STRING "Build type.")
project(SSVJsonCpp)

set(CMAKE_CXX_FLAGS "-std=c++11 -Wall -Wextra -W -pedantic" CACHE STRING "" FORCE)
set(CMAKE_CXX_FLAGS_RELEASE "-DNDEBUG -O3" CACHE STRING "" FORCE)
set(CMAKE_CXX_FLAGS_DEBUG "-fno-omit-frame-pointer -g3 -gdwarf-2 -O3" CACHE STRING "" FORCE)
set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake/Modules/;${CMAKE_MODULE_PATH}")
set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/../SSVUtils/cmake/Modules/;${CMAKE_MODULE_PATH}")
set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/extlibs/SSVUtils/cmake/Modules/;${CMAKE_MODULE_PATH}")
set(SSVJSONCPP_BUILD_SHARED_LIB FALSE CACHE BOOL "Build shared libraries.")

if(SSVJSONCPP_BUILD_SHARED_LIB)
	set(LIB_TYPE SHARED)
else()
	set(LIB_TYPE STATIC)
endif()

set(SRC_DIR "src")
set(INC_DIR "include")

file(GLOB SRC_LIST
	"${SRC_DIR}/jsoncpp/*.h"
	"${SRC_DIR}/jsoncpp/*.cpp"
	"${SRC_DIR}/jsoncpp/json/*.h"
	"${SRC_DIR}/jsoncpp/json/*.cpp"
	"${INC_DIR}/jsoncpp/*.h"
	"${INC_DIR}/jsoncpp/*.cpp"
	"${INC_DIR}/jsoncpp/json/*.h"
	"${INC_DIR}/jsoncpp/json/*.cpp"
)

include_directories(${INC_DIR})
include_directories("${INC_DIR}/jsoncpp")
add_library(${PROJECT_NAME} ${LIB_TYPE} ${SRC_LIST})

if(SSVJSONCPP_BUILD_SHARED_LIB)
	set_target_properties(${PROJECT_NAME} PROPERTIES DEBUG_POSTFIX -d)
else()
	set_target_properties(${PROJECT_NAME} PROPERTIES DEBUG_POSTFIX -s-d)
	set_target_properties(${PROJECT_NAME} PROPERTIES RELEASE_POSTFIX -s)
endif()

if(WIN32)
	install(TARGETS ${PROJECT_NAME} RUNTIME DESTINATION bin LIBRARY DESTINATION lib ARCHIVE DESTINATION lib)
else()
	install(TARGETS ${PROJECT_NAME} RUNTIME DESTINATION lib LIBRARY DESTINATION lib ARCHIVE DESTINATION lib)
endif()

install(DIRECTORY ${SRC_DIR} DESTINATION .)
