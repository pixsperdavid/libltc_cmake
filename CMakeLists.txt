cmake_minimum_required(VERSION 3.15)
cmake_policy(SET CMP0091 NEW)

project(libltc VERSION 1.3.2 LANGUAGES C)

option(BUILD_SHARED_LIBS "Build shared libaries" ON)
option(LIBLTC_USE_STATIC_MSVC_RUNTIME "Link against the static MSVC runtime" ON)

include_directories(src)

set(HEADER_FILES 
	src/decoder.h 
	src/encoder.h
	src/ltc.h)
source_group("include" FILES ${HEADER_FILES})

set(SOURCE_FILES 
	src/decoder.c
	src/encoder.c
	src/ltc.c
	src/timecode.c)
source_group("src" FILES ${SOURCE_FILES})

if(BUILD_SHARED_LIBS)
	add_library(${PROJECT_NAME} SHARED ${HEADER_FILES} ${SOURCE_FILES})
else()
	add_library(${PROJECT_NAME} STATIC ${HEADER_FILES} ${SOURCE_FILES})
	target_compile_definitions(${PROJECT_NAME} PUBLIC "libltc_STATIC")
endif()

set_target_properties(${PROJECT_NAME} PROPERTIES PREFIX "")

target_include_directories(${PROJECT_NAME} PUBLIC src)

if (LIBLTC_USE_STATIC_MSVC_RUNTIME)
	set_property(TARGET ${PROJECT_NAME} PROPERTY
             MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")
else()
	set_property(TARGET ${PROJECT_NAME} PROPERTY
             MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>DLL")
endif()