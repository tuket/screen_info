project(glad)

add_library(glad STATIC include/glad/glad.h src/glad.c)

target_include_directories(glad PUBLIC include)

if(UNIX)
	target_link_libraries(glad m dl pthread)
endif()