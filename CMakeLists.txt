cmake_minimum_required (VERSION 3.20)

# Project name
set (PROJECT_NAME "RKToolkit")

project (${PROJECT_NAME})

# The version number.
set (RKToolkit_PACKAGE_VERSION "1.3")

file (GLOB LOCAL_CPP_FILES src/*.cpp)
set (SOURCE_FILES ${SOURCE_FILES} ${LOCAL_CPP_FILES})

file (GLOB LOCAL_H_FILES inc/*.h)
set (HEADER_FILES ${HEADER_FILES} ${LOCAL_H_FILES})

find_package(PkgConfig REQUIRED)
pkg_search_module(LIBUSB REQUIRED libusb-1.0)
pkg_search_module(LIBUSB REQUIRED libiconv)

# Turn off build log
# set (CMAKE_VERBOSE_MAKEFILE ON)

# Configure a header file to pass some of the CMake settings
# to the source code
configure_file (
    "${PROJECT_SOURCE_DIR}/config.h.in"
    "${PROJECT_BINARY_DIR}/config.h"
    )

# add the binary tree to the search path for include files
# so that we will find config.h
include_directories (
    ${PROJECT_BINARY_DIR}
    ${PROJECT_SOURCE_DIR}
    ${LIBUSB_INCLUDE_DIRS}
    ${LIBICONV_INCLUDE_DIRS}
    )

link_directories (
    ${LIBUSB_LIBRARIES}
    ${LIBICONV_LIBRARIES}
    )

add_executable (${PROJECT_NAME} 
    ${SOURCE_FILES})

target_link_libraries (${PROJECT_NAME}
    ${LIBUSB_LIBRARIES}
    ${LIBICONV_LIBRARIES}
    )

