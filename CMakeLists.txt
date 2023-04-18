cmake_minimum_required(VERSION 3.0.2)
project(ded C)

find_package(PkgConfig)
# find_package(SDL2 REQUIRED)
PKG_SEARCH_MODULE(SDL2 REQUIRED sdl2)
# find_package(GLEW REQUIRED)
# find_package(Freetype REQUIRED)
find_package(OpenGL REQUIRED)

if(MINGW)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -mwindows -s")
    add_definitions(-DMINIRENT_IMPLEMENTATION)
endif(MINGW)

add_subdirectory(${PROJECT_SOURCE_DIR}/Extern/glew-cmake)
add_subdirectory(${PROJECT_SOURCE_DIR}/Extern/freetype)
include_directories(BEFORE ${PROJECT_SOURCE_DIR}/Extern/glew-cmake/include/)
include_directories(BEFORE ${PROJECT_SOURCE_DIR}/Extern/freetype/include/)

set(GLEW_USE_STATIC_LIBS TRUE)
set(glew-cmake_BUILD_SHARED FALSE)

file(GLOB_RECURSE ${PROJECT_NAME}_SRC
    ${PROJECT_SOURCE_DIR}/src/**.c)

include_directories(${SDL2_INCLUDE_DIRS} ${GLEW_INCLUDE_DIRS})

add_executable(${PROJECT_NAME} ${${PROJECT_NAME}_SRC})

target_link_libraries(${PROJECT_NAME} ${SDL2_LIBRARIES} libglew_static freetype ${OPENGL_gl_LIBRARY} ${OPENGL_glu_LIBRARY})
if(MINGW)
    # experiment
    target_link_libraries(${PROJECT_NAME} gdi32 shell32 winmm version setupapi hid)
endif(MINGW)

file(COPY ${PROJECT_SOURCE_DIR}/shaders/ DESTINATION ${CMAKE_BINARY_DIR}/shaders)
file(COPY ${PROJECT_SOURCE_DIR}/fonts/ DESTINATION ${CMAKE_BINARY_DIR}/fonts)
