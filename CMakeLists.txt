#
# project: pngnq-s9
#
cmake_minimum_required(VERSION 3.15)

set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED ON) #...is required...

project(pngnq-s9)

if(MSVC)
  add_compile_options("/wd5105")
endif()

# add zlib

include_directories(zlib)

# generated files from zlib during build
include_directories(${CMAKE_CURRENT_BINARY_DIR}/zlib)

add_subdirectory(zlib)

#set(ZLIB_ROOT ./zlib)
#find_package(ZLIB REQUIRED)
# find_package(PNG REQUIRED)

# libpng configurable options

set(PNG_BUILD_ZLIB ON CACHE BOOL "" FORCE)
set(PNG_SHARED ON CACHE BOOL "" FORCE)
set(PNG_STATIC ON CACHE BOOL "" FORCE)
set(PNG_EXECUTABLES OFF CACHE BOOL "" FORCE) # we only want lib
set(PNG_TESTS OFF CACHE BOOL "" FORCE) # we only want lib
set(SKIP_INSTALL_ALL OFF CACHE BOOL "" FORCE) # we only want lib

# add libpng

include_directories(libpng)

# generated files from libpng during build
include_directories(${CMAKE_CURRENT_BINARY_DIR}/libpng)

add_subdirectory(libpng)

include_directories(${CMAKE_CURRENT_BINARY_DIR}/src)
set(S9_SOURCES
  src/rwpng.c
  src/colorspace.c
  src/pngnq.c
  src/neuquant32.c
  src/rwpng.c
  src/ya_getopt.c
)

add_executable(pngnq-s9 ${S9_SOURCES})
target_link_libraries(pngnq-s9 PUBLIC png_static zlibstatic)

