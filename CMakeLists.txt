cmake_minimum_required(VERSION 3.21)
project(BlackHoleSim)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(GLEW REQUIRED)
find_package(glfw3 CONFIG REQUIRED)
find_package(glm CONFIG REQUIRED)
find_package(OpenGL REQUIRED)

set(DEPS glfw GLEW::GLEW glm::glm OpenGL::GL)

# ==========================
# Build ONLY the 3D version
# ==========================

add_executable(BlackHole3D black_hole.cpp)

target_link_libraries(BlackHole3D PRIVATE ${DEPS})

target_include_directories(BlackHole3D PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}
)

file(GLOB SHADERS
    "${CMAKE_CURRENT_SOURCE_DIR}/*.vert"
    "${CMAKE_CURRENT_SOURCE_DIR}/*.frag"
    "${CMAKE_CURRENT_SOURCE_DIR}/*.comp"
)

foreach(shader ${SHADERS})
    add_custom_command(TARGET BlackHole3D POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_if_different
            ${shader}
            $<TARGET_FILE_DIR:BlackHole3D>
    )
endforeach()
