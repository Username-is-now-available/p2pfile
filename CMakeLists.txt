cmake_minimum_required(VERSION 3.15)
project(p2pfile LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Enable Qt's automoc, autouic, and autorcc BEFORE find_package and add_executable
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTORCC ON)

# Find Qt Widgets module
find_package(Qt6 REQUIRED COMPONENTS Widgets)

# Gather sources and headers
file(GLOB_RECURSE SOURCES src/*.cpp)
file(GLOB_RECURSE HEADERS include/*.hpp)

add_executable(${PROJECT_NAME} ${SOURCES} ${HEADERS})

target_include_directories(${PROJECT_NAME} PRIVATE include)

# Link Qt Widgets library
target_link_libraries(${PROJECT_NAME} PRIVATE Qt6::Widgets)

# Platform-specific definitions and libs
if(WIN32)
    target_compile_definitions(${PROJECT_NAME} PRIVATE PLATFORM_WINDOWS)

    # Link Windows sockets library
    target_link_libraries(${PROJECT_NAME} PRIVATE ws2_32)

    # Example: only add this flag if GCC is used on Windows (rare)
    if (CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
        target_compile_options(${PROJECT_NAME} PRIVATE -mno-direct-extern-accePss)
    endif()
elseif(UNIX)
    target_compile_definitions(${PROJECT_NAME} PRIVATE PLATFORM_LINUX)
endif()
