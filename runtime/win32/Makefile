# Makefile for create tcc runtime library, windows+cygwin only

.PHONY: run clean

ARCH := $(shell uname -s)
MAKEFILE_DIR_TMP := $(dir $(abspath $(lastword ${MAKEFILE_LIST})))
# MAKEFILE_DIR := $(patsubst %/,%,${MAKEFILE_DIR_TMP})
PROJECT_DIR := $(patsubst %/runtime/win32/,%,${MAKEFILE_DIR_TMP})

INCLUDE_DIR_PATH := ${PROJECT_DIR}/runtime/include
INCLUDE_DIR_PATH := $(subst \,\\,$(shell cygpath -w -a -p ${INCLUDE_DIR_PATH}))

BINARY_DIR_PATH := ${PROJECT_DIR}/runtime/win32
BINARY_DIR_PATH := $(subst \,\\,$(shell cygpath -w -a -p ${BINARY_DIR_PATH}))

LIBTCC1_C_PATH := ${PROJECT_DIR}/runtime/lib/libtcc1.c
LIBTCC1_C_PATH := $(subst \,\\,$(shell cygpath -w -a -p ${LIBTCC1_C_PATH}))
BCHECK_C_PATH := ${PROJECT_DIR}/runtime/lib/bcheck.c
BCHECK_C_PATH := $(subst \,\\,$(shell cygpath -w -a -p ${BCHECK_C_PATH}))
ALLOCA86_64_BT_S_PATH := ${PROJECT_DIR}/runtime/lib/alloca86_64-bt.S
ALLOCA86_64_BT_S_PATH := $(subst \,\\,$(shell cygpath -w -a -p ${ALLOCA86_64_BT_S_PATH}))
ALLOCA86_64_S_PATH := ${PROJECT_DIR}/runtime/lib/alloca86_64.S
ALLOCA86_64_S_PATH := $(subst \,\\,$(shell cygpath -w -a -p ${ALLOCA86_64_S_PATH}))

CHKSTK_S_PATH := ${PROJECT_DIR}/runtime/win32/lib/chkstk.S
CHKSTK_S_PATH := $(subst \,\\,$(shell cygpath -w -a -p ${CHKSTK_S_PATH}))
CRT1_C_PATH := ${PROJECT_DIR}/runtime/win32/lib/crt1.c
CRT1_C_PATH := $(subst \,\\,$(shell cygpath -w -a -p ${CRT1_C_PATH}))
CRT1W_C_PATH := ${PROJECT_DIR}/runtime/win32/lib/crt1w.c
CRT1W_C_PATH := $(subst \,\\,$(shell cygpath -w -a -p ${CRT1W_C_PATH}))
WINCRT1_C_PATH := ${PROJECT_DIR}/runtime/win32/lib/wincrt1.c
WINCRT1_C_PATH := $(subst \,\\,$(shell cygpath -w -a -p ${WINCRT1_C_PATH}))
WINCRT1W_C_PATH := ${PROJECT_DIR}/runtime/win32/lib/wincrt1w.c
WINCRT1W_C_PATH := $(subst \,\\,$(shell cygpath -w -a -p ${WINCRT1W_C_PATH}))
DLLCRT1_C_PATH := ${PROJECT_DIR}/runtime/win32/lib/dllcrt1.c
DLLCRT1_C_PATH := $(subst \,\\,$(shell cygpath -w -a -p ${DLLCRT1_C_PATH}))
DLLMAIN_C_PATH := ${PROJECT_DIR}/runtime/win32/lib/dllmain.c
DLLMAIN_C_PATH := $(subst \,\\,$(shell cygpath -w -a -p ${DLLMAIN_C_PATH}))

LIBTCC_DLL_PATH := ${PROJECT_DIR}/buildVS/install/bin/tcc.dll
LIBTCC_DLL_PATH := $(subst \,\\,$(shell cygpath -w -a -p ${LIBTCC_DLL_PATH}))

libtcc1.a:
	${PROJECT_DIR}/buildVS/install/bin/tcc.exe -c ${LIBTCC1_C_PATH} -o libtcc1.o -I${INCLUDE_DIR_PATH} -B${BINARY_DIR_PATH}
	${PROJECT_DIR}/buildVS/install/bin/tcc.exe -c ${BCHECK_C_PATH} -o bcheck.o -I${INCLUDE_DIR_PATH} -B${BINARY_DIR_PATH}
	${PROJECT_DIR}/buildVS/install/bin/tcc.exe -c ${ALLOCA86_64_BT_S_PATH} -o alloca86_64-bt.o -I${INCLUDE_DIR_PATH} -B${BINARY_DIR_PATH}
	${PROJECT_DIR}/buildVS/install/bin/tcc.exe -c ${ALLOCA86_64_S_PATH} -o alloca86_64.o -I${INCLUDE_DIR_PATH} -B${BINARY_DIR_PATH}
	${PROJECT_DIR}/buildVS/install/bin/tcc.exe -c ${CHKSTK_S_PATH} -o chkstk.o -I${INCLUDE_DIR_PATH} -B${BINARY_DIR_PATH}
	${PROJECT_DIR}/buildVS/install/bin/tcc.exe -c ${CRT1_C_PATH} -o crt1.o -I${INCLUDE_DIR_PATH} -B${BINARY_DIR_PATH}
	${PROJECT_DIR}/buildVS/install/bin/tcc.exe -c ${CRT1W_C_PATH} -o crt1w.o -I${INCLUDE_DIR_PATH} -B${BINARY_DIR_PATH}
	${PROJECT_DIR}/buildVS/install/bin/tcc.exe -c ${WINCRT1_C_PATH} -o wincrt1.o -I${INCLUDE_DIR_PATH} -B${BINARY_DIR_PATH}
	${PROJECT_DIR}/buildVS/install/bin/tcc.exe -c ${WINCRT1W_C_PATH} -o wincrt1w.o -I${INCLUDE_DIR_PATH} -B${BINARY_DIR_PATH}
	${PROJECT_DIR}/buildVS/install/bin/tcc.exe -c ${DLLCRT1_C_PATH} -o dllcrt1.o -I${INCLUDE_DIR_PATH} -B${BINARY_DIR_PATH}
	${PROJECT_DIR}/buildVS/install/bin/tcc.exe -c ${DLLMAIN_C_PATH} -o dllmain.o -I${INCLUDE_DIR_PATH} -B${BINARY_DIR_PATH}
	${PROJECT_DIR}/buildVS/install/bin/tcc.exe -ar rcs libtcc1.a libtcc1.o alloca86_64.o alloca86_64-bt.o bcheck.o chkstk.o crt1.o crt1w.o wincrt1.o wincrt1w.o dllcrt1.o dllmain.o
	${PROJECT_DIR}/buildVS/install/bin/tcc.exe -impdef ${LIBTCC_DLL_PATH} -o libtcc.def

EXAMPLE_1_PATH := ${PROJECT_DIR}/examples/ex1.c
EXAMPLE_1_PATH := $(subst \,\\,$(shell cygpath -w -a -p ${EXAMPLE_1_PATH}))

run:
	${PROJECT_DIR}/buildVS/demo/libtcc-usage/Debug/libtcc-usage.exe -I../include -L.
	${PROJECT_DIR}/buildVS/install/bin/tcc.exe -I../include -L. -run ${EXAMPLE_1_PATH}

clean:
	rm -rf *.o *.a *.def
