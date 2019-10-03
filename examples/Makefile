.PHONY: run

ARCH := $(shell uname -s)
MAKEFILE_DIR_TMP := $(dir $(abspath $(lastword ${MAKEFILE_LIST})))
# MAKEFILE_DIR := $(patsubst %/,%,${MAKEFILE_DIR_TMP})
PROJECT_DIR := $(patsubst %/examples/,%,${MAKEFILE_DIR_TMP})

EXAMPLE_1_PATH := ${PROJECT_DIR}/examples/ex1.c

ifeq (${ARCH}, Darwin)
TCC_CMD := ${PROJECT_DIR}/buildXcode/tcc/Debug/tcc
LIBTCC_USAGE_CMD := ${PROJECT_DIR}/buildXcode/demo/libtcc-usage/Debug/libtcc-usage
endif

ifeq (${ARCH}, Linux)
TCC_CMD := ${PROJECT_DIR}/build/tcc/tcc
TCC_FLAGS := -L/usr/lib/x86_64-linux-gnu
LIBTCC_USAGE_CMD := ${PROJECT_DIR}/build/demo/libtcc-usage/libtcc-usage
endif

ifeq ($(findstring CYGWIN, ${ARCH}), CYGWIN)
TCC_CMD := ${PROJECT_DIR}/buildVS/install/bin/tcc.exe

TCC_RUNTIME_PATH := ${PROJECT_DIR}/runtime/win32
TCC_RUNTIME_PATH := $(subst \,\\,$(shell cygpath -w -a -p ${TCC_RUNTIME_PATH}))
TCC_INCLUDE_PATH := ${PROJECT_DIR}/runtime/include
TCC_INCLUDE_PATH := $(subst \,\\,$(shell cygpath -w -a -p ${TCC_INCLUDE_PATH}))
TCC_FLAGS := -L${TCC_RUNTIME_PATH} -I${TCC_INCLUDE_PATH}

EXAMPLE_1_PATH := $(subst \,\\,$(shell cygpath -w -a -p ${EXAMPLE_1_PATH}))

LIBTCC_USAGE_CMD := ${PROJECT_DIR}/buildVS/demo/libtcc-usage/Debug/libtcc-usage.exe
endif

run:
	${TCC_CMD} ${TCC_FLAGS} -run ${EXAMPLE_1_PATH}
	${LIBTCC_USAGE_CMD} ${TCC_FLAGS}
