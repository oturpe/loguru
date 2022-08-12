cmake_minimum_required(VERSION 3.22)

project(loguru)

include(GNUInstallDirs)

if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE "Debug" CACHE STRING
      "Choose the type of build, options are: Debug Release RelWithDebInfo MinSizeRel." FORCE)
endif(NOT CMAKE_BUILD_TYPE)

MESSAGE(STATUS "CMAKE_BUILD_TYPE: ${CMAKE_BUILD_TYPE}")

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -Werror -Wall -Wextra -pedantic")

file(GLOB source
    "${CMAKE_CURRENT_SOURCE_DIR}/loguru.cpp"
)

add_library(loguru SHARED ${source})
set_target_properties(loguru PROPERTIES SOVERSION 1)
set_target_properties(loguru PROPERTIES PUBLIC_HEADER loguru.hpp)

find_package(Threads)
target_link_libraries(loguru ${CMAKE_THREAD_LIBS_INIT}) # For pthreads
target_link_libraries(loguru dl) # For ldl

install(TARGETS loguru
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})
install(FILES loguru.pc DESTINATION ${CMAKE_INSTALL_LIBDIR}/pkgconfig)
