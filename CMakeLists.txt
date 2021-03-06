cmake_minimum_required(VERSION 2.8)
project(git-prompt)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c99")

if("${CMAKE_C_COMPILER_ID}" MATCHES "Clang")
  set(GP_USE_SANITIZER "Enable the named clang sanitizer!" CACHE STRING "")
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fcolor-diagnostics ${GP_USE_SANITIZER}")
endif()

option(GP_EXPERIMENTAL "Enable experimental features" OFF)
if(${GP_EXPERIMENTAL})
  add_definitions(-DGP_EXPERIMENTAL)
endif()

set(CMAKE_C_FLAGS_DEBUG "${CMAKE_C_FLAGS_DEBUG} -DGP_DEBUG")

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/external/libgit2)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/external/libgit2/include)

add_executable(git-prompt ${CMAKE_CURRENT_SOURCE_DIR}/source/git-prompt.c)
target_link_libraries(git-prompt git2)

install(TARGETS git-prompt DESTINATION bin)
