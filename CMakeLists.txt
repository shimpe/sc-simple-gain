####################################################################################################
# CMakeLists file for Simple Gain
# Generated by Manifest Heap
# 2019-03-21
####################################################################################################

####################################################################################################
# basic project config
cmake_minimum_required(VERSION 3.5)
set(project_name "Simple Gain")
set(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake_modules ${CMAKE_MODULE_PATH})
set(CMAKE_CXX_STANDARD 11)

####################################################################################################
# load modules
include(SuperColliderServerPlugin RESULT_VARIABLE server_plugin_found)
if(NOT server_plugin_found)
    message(FATAL_ERROR "Could not find server plugin functions module")
endif()

include(SuperColliderCompilerConfig RESULT_VARIABLE compiler_config_found)
if(NOT compiler_config_found)
    message(FATAL_ERROR "Could not find compiler config module")
endif()

# Windows - puts redistributable DLLs in install directory
include(InstallRequiredSystemLibraries)

sc_check_sc_path("${SC_PATH}")
message(STATUS "Found SuperCollider: ${SC_PATH}")
set(SC_PATH "${SC_PATH}" CACHE PATH
    "Path to SuperCollider source. Relative paths are treated as relative to this script")

include("${SC_PATH}/SCVersion.txt")
set(SC_VERSION "${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}${PROJECT_VERSION_PATCH}")
message(STATUS "Building plugins for SuperCollider version: ${SC_VERSION}")

# set project here to avoid SCVersion.txt clobbering our version info
project(${project_name})
sc_do_initial_compiler_config() # do after setting project so compiler ID is available

if(CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT OR NOT CMAKE_INSTALL_PREFIX)
    message(WARNING "No install prefix provided, defaulting to $BUILD_DIR/install")
    set(CMAKE_INSTALL_PREFIX "${CMAKE_BINARY_DIR}/install" CACHE PATH "Install prefix" FORCE)
endif()

message(STATUS "Install directory set to: ${CMAKE_INSTALL_PREFIX}")

####################################################################################################
# options
option(SUPERNOVA "Build plugins for supernova" ON)
option(SCSYNTH "Build plugins for scsynth" ON)
option(NATIVE "Optimize for native architecture" OFF)
option(STRICT "Use strict warning flags" OFF)

####################################################################################################
# Begin target SimpleGain

set(SimpleGain_cpp_files
    plugins/SimpleGain/SimpleGain.cpp
    plugins/SimpleGain/SimpleGain.hpp
)
set(SimpleGain_sc_files
    plugins/SimpleGain/SimpleGain.sc
)
set(SimpleGain_schelp_files
    plugins/SimpleGain/SimpleGain.schelp
)

sc_add_server_plugin(
    "SimpleGain/SimpleGain" # desination directory
    "SimpleGain" # target name
    "${SimpleGain_cpp_files}"
    "${SimpleGain_sc_files}"
    "${SimpleGain_schelp_files}"
)

# End target SimpleGain
####################################################################################################

####################################################################################################
# END PLUGIN TARGET DEFINITION
####################################################################################################

message(STATUS "Generating plugin targets done")
