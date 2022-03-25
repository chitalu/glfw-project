cmake_minimum_required( VERSION 3.1 )

project( glfw-project )

find_package( OpenGL REQUIRED )

include_directories( ${OPENGL_INCLUDE_DIRS} )

set( GLFW_BUILD_DOCS OFF CACHE BOOL  "GLFW lib only" )
set( GLFW_INSTALL OFF CACHE BOOL  "GLFW lib only" )
set( GLFW_BUILD_EXAMPLES OFF CACHE BOOL  "GLFW lib only" )
set( GLFW_BUILD_TESTS OFF CACHE BOOL  "GLFW lib only" )

add_subdirectory( extern/glfw )

if( MSVC )
    SET( CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} /ENTRY:mainCRTStartup" )
endif()

set(CMAKE_CXX_STANDARD 11)  

file(GLOB HEADER_FILES *.h)
file(GLOB SRC_FILES *.c *.cpp)
     
add_executable( glfw-project WIN32 ${HEADER_FILES} ${SRC_FILES})

target_link_libraries( glfw-project ${OPENGL_LIBRARIES} glfw )

#
# add extra include directories e.g. "glm" here
#
target_include_directories(glfw-project PRIVATE extern/glm)

if( MSVC )
    if(${CMAKE_VERSION} VERSION_LESS "3.6.0") 
        message( "\n\t[ WARNING ]\n\n\tCMake version lower than 3.6.\n\n\t - Please update CMake and rerun; OR\n\t - Manually set 'glfw-project' as StartUp Project in Visual Studio.\n" )
    else()
        set_property( DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR} PROPERTY VS_STARTUP_PROJECT glfw-project )
    endif()
endif()
