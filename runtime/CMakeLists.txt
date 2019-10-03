cmake_minimum_required(VERSION 3.0)
project(runtime)

if(NOT CMAKE_BUILD_TYPE)
	set(CMAKE_BUILD_TYPE "Debug")
endif()
message(STATUS "CMakeLists.txt for ${PROJECT_NAME}")
message(STATUS "CMAKE_BUILD_TYPE is ${CMAKE_BUILD_TYPE}")

# message(STATUS ${CMAKE_CURRENT_SOURCE_DIR}) # current CMakeLists.txt path, the same with ${PROJECT_SOURCE_DIR}
# message(STATUS ${CMAKE_SOURCE_DIR}) # root project path
# message(STATUS ${CMAKE_CURRENT_BINARY_DIR})
set(TCC_RUNTIME_LIB_PATH ${CMAKE_SOURCE_DIR}/runtime/lib)
set(TCC_RT_WIN32_LIB_PATH ${CMAKE_SOURCE_DIR}/runtime/win32/lib)

set(TCC_RUNTIME_LIB_SRC
	${TCC_RUNTIME_LIB_PATH}/libtcc1.c
	${TCC_RUNTIME_LIB_PATH}/alloca86_64-bt.S
	${TCC_RUNTIME_LIB_PATH}/alloca86_64.S
)

if(APPLE)
	list(APPEND TCC_RUNTIME_LIB_SRC
		${TCC_RUNTIME_LIB_PATH}/va_list.c
	)
elseif(CMAKE_SYSTEM_NAME MATCHES "Linux")
	list(APPEND TCC_RUNTIME_LIB_SRC
		${TCC_RUNTIME_LIB_PATH}/va_list.c
		${TCC_RUNTIME_LIB_PATH}/bcheck.c
	)
elseif(WIN32)
	list(APPEND TCC_RUNTIME_LIB_SRC
		${TCC_RUNTIME_LIB_PATH}/bcheck.c
		${TCC_RT_WIN32_LIB_PATH}/crt1.c
		${TCC_RT_WIN32_LIB_PATH}/crt1w.c
		${TCC_RT_WIN32_LIB_PATH}/wincrt1.c
		${TCC_RT_WIN32_LIB_PATH}/wincrt1w.c
		${TCC_RT_WIN32_LIB_PATH}/dllcrt1.c
		${TCC_RT_WIN32_LIB_PATH}/dllmain.c
		${TCC_RT_WIN32_LIB_PATH}/chkstk.S
	)
endif()

source_group(src FILES ${TCC_RUNTIME_LIB_SRC})

if(APPLE)
	add_custom_target(libtcc1
		COMMAND tcc -c ${TCC_RUNTIME_LIB_PATH}/libtcc1.c -o libtcc1.o -D_ANSI_SOURCE -B${CMAKE_SOURCE_DIR}/runtime
		COMMAND tcc -c ${TCC_RUNTIME_LIB_PATH}/va_list.c -o va_list.o -D_ANSI_SOURCE -B${CMAKE_SOURCE_DIR}/runtime
		COMMAND tcc -c ${TCC_RUNTIME_LIB_PATH}/alloca86_64-bt.S -o alloca86_64-bt.o -D_ANSI_SOURCE -B${CMAKE_SOURCE_DIR}/runtime
		COMMAND tcc -c ${TCC_RUNTIME_LIB_PATH}/alloca86_64.S -o alloca86_64.o -D_ANSI_SOURCE -B${CMAKE_SOURCE_DIR}/runtime
		COMMAND tcc -ar rcs libtcc1.a libtcc1.o alloca86_64.o alloca86_64-bt.o va_list.o
		DEPENDS tcc
		WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
		SOURCES ${TCC_RUNTIME_LIB_SRC}
	)
