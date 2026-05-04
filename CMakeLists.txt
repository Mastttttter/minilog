cmake_minimum_required(VERSION 4.3.2)
project(minilog)

if (NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release)
endif()
set(CMAKE_CXX_STANDARD 26)

project(main LANGUAGES CXX)

add_executable(main main.cpp)
target_include_directories(main PUBLIC .)
