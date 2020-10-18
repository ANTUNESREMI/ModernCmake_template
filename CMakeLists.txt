#--------------------------------------------------------------------
# CMake Version
#--------------------------------------------------------------------
cmake_minimum_required(VERSION 3.12...3.18)

if( ${CMAKE_VERSION} VERSION_LESS 3.12 )
    cmake_policy(VERSION ${CMAKE_MAJOR_VERSION}.${CMAKE_MINOR_VERSION})
endif()

#--------------------------------------------------------------------
# Properties
#--------------------------------------------------------------------

# C Properties
set(CMAKE_C_COMPILER "gcc")
set( CMAKE_POSITION_INDEPENDENT_CODE ON )
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS}")

# C++ Properties
#set(CMAKE_CXX_COMPILER "g++")
#set(CMAKE_CXX_STANDARD 17)
#set(CMAKE_CXX_STANDARD_REQUIRED ON)
#set(CMAKE_CXX_EXTENSIONS OFF)

include( "${CMAKE_CURRENT_LIST_DIR}/cmake/project-meta-info.in" )

#--------------------------------------------------------------------
# Project
#--------------------------------------------------------------------
project(    MyProject
    VERSION ${project_version} 
    DESCRIPTION ${project_description} 
    HOMEPAGE_URL ${project_homepage} 
    # C Project
    LANGUAGES C
    # C++ Project
    #LANGUAGES CXX
)

#--------------------------------------------------------------------
# Add subdirectories
#--------------------------------------------------------------------
add_subdirectory( apps )

# Library subdirectory
add_subdirectory( lib ) 

#--------------------------------------------------------------------
# Package
#--------------------------------------------------------------------

# Example find a package
#find_package( OpenGL REQUIRED) #OpenGL

#--------------------------------------------------------------------
# Program Core
#--------------------------------------------------------------------

file(GLOB_RECURSE SOURCE "${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp")
file(GLOB_RECURSE HEADER "${CMAKE_CURRENT_SOURCE_DIR}/include/*.h")

#--------------------------------------------------------------------
# Object Library
#--------------------------------------------------------------------
add_library( source_ObjLib OBJECT )

target_sources( source_ObjLib
    PRIVATE 
        ${SOURCE}
    PUBLIC 
        ${HEADER} 
)

#--------------------------------------------------------------------
# Object Library Include
#--------------------------------------------------------------------
target_include_directories( source_ObjLib 
    PUBLIC 
        "${CMAKE_CURRENT_SOURCE_DIR}/include"
        #include your library include here
)

#--------------------------------------------------------------------
# Object Library Compile Features
#--------------------------------------------------------------------       
target_compile_features( source_ObjLib 
    PUBLIC 
        # C Project 
        c_std_99
        # C++ Project
        #cxx_std_17
        #cxx_constexpr 
)

#--------------------------------------------------------------------
# Include Libraries
#--------------------------------------------------------------------
target_link_libraries( source_ObjLib 
    PUBLIC
        # add your libraries
)

#--------------------------------------------------------------------
# Create Shared Library
#--------------------------------------------------------------------
add_library( source SHARED )

target_link_libraries( source 
    PUBLIC 
        source_ObjLib 
)

#--------------------------------------------------------------------
# Create Target
#--------------------------------------------------------------------
add_library( 
    Source::source ALIAS source 
)
