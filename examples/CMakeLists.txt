cmake_minimum_required(VERSION 3.0)
project(examples
	VERSION 0.1.0
	# DESCRIPTION "Examples from tcc"
	# HOMEPAGE_URL "www.zhyingkun.com"
	LANGUAGES C CXX
)

if(NOT CMAKE_BUILD_TYPE)
	set(CMAKE_BUILD_TYPE "Debug")
endif()
message(STATUS "CMakeLists.txt for ${PROJECT_NAME}")
message(STATUS "CMAKE_BUILD_TYPE is ${CMAKE_BUILD_TYPE}")

if(APPLE)
	set(CMAKE_C_FLAGS         "-Wall -Wextra")
	set(CMAKE_C_FLAGS_DEBUG   "-g")
	set(CMAKE_C_FLAGS_RELEASE "-O2 -DNDEBUG")
elseif(CMAKE_SYSTEM_NAME MATCHES "Linux")
	set(CMAKE_C_FLAGS         "-Wall -Wextra")
	set(CMAKE_C_FLAGS_DEBUG   "-g")
	set(CMAKE_C_FLAGS_RELEASE "-O2 -DNDEBUG")
elseif(WIN32)
	set(CMAKE_C_FLAGS         "-Wall")
	# set(CMAKE_C_FLAGS_DEBUG   "-g")
	set(CMAKE_C_FLAGS_RELEASE "-O2 -DNDEBUG")
endif()

include_directories(../runtime/include) # for tcclib.h

aux_source_directory(. EXAMPLES_SRC)
source_group(src FILES ${EXAMPLES_SRC})

add_executable(ex3 ex3.c)
set_target_properties(ex3 PROPERTIES
	FOLDER "examples"
)

# Install libtcc1.a in /usr/local/zyk/lib/tcc/libtcc1.a
# and install headers in /usr/local/zyk/lib/tcc/include
# config path in config.h
# tcc will find it automatically

# install(TARGETS ${PROJECT_NAME}
# 	RUNTIME DESTINATION bin
# 	LIBRARY DESTINATION lib
# 	ARCHIVE DESTINATION lib
# )
