cmake_minimum_required(VERSION 3.0)
project(libtcc-usage
	VERSION 0.1.0
	# DESCRIPTION "Demo for using libtcc"
	# HOMEPAGE_URL "www.zhyingkun.com"
	LANGUAGES C CXX
)

if(NOT CMAKE_BUILD_TYPE)
	set(CMAKE_BUILD_TYPE "Debug")
endif()
message(STATUS "CMakeLists.txt for ${PROJECT_NAME}")
message(STATUS "CMAKE_BUILD_TYPE is ${CMAKE_BUILD_TYPE}")

if(APPLE)
	set(CMAKE_C_FLAGS         "-Wall -Wextra -fno-strict-aliasing -D_FORTIFY_SOURCE=0") #  -flat_namespace -undefined warning
	set(CMAKE_C_FLAGS_DEBUG   "-g")
	set(CMAKE_C_FLAGS_RELEASE "-O2 -DNDEBUG")
elseif(CMAKE_SYSTEM_NAME MATCHES "Linux")
	set(CMAKE_C_FLAGS         "-Wall -Wextra -fno-strict-aliasing")
	set(CMAKE_C_FLAGS_DEBUG   "-g")
	set(CMAKE_C_FLAGS_RELEASE "-O2 -DNDEBUG")
elseif(WIN32)
	set(CMAKE_C_FLAGS         "-Wall")
	# set(CMAKE_C_FLAGS_DEBUG   "-g")
	set(CMAKE_C_FLAGS_RELEASE "-O2 -DNDEBUG")
endif()

include_directories(../../libtcc/include) # for libtcc.h

aux_source_directory(./src LIBTCC_USAGE_SRC)
source_group(src FILES ${LIBTCC_USAGE_SRC})

add_executable(${PROJECT_NAME} ${LIBTCC_USAGE_SRC})
target_link_libraries(${PROJECT_NAME} libtcc)
if(APPLE)
    # target_link_libraries(${PROJECT_NAME} m dl)
elseif(CMAKE_SYSTEM_NAME MATCHES "Linux")
    target_link_libraries(${PROJECT_NAME} m dl)
endif()
set_target_properties(${PROJECT_NAME} PROPERTIES
	FOLDER "demo"
	INSTALL_RPATH ${CMAKE_INSTALL_PREFIX}/lib)

# install(TARGETS ${PROJECT_NAME}
# 	RUNTIME DESTINATION bin
# 	LIBRARY DESTINATION lib
# 	ARCHIVE DESTINATION lib
# )
