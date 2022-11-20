if (POLICY CMP0048)
    cmake_policy(SET CMP0048 NEW)
endif (POLICY CMP0048)

cmake_minimum_required(VERSION 3.20)

set(CMAKE_VERBOSE_MAKEFILE ON)

# Require out-of-source builds
file(TO_CMAKE_PATH "${PROJECT_BINARY_DIR}/CMakeLists.txt" LOC_PATH)
if(EXISTS "${LOC_PATH}")
    message(FATAL_ERROR "You cannot build in a source directory (or any directory with a CMakeLists.txt file). Please make a build subdirectory. Feel free to remove CMakeCache.txt and CMakeFiles.")
endif()

# Set a default build type if none was specified
if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
  message(STATUS "Setting build type to 'Release' as none was specified.")
  set(CMAKE_BUILD_TYPE Release CACHE STRING "Choose the type of build." FORCE)
  # Set the possible values of build type for cmake-gui
  set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "Debug" "Release" "MinSizeRel" "RelWithDebInfo")
endif()

project(libbase LANGUAGES CXX)

set(LIBBASE_ROOT ${CMAKE_CURRENT_SOURCE_DIR}/../)

set(LIBBASE_SOURCES ${LIBBASE_ROOT}/abi_compatibility.cpp
                    ${LIBBASE_ROOT}/chrono_utils.cpp
                    ${LIBBASE_ROOT}/cmsg.cpp
                    ${LIBBASE_ROOT}/file.cpp
                    ${LIBBASE_ROOT}/hex.cpp
                    ${LIBBASE_ROOT}/logging.cpp
                    ${LIBBASE_ROOT}/mapped_file.cpp
                    ${LIBBASE_ROOT}/parsebool.cpp
                    ${LIBBASE_ROOT}/parsenetaddress.cpp
                    ${LIBBASE_ROOT}/posix_strerror_r.cpp
                    ${LIBBASE_ROOT}/process.cpp
                    ${LIBBASE_ROOT}/properties.cpp
                    ${LIBBASE_ROOT}/stringprintf.cpp
                    ${LIBBASE_ROOT}/strings.cpp
                    ${LIBBASE_ROOT}/threads.cpp
                    ${LIBBASE_ROOT}/test_utils.cpp
)

add_library(base STATIC ${LIBBASE_SOURCES})
target_include_directories(base PUBLIC ${LIBBASE_ROOT}/include)
target_include_directories(base PRIVATE ${LIBBASE_ROOT})

set_property(TARGET base PROPERTY CXX_STANDARD 17)

if(CMAKE_CXX_COMPILER_ID MATCHES "Clang")
    target_compile_options(base PRIVATE $<BUILD_INTERFACE:-Wno-unknown-attributes>)
endif()