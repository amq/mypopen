cmake_minimum_required(VERSION 2.8.7)
find_package(Doxygen)

project(mypopen)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=gnu99 -Wall -Wextra -Wstrict-prototypes -pedantic")
set(CMAKE_C_FLAGS_DEBUG "-g -O0 -fprofile-arcs -ftest-coverage")
set(CMAKE_EXE_LINKER_FLAGS="-fprofile-arcs -ftest-coverage")

add_library(MYPOPEN src/mypopen.c src/mypopen.h)
add_library(LIBPOPENUTILS tests/libpopentest/utils.c tests/libpopentest/utils.h)

add_executable(killparent tests/libpopentest/killparent.c)
target_link_libraries(killparent LIBPOPENUTILS)

add_executable(checkopenfds tests/libpopentest/checkopenfds.c)
target_link_libraries(checkopenfds LIBPOPENUTILS)

add_executable(libpopentest tests/libpopentest/popentest.c tests/libpopentest/mypopen.h)
target_link_libraries(libpopentest MYPOPEN LIBPOPENUTILS)

add_executable(test-pipe tests/test-pipe/test-pipe.c)
target_link_libraries(test-pipe MYPOPEN LIBPOPENUTILS)

add_executable(sandbox tests/sandbox/sandbox.c)
target_link_libraries(sandbox MYPOPEN)

if(DOXYGEN_FOUND)
    add_custom_target(doc
            WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
            COMMAND ${DOXYGEN_EXECUTABLE})
endif()