cmake_minimum_required(VERSION 4.1)
# Include header files
include_directories("${CMAKE_SOURCE_DIR}/lib/glfw/include")
include_directories("${CMAKE_SOURCE_DIR}/lib")

# Add executable
add_executable(${PROJECT_NAME} main.cpp
        "${CMAKE_SOURCE_DIR}/lib/imgui.cpp"
        "${CMAKE_SOURCE_DIR}/lib/imgui_demo.cpp"
        "${CMAKE_SOURCE_DIR}/lib/imgui_draw.cpp"
        "${CMAKE_SOURCE_DIR}/lib/backends/imgui_impl_glfw.cpp"
        "${CMAKE_SOURCE_DIR}/lib/backends/imgui_impl_opengl3.cpp"
        "${CMAKE_SOURCE_DIR}/lib/imgui_tables.cpp"
        "${CMAKE_SOURCE_DIR}/lib/imgui_widgets.cpp"
        )

# Find OpenGL and set OpenGL_GL_PREFERENCE to GLVND
set(OpenGL_GL_PREFERENCE GLVND)
find_package(OpenGL 3.3 REQUIRED)

# Link OpenGL
target_link_libraries(${PROJECT_NAME} OpenGL::GL)

# Link GLFW
if (WIN32)
    if (MSVC)
        target_link_libraries(${PROJECT_NAME} "${CMAKE_SOURCE_DIR}/dependencies/glfw/lib-vc2019/glfw3.lib")
    elseif (MINGW)
        target_link_libraries(${PROJECT_NAME} "${CMAKE_SOURCE_DIR}/dependencies/glfw/lib-mingw-w64/libglfw3.a")
    endif ()
elseif (UNIX)
    find_package(glfw3 3.3 REQUIRED)
    target_link_libraries(${PROJECT_NAME} glfw ${CMAKE_DL_LIBS})
else ()
    message(FATAL_ERROR "OS not configured in CMakeLists.txt")
endif ()