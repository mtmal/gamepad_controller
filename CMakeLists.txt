cmake_minimum_required(VERSION 3.10)

# set the project name
project(GamepadController)

set(CMAKE_CXX_FLAGS "-fPIC -g -pedantic -Wall -Wextra")

if(CUSTOM_UTILS)
    include_directories(${CUSTOM_UTILS}/src)
else()
    # add the utils submodule
    add_subdirectory(Utils)
    include_directories(Utils/src)
endif()

include_directories(src)

# Build the actual library
add_library(GamepadController SHARED src/gamepad.cpp)
target_link_libraries(GamepadController pthread)
