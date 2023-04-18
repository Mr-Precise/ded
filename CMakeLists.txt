cmake_minimum_required(VERSION 3.0.2)
project(ded C)

# find_package(PkgConfig)
# find_package(SDL2 REQUIRED)
# PKG_SEARCH_MODULE(SDL2 REQUIRED sdl2)
# find_package(GLEW REQUIRED)
# find_package(Freetype REQUIRED)
find_package(OpenGL REQUIRED)

if(NOT CMAKE_BUILD_TYPE)
   set(CMAKE_BUILD_TYPE "Release")
   message(STATUS "Build type not specified: defaulting to release.")
endif(NOT CMAKE_BUILD_TYPE)
set(CMAKE_BUILD_TYPE ${CMAKE_BUILD_TYPE} CACHE STRING "")

if(MINGW)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -mwindows -s")
    add_definitions(-DMINIRENT_IMPLEMENTATION)
endif(MINGW)

set(GLEW_USE_STATIC_LIBS CACHE BOOL TRUE)
set(glew-cmake_BUILD_SHARED CACHE BOOL FALSE)

add_subdirectory(${PROJECT_SOURCE_DIR}/Extern/glew-cmake)
add_subdirectory(${PROJECT_SOURCE_DIR}/Extern/freetype)
add_subdirectory(${PROJECT_SOURCE_DIR}/Extern/SDL2)
include_directories(BEFORE ${PROJECT_SOURCE_DIR}/Extern/glew-cmake/include/)
include_directories(BEFORE ${PROJECT_SOURCE_DIR}/Extern/freetype/include/)

# MegaCrutch:
file(MAKE_DIRECTORY ${CMAKE_BINARY_DIR}/Extern/SDL2/include/SDL2)
file(GLOB SDL2_headers
  "${PROJECT_SOURCE_DIR}/Extern/SDL2/include/*.h"
  "${CMAKE_BINARY_DIR}/Extern/SDL2/include/*.h"
)
file(COPY ${SDL2_headers} DESTINATION ${CMAKE_BINARY_DIR}/Extern/SDL2/include/SDL2/)
include_directories(BEFORE ${PROJECT_SOURCE_DIR}/Extern/SDL2/include/)

set(SDL_SHARED CACHE BOOL FALSE)
set(SDL_STATIC CACHE BOOL TRUE)

file(GLOB_RECURSE ${PROJECT_NAME}_SRC
    ${PROJECT_SOURCE_DIR}/src/**.c)

# include_directories(${SDL2_INCLUDE_DIRS} ${GLEW_INCLUDE_DIRS})

add_executable(${PROJECT_NAME} ${${PROJECT_NAME}_SRC})

target_link_libraries(${PROJECT_NAME} SDL2::SDL2main SDL2::SDL2-static libglew_static freetype ${OPENGL_gl_LIBRARY} ${OPENGL_glu_LIBRARY})
if(MINGW)
    # experiment
    target_link_libraries(${PROJECT_NAME} gdi32 shell32 winmm version setupapi hid)
endif(MINGW)

file(COPY ${PROJECT_SOURCE_DIR}/shaders/ DESTINATION ${CMAKE_BINARY_DIR}/shaders)
file(COPY ${PROJECT_SOURCE_DIR}/fonts/ DESTINATION ${CMAKE_BINARY_DIR}/fonts)
