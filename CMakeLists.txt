cmake_minimum_required(VERSION 3.0.2)
project(ded C)

find_package(SDL2 REQUIRED)
find_package(GLEW REQUIRED)
find_package(Freetype REQUIRED)
find_package(OpenGL REQUIRED)

if(MINGW)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -mconsole -s")
endif(MINGW)


file(GLOB_RECURSE ${PROJECT_NAME}_SRC
    ${CMAKE_CURRENT_SOURCE_DIR}/src/**.c)

include_directories(${SDL2_INCLUDE_DIRS} ${GLEW_INCLUDE_DIRS} ${FREETYPE_INCLUDE_DIRS})

add_executable(${PROJECT_NAME} ${${PROJECT_NAME}_SRC})

target_link_libraries(${PROJECT_NAME} ${SDL2_LIBRARIES} ${GLEW_LIBRARIES} ${FREETYPE_LIBRARIES} ${OPENGL_gl_LIBRARY} ${OPENGL_glu_LIBRARY})
# if(MINGW OR WIN32)
#     target_link_libraries(${PROJECT_NAME} imm32 winmm version setupapi hid)
# endif(MINGW OR WIN32)
