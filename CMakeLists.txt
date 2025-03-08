cmake_minimum_required(VERSION 3.10)

# set the project name
project(GamepadController)

set(CMAKE_CXX_FLAGS "-fPIC -g -pedantic -Wall -Wextra")

if(CUSTOM_JETRACER_UTILS)
    include(CMakePrintHelpers)
    cmake_print_variables(CUSTOM_JETRACER_UTILS)
    include_directories(${CUSTOM_JETRACER_UTILS}/src)
else()
    # make sure we have the necessary submodule
    find_package(Git QUIET)
    if(GIT_FOUND AND EXISTS "${PROJECT_SOURCE_DIR}/.git")
        # Update submodules as needed
        option(GIT_SUBMODULE "Check submodules during build" ON)
        if(GIT_SUBMODULE)
            message(STATUS "Submodule update")
            execute_process(COMMAND ${GIT_EXECUTABLE} submodule update --init --recursive
                            WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
                            RESULT_VARIABLE GIT_SUBMOD_RESULT)
            if(NOT GIT_SUBMOD_RESULT EQUAL "0")
                message(FATAL_ERROR "git submodule update --init failed with ${GIT_SUBMOD_RESULT}, please checkout submodules")
            endif()
        endif()
    endif()

    # add the jetracer utils submodule
    add_subdirectory(JetracerUtils)
    include_directories(JetracerUtils/src)
endif()

include_directories(src)

# Build the actual library
add_library(GamepadController SHARED src/Gamepad.cpp)
target_link_libraries(GamepadController JetracerUtils)
