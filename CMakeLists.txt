cmake_minimum_required(VERSION 3.6 FATAL_ERROR)

set(namespace "idni")
set(PROJECT_SHORT_NAME bdd)
project("${namespace}_${PROJECT_SHORT_NAME}"
	VERSION 0.0.1
	DESCRIPTION "IDNI's bdd library"
)

################################################################################
option(WITH_ARITH "compile with arithmetics support " FALSE)
option(WITH_MMAP  "compile with memory map support" FALSE)
################################################################################
option(BUILD_TESTS "Build library tests" FALSE)
option(BUILD_STATIC_LIBRARY "build static library" FALSE)
option(BUILD_SHARED_LIBRARY "build shared library" FALSE)
set(BUILD_EXECUTABLE FALSE)
set(BUILD_SHARED_EXECUTABLE FALSE)
################################################################################
message(STATUS "WITH_ARITH: ${WITH_ARITH}")
message(STATUS "WITH_MMAP: ${WITH_MMAP}")
message(STATUS "BUILD_TESTS: ${BUILD_TESTS}")
message(STATUS "BUILD_STATIC_LIBRARY: ${BUILD_STATIC_LIBRARY}")
message(STATUS "BUILD_SHARED_LIBRARY: ${BUILD_SHARED_LIBRARY}")
################################################################################

# public headers and sources
set(PROJECT_HEADERS src/bdd.h)
if(WITH_MMAP)
	list(APPEND PROJECT_HEADERS src/memory_map.h)
endif()	
set(PROJECT_SOURCES src/bdd.cpp)	
if(WITH_ARITH)
	list(APPEND PROJECT_SOURCES src/bdd_arith.cpp)
endif()	

# load initialize cmake and load target_setup and exclude functions
list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)
include(idni-common)

if(WITH_ARITH)
	target_compile_definitions(${OBJECT_LIB_NAME} PRIVATE "-DWITH_ARITH")
endif()

if(WITH_MMAP)
	target_compile_definitions(${OBJECT_LIB_NAME} PRIVATE "-DWITH_MMAP")
endif()

if(EMSCRIPTEN_DIR)
	target_compile_definitions(${OBJECT_LIB_NAME}
		PRIVATE "-DEMSCRIPTEN_DIR=${EMSCRIPTEN_DIR}")
endif()

if(BUILD_TESTS)
	add_subdirectory(tests)
endif()	

include(installation)
