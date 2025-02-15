
# This file was gererated by the build system used internally in the Yandex monorepo.
# Only simple modifications are allowed (adding source-files to targets, adding simple properties
# like target_include_directories). These modifications will be ported to original
# ya.make files by maintainers. Any complex modifications which can't be ported back to the
# original buildsystem will not be accepted.


cmake_minimum_required(VERSION 3.15)
project(CATBOOST LANGUAGES ASM C CXX)

set(BUILD_SHARED_LIBS Off)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_EXTENSIONS Off)
set(CATBOOST_MAX_LINK_JOBS 5 CACHE STRING "Maximum parallel link jobs for Ninja generator")
set_property(GLOBAL PROPERTY JOB_POOLS link_jobs=${CATBOOST_MAX_LINK_JOBS})
set(CMAKE_JOB_POOL_LINK link_jobs)

list(APPEND CMAKE_MODULE_PATH ${CMAKE_BINARY_DIR} ${CMAKE_SOURCE_DIR}/cmake)
include_directories(${CMAKE_SOURCE_DIR} ${CMAKE_BINARY_DIR})
list(APPEND CMAKE_CTEST_ARGUMENTS "--output-on-failure")
enable_testing()
include(cmake/archive.cmake)
include(cmake/conan.cmake)
include(cmake/cuda.cmake)
include(cmake/cython.cmake)
include(cmake/fat_object.cmake)
include(cmake/fbs.cmake)
include(cmake/global_flags.cmake)
include(cmake/protobuf.cmake)
include(cmake/swig.cmake)
include(cmake/yandex_common.cmake)
include(cmake/global_vars.cmake)

if (CMAKE_CROSSCOMPILING)
  include(${CMAKE_BINARY_DIR}/conan_paths.cmake)
else()
  conan_cmake_autodetect(settings)
  conan_cmake_install(
    PATH_OR_REFERENCE ${CMAKE_SOURCE_DIR}
    INSTALL_FOLDER ${CMAKE_BINARY_DIR}
    BUILD missing
    REMOTE conancenter
    SETTINGS ${settings}
  )
endif()

if (CMAKE_SYSTEM_PROCESSOR STREQUAL "x86_64" AND UNIX AND NOT APPLE AND NOT ANDROID)
  include(CMakeLists.linux.txt)
elseif (CMAKE_SYSTEM_PROCESSOR STREQUAL "aarch64" AND UNIX AND NOT APPLE AND NOT ANDROID)
  include(CMakeLists.linux-aarch64.txt)
elseif (APPLE)
  include(CMakeLists.darwin.txt)
elseif (ANDROID AND CMAKE_ANDROID_ARCH STREQUAL "arm")
  include(CMakeLists.android-arm.txt)
elseif (ANDROID AND CMAKE_ANDROID_ARCH STREQUAL "arm64")
  include(CMakeLists.android-arm64.txt)
elseif (ANDROID AND CMAKE_ANDROID_ARCH STREQUAL "x86")
  include(CMakeLists.android-x86.txt)
elseif (ANDROID AND CMAKE_ANDROID_ARCH STREQUAL "x86_64")
  include(CMakeLists.android-x86_64.txt)
endif()
