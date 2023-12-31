cmake_minimum_required(VERSION 3.3.0)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)



file(GLOB SOURCES
	src/**.cpp
)

add_library(${PROJECT_NAME} SHARED ${SOURCES})

target_include_directories(${PROJECT_NAME} PUBLIC
    src
)

if (NOT DEFINED ENV{GEODE_SDK})
    message(FATAL_ERROR "Unable to find Geode SDK! Please define GEODE_SDK environment variable to point to Geode")
else()
    message(STATUS "Found Geode: $ENV{GEODE_SDK}")
endif()

add_subdirectory($ENV{GEODE_SDK} $ENV{GEODE_SDK}/build)

CPMAddPackage("gh:matcool/gd-imgui-cocos#cb7290b4b9b529a395330cfbe01cb6a892828215")

if (WIN32)
target_link_libraries(${PROJECT_NAME} imgui-cocos)
elseif (APPLE)
target_link_libraries(${PROJECT_NAME} imgui-cocos "-framework IOKit")
endif()


create_geode_file(${PROJECT_NAME})

add_compile_definitions(MOD_VERSION="v${CMAKE_PROJECT_VERSION}")