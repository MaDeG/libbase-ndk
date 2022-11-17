cmake_minimum_required(VERSION 3.10)

if(CMAKE_SOURCE_DIR STREQUAL CMAKE_CURRENT_SOURCE_DIR)
    include("./CMakeListsConan.txt")
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

add_library(libbase STATIC ${LIBBASE_SOURCES})
target_include_directories(libbase PUBLIC ${LIBBASE_ROOT}/include)
target_include_directories(libbase PRIVATE ${LIBBASE_ROOT})

set_property(TARGET libbase PROPERTY CXX_STANDARD 17)

if(CMAKE_CXX_COMPILER_ID MATCHES "Clang")
    target_compile_options(libbase PRIVATE $<BUILD_INTERFACE:-Wno-unknown-attributes>)
endif()