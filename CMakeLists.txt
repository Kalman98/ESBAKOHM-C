cmake_minimum_required(VERSION 2.8)

project(OpenGL)

option(ASSIMP_BUILD_ASSIMP_TOOLS OFF)
option(ASSIMP_BUILD_SAMPLES OFF)
option(ASSIMP_BUILD_TESTS OFF)
add_subdirectory(lib/assimp)

add_library(glad lib/glad/src/glad.c)

option(GLFW_BUILD_DOCS OFF)
option(GLFW_BUILD_EXAMPLES OFF)
option(GLFW_BUILD_TESTS OFF)
add_subdirectory(lib/glfw)

include_directories(
    lib/assimp/include/
    ${CMAKE_BINARY_DIR}/lib/assimp/include
    lib/glad/include/
    lib/glfw/include/
    lib/glm/
    lib/stb/
    )

file(GLOB PROJECT_HEADERS *.h)
file(GLOB PROJECT_SOURCES *.cpp)

add_executable(${PROJECT_NAME} ${PROJECT_SOURCES} ${PROJECT_HEADERS})

target_link_libraries(${PROJECT_NAME} assimp glad glfw)

if(MSVC)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /W4")
else()
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra -Wpedantic -std=c++14")
endif()

set (source "${CMAKE_SOURCE_DIR}/resources")
set (destination "${CMAKE_CURRENT_BINARY_DIR}/resources")
add_custom_command(
 TARGET ${PROJECT_NAME} POST_BUILD
 COMMAND ${CMAKE_COMMAND} -E create_symlink ${source} ${destination}
 DEPENDS ${destination}
 COMMENT "symbolic link resources folder from ${source} => ${destination}"
)