elseif(CMAKE_SYSTEM_NAME MATCHES "Linux")
	add_custom_target(libtcc1
		COMMAND tcc -c ${TCC_RUNTIME_LIB_PATH}/libtcc1.c -o libtcc1.o -D_ANSI_SOURCE -B${CMAKE_SOURCE_DIR}/runtime
		COMMAND tcc -c ${TCC_RUNTIME_LIB_PATH}/va_list.c -o va_list.o -D_ANSI_SOURCE -B${CMAKE_SOURCE_DIR}/runtime
		COMMAND tcc -c ${TCC_RUNTIME_LIB_PATH}/bcheck.c -o bcheck.o -D_ANSI_SOURCE -B${CMAKE_SOURCE_DIR}/runtime
		COMMAND tcc -c ${TCC_RUNTIME_LIB_PATH}/alloca86_64-bt.S -o alloca86_64-bt.o -D_ANSI_SOURCE -B${CMAKE_SOURCE_DIR}/runtime
		COMMAND tcc -c ${TCC_RUNTIME_LIB_PATH}/alloca86_64.S -o alloca86_64.o -D_ANSI_SOURCE -B${CMAKE_SOURCE_DIR}/runtime
		COMMAND tcc -ar rcs libtcc1.a libtcc1.o alloca86_64.o alloca86_64-bt.o va_list.o  bcheck.o
		DEPENDS tcc
		WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
		SOURCES ${TCC_RUNTIME_LIB_SRC}
	)
elseif(WIN32)
	add_custom_target(libtcc1
		COMMAND tcc -c ${TCC_RUNTIME_LIB_PATH}/libtcc1.c -o libtcc1.o -I${CMAKE_SOURCE_DIR}/runtime/include -B${CMAKE_SOURCE_DIR}/runtime/win32
		COMMAND tcc -c ${TCC_RUNTIME_LIB_PATH}/bcheck.c -o bcheck.o -I${CMAKE_SOURCE_DIR}/runtime/include -B${CMAKE_SOURCE_DIR}/runtime/win32
		COMMAND tcc -c ${TCC_RUNTIME_LIB_PATH}/alloca86_64-bt.S -o alloca86_64-bt.o -I${CMAKE_SOURCE_DIR}/runtime/include -B${CMAKE_SOURCE_DIR}/runtime/win32
		COMMAND tcc -c ${TCC_RUNTIME_LIB_PATH}/alloca86_64.S -o alloca86_64.o -I${CMAKE_SOURCE_DIR}/runtime/include -B${CMAKE_SOURCE_DIR}/runtime/win32
		COMMAND tcc -c ${TCC_RT_WIN32_LIB_PATH}/chkstk.S -o chkstk.o -I${CMAKE_SOURCE_DIR}/runtime/include -B${CMAKE_SOURCE_DIR}/runtime/win32
		COMMAND tcc -c ${TCC_RT_WIN32_LIB_PATH}/crt1.c -o crt1.o -I${CMAKE_SOURCE_DIR}/runtime/include -B${CMAKE_SOURCE_DIR}/runtime/win32
		COMMAND tcc -c ${TCC_RT_WIN32_LIB_PATH}/crt1w.c -o crt1w.o -I${CMAKE_SOURCE_DIR}/runtime/include -B${CMAKE_SOURCE_DIR}/runtime/win32
		COMMAND tcc -c ${TCC_RT_WIN32_LIB_PATH}/wincrt1.c -o wincrt1.o -I${CMAKE_SOURCE_DIR}/runtime/include -B${CMAKE_SOURCE_DIR}/runtime/win32
		COMMAND tcc -c ${TCC_RT_WIN32_LIB_PATH}/wincrt1w.c -o wincrt1w.o -I${CMAKE_SOURCE_DIR}/runtime/include -B${CMAKE_SOURCE_DIR}/runtime/win32
		COMMAND tcc -c ${TCC_RT_WIN32_LIB_PATH}/dllcrt1.c -o dllcrt1.o -I${CMAKE_SOURCE_DIR}/runtime/include -B${CMAKE_SOURCE_DIR}/runtime/win32
		COMMAND tcc -c ${TCC_RT_WIN32_LIB_PATH}/dllmain.c -o dllmain.o -I${CMAKE_SOURCE_DIR}/runtime/include -B${CMAKE_SOURCE_DIR}/runtime/win32
		COMMAND tcc -ar rcs libtcc1.a libtcc1.o alloca86_64.o alloca86_64-bt.o bcheck.o chkstk.o crt1.o crt1w.o wincrt1.o wincrt1w.o dllcrt1.o dllmain.o
		COMMAND tcc -impdef libtcc -o libtcc.def
		DEPENDS tcc libtcc
		WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
		SOURCES ${TCC_RUNTIME_LIB_SRC}
	)
endif()

install(
	FILES ${CMAKE_CURRENT_BINARY_DIR}/libtcc1.a
	DESTINATION lib/tcc
)
file(GLOB TCC_RUNTIME_HEADERS "./include/*.h")
install(
	FILES ${TCC_RUNTIME_HEADERS}
	DESTINATION lib/tcc/include
)
