cmake_minimum_required( VERSION 3.1.0 )

project( cJSON C )

SET(CMAKE_INSTALL_PREFIX "" CACHE PATH "Don't want an install target" FORCE)

# We do things my submodules.
set( cJSON_TRUE_SOURCE_DIR "${cJSON_SOURCE_DIR}/cJSON" )

set( cJSON_INCLUDE_DIR ${cJSON_TRUE_SOURCE_DIR}/include )
set( cJSON_SOURCES ${cJSON_TRUE_SOURCE_DIR}/cJSON.c )

set( cJSON_LINK "" )
set( cJSON_DEFINES "-DTARGE_BASE=1" )

set( cJSON_INCLUDE_DIR_PRIVATE "" )
set( cJSON_LINK_PRIVATE "" )
set( cJSON_DEFINES_PRIVATE "-DTARGET_BASE_BUILD=1" )
set( cJSON_COMPILE_FLAGS_PRIVATE "" )

if( MSVC )
	list( APPEND cJSON_DEFINES_PRIVATE 
		"-D_CRT_SECURE_NO_WARNINGS" 
	)
	
	list( APPEND cJSON_COMPILE_FLAGS_PRIVATE 
		"/wd4068"
	)
	
endif()

add_library( cJSON STATIC ${cJSON_SOURCES} )

target_include_directories( cJSON PRIVATE ${cJSON_INCLUDE_DIR} )
target_compile_definitions( cJSON PRIVATE ${cJSON_DEFINES} ${cJSON_DEFINES_PRIVATE} )
target_link_libraries( cJSON PRIVATE ${cJSON_LINK} )
target_compile_options( cJSON PRIVATE ${cJSON_COMPILE_FLAGS_PRIVATE} )

target_include_directories( cJSON INTERFACE $<BUILD_INTERFACE:${cJSON_INCLUDE_DIR}> )
target_compile_definitions( cJSON INTERFACE ${cJSON_DEFINES} )
target_link_libraries( cJSON INTERFACE ${cJSON_LINK} )