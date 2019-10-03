cmake_minimum_required(VERSION 3.0)
project(tcc
	VERSION 0.9.27
	# DESCRIPTION "tcc cmd"
	# HOMEPAGE_URL "www.zhyingkun.com"
	LANGUAGES C CXX
)

if(NOT CMAKE_BUILD_TYPE)
	set(CMAKE_BUILD_TYPE "Debug")
endif()
message(STATUS "CMakeLists.txt for ${PROJECT_NAME}")
message(STATUS "CMAKE_BUILD_TYPE is ${CMAKE_BUILD_TYPE}")

if(APPLE)
	set(CMAKE_C_FLAGS         "-Wall -Wextra -DTCC_TARGET_X86_64 -DTCC_TARGET_MACHO -DONE_SOURCE=0 -Wdeclaration-after-statement -fno-strict-aliasing -Wno-pointer-sign -Wno-sign-compare -Wno-unused-result -D_FORTIFY_SOURCE=0") # -flat_namespace -undefined warning
	set(CMAKE_C_FLAGS_DEBUG   "-g")
	set(CMAKE_C_FLAGS_RELEASE "-O2 -DNDEBUG")
elseif(CMAKE_SYSTEM_NAME MATCHES "Linux")
	set(CMAKE_C_FLAGS         "-Wall -Wextra -DTCC_TARGET_X86_64 -DONE_SOURCE=0 -Wdeclaration-after-statement -fno-strict-aliasing -Wno-pointer-sign -Wno-sign-compare -Wno-unused-result")
	set(CMAKE_C_FLAGS_DEBUG   "-g")
	set(CMAKE_C_FLAGS_RELEASE "-O2 -DNDEBUG")
elseif(WIN32)
	set(CMAKE_C_FLAGS         "-Wall -DTCC_TARGET_X86_64 -DTCC_TARGET_PE -DONE_SOURCE=0 -Wdeclaration-after-statement -fno-strict-aliasing -Wno-pointer-sign -Wno-sign-compare -Wno-unused-result")
	# set(CMAKE_C_FLAGS_DEBUG   "-g")
	set(CMAKE_C_FLAGS_RELEASE "-O2 -DNDEBUG")
endif()

include_directories(../libtcc/include)
include_directories(../libtcc/src)

if(APPLE)
	include_directories(../libtcc/src/macosx)
elseif(CMAKE_SYSTEM_NAME MATCHES "Linux")
	include_directories(../libtcc/src/linux)
elseif(WIN32)
endif()

aux_source_directory(./src TCC_SRC)
source_group(src FILES ${TCC_SRC})

add_executable(${PROJECT_NAME} ./src/tcc.c)
set_target_properties(${PROJECT_NAME} PROPERTIES
	INSTALL_RPATH ${CMAKE_INSTALL_PREFIX}/lib
)
target_link_libraries(${PROJECT_NAME} libtcc)

if(APPLE)
    # target_link_libraries(${PROJECT_NAME} m dl)
elseif(CMAKE_SYSTEM_NAME MATCHES "Linux")
    target_link_libraries(${PROJECT_NAME} m dl)
endif()

install(
	TARGETS ${PROJECT_NAME}
	RUNTIME DESTINATION bin
	LIBRARY DESTINATION lib
	ARCHIVE DESTINATION lib
)
