cmake_minimum_required(VERSION 3.8)
project(screen_info)

if (POLICY CMP0058)
	# Ninja requires custom command byproducts to be explicit
	cmake_policy(SET CMP0058 NEW)
endif()

set (CMAKE_CXX_STANDARD 17)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${PROJECT_SOURCE_DIR}/cmake_modules/")

# this function preppends a path to all files in a list
FUNCTION(PREPEND var prefix)
SET(listVar "")
   FOREACH(f ${ARGN})
   LIST(APPEND listVar "${prefix}/${f}")
   ENDFOREACH(f)
   SET(${var} "${listVar}" PARENT_SCOPE)
ENDFUNCTION(PREPEND)

#set(SDL2_USE_STATIC_LIBS ON)
find_package(SDL2)

find_package(OpenGL REQUIRED)
if(OPENGL_FOUND)
	include_directories(${OPENGL_INCLUDE_DIRS})
	link_libraries(${OPENGL_LIBRARIES})
endif()

add_subdirectory(libs/glad)
add_subdirectory(libs/imgui)

if(SDL2_FOUND)
	include_directories(${SDL2_INCLUDE_DIR})
else()
	include_directories(${SDL2_SOURCE_DIR}/include)
endif()

set(SOURCES "main.cpp")
PREPEND(SOURCES "src/" ${SOURCES})

add_executable(screen_info
    ${SOURCES}
)

target_link_libraries(screen_info SDL2)
target_link_libraries(screen_info glad)
target_link_libraries(screen_info imgui)

add_subdirectory(imgui_demo)