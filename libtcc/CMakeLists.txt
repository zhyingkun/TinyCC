cmake_minimum_required(VERSION 3.0)
project(libtcc
	VERSION 0.9.27
	# DESCRIPTION "TCC dynamic library"
	# HOMEPAGE_URL "www.zhyingkun.com"
	LANGUAGES C CXX
)

if(NOT CMAKE_BUILD_TYPE)
	set(CMAKE_BUILD_TYPE "Debug")
endif()
message(STATUS "CMakeLists.txt for ${PROJECT_NAME}")
message(STATUS "CMAKE_BUILD_TYPE is ${CMAKE_BUILD_TYPE}")

if(APPLE)
	set(CMAKE_C_FLAGS         "-Wall -Wextra -DTCC_TARGET_X86_64 -DTCC_TARGET_MACHO -DONE_SOURCE=0 -Wdeclaration-after-statement -fno-strict-aliasing -Wno-pointer-sign -Wno-sign-compare -Wno-unused-result -D_FORTIFY_SOURCE=0")
	set(CMAKE_C_FLAGS_DEBUG   "-g")
	set(CMAKE_C_FLAGS_RELEASE "-O2 -DNDEBUG")
elseif(CMAKE_SYSTEM_NAME MATCHES "Linux")
	set(CMAKE_C_FLAGS         "-Wall -Wextra -DTCC_TARGET_X86_64 -DONE_SOURCE=0 -Wdeclaration-after-statement -fno-strict-aliasing -Wno-pointer-sign -Wno-sign-compare -Wno-unused-result")
	set(CMAKE_C_FLAGS_DEBUG   "-g")
	set(CMAKE_C_FLAGS_RELEASE "-O2 -DNDEBUG")
elseif(WIN32)
	set(CMAKE_C_FLAGS         "-Wall -DLIBTCC_AS_DLL -DTCC_TARGET_X86_64 -DTCC_TARGET_PE -DONE_SOURCE=0 -Wdeclaration-after-statement -fno-strict-aliasing -Wno-pointer-sign -Wno-sign-compare -Wno-unused-result")
	# set(CMAKE_C_FLAGS_DEBUG   "-g")
	set(CMAKE_C_FLAGS_RELEASE "-O2 -DNDEBUG")
endif()

include_directories(./include)
file(GLOB LIBTCC_HEADERS "./include/*.h")

include_directories(./src)
file(GLOB LIBTCC_CORE_HEADERS "./src/*.h")

if(APPLE)
	include_directories(./src/macosx)
	file(GLOB PLATFORM_HEADERS "./src/macosx/*.h")
elseif(CMAKE_SYSTEM_NAME MATCHES "Linux")
	include_directories(./src/linux)
	file(GLOB PLATFORM_HEADERS "./src/linux/*.h")
elseif(WIN32)
	include_directories(./src/win32)
	file(GLOB PLATFORM_HEADERS "./src/win32/*.h")
endif()

aux_source_directory(./src LIBTCC_CORE_SRC)
if(WIN32)
	list(APPEND ${LIBTCC_CORE_SRC} ./src/win32/tccpe.c)
endif(WIN32)

source_group(src FILES ${LIBTCC_CORE_SRC} ${LIBTCC_CORE_HEADERS} ${PLATFORM_HEADERS})
source_group(include FILES ${LIBTCC_HEADERS})

add_library(${PROJECT_NAME} SHARED 
	${LIBTCC_CORE_SRC}
	${LIBTCC_CORE_HEADERS}
	${PLATFORM_HEADERS}
	${LIBTCC_HEADERS}
)
set_target_properties(${PROJECT_NAME} PROPERTIES
	OUTPUT_NAME "tcc"
	VERSION ${PROJECT_VERSION}
	SOVERSION "${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}"
	POSITION_INDEPENDENT_CODE ON
)

if(CMAKE_SYSTEM_NAME MATCHES "Linux")
	target_link_libraries(${PROJECT_NAME} m dl)
endif(CMAKE_SYSTEM_NAME MATCHES "Linux")

install(
	TARGETS ${PROJECT_NAME}
	RUNTIME DESTINATION bin
	LIBRARY DESTINATION lib
	ARCHIVE DESTINATION lib
)
install(FILES ${LIBTCC_HEADERS} DESTINATION include/tcc)
