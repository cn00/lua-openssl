cmake_minimum_required(VERSION 3.13)

include("../cmake/source_group_by_dir.cmake")

project(openssl)

# add_subdirectory(third_party/flatbuffers)
# add_subdirectory(../lua binary_dir EXCLUDE_FROM_ALL)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -L${CMAKE_CURRENT_SOURCE_DIR} -std=gnu++11")

file(GLOB_RECURSE lopenssl_sources ./src/*.c ./src/*.h deps/*.c ./deps/*.h
    # openssl/engines/*.c 
    # openssl/include/*.h
    )
list(FILTER lopenssl_sources EXCLUDE REGEX "deps/.*test.*")


source_group_by_dir(${CMAKE_CURRENT_SOURCE_DIR} lopenssl_sources)


include_directories(
  ${CMAKE_CURRENT_SOURCE_DIR}/
  ${CMAKE_CURRENT_SOURCE_DIR}/src
  ${CMAKE_CURRENT_SOURCE_DIR}/deps/auxiliar
  ${CMAKE_CURRENT_SOURCE_DIR}/deps/lua-compat
  ${CMAKE_CURRENT_SOURCE_DIR}/deps/lua-compat/c-api
  ${CMAKE_CURRENT_SOURCE_DIR}/../lua
  ${CMAKE_CURRENT_SOURCE_DIR}/openssl/include
)

link_directories(
    # ${CMAKE_LIBRARY_OUTPUT_DIRECTORY}
    ${CMAKE_CURRENT_SOURCE_DIR}/openssl/
    /Volumes/Data/test/xlua/build.OSX/bin/Debug
)

# add_library(openssl SHARED ssl)
add_library(openssl SHARED ${lopenssl_sources})
target_link_libraries(openssl lua ssl crypto)

