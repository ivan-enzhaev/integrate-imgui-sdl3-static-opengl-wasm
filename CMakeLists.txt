cmake_minimum_required(VERSION 3.21)

if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Debug")
endif()

project(integrate-imgui-sdl3-wasm CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# -----------------------------------------------------------------------------
# Paths
# -----------------------------------------------------------------------------
set(SDL3_DIR "C:/libs/sdl3-3.4.18-wasm")
set(IMGUI_DIR "C:/libs/imgui-1.93.0")

# -----------------------------------------------------------------------------
# Executable Target (Emscripten generates .js and .wasm)
# -----------------------------------------------------------------------------
add_executable(app
    src/main.cpp
    ${IMGUI_DIR}/imgui.cpp
    ${IMGUI_DIR}/imgui_demo.cpp
    ${IMGUI_DIR}/imgui_draw.cpp
    ${IMGUI_DIR}/imgui_tables.cpp
    ${IMGUI_DIR}/imgui_widgets.cpp
    ${IMGUI_DIR}/backends/imgui_impl_sdl3.cpp
    ${IMGUI_DIR}/backends/imgui_impl_opengl3.cpp
)

# Set output name to app.js
set_target_properties(app PROPERTIES
    OUTPUT_NAME "app"
    SUFFIX ".js"
)

# -----------------------------------------------------------------------------
# Include Directories
# -----------------------------------------------------------------------------
target_include_directories(app PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/src
    ${SDL3_DIR}/include
    ${IMGUI_DIR}
    ${IMGUI_DIR}/backends
)

# -----------------------------------------------------------------------------
# Compile Definitions
# -----------------------------------------------------------------------------
# Enable GLES3 / WebGL2 shaders inside imgui_impl_opengl3
target_compile_definitions(app PRIVATE
    IMGUI_IMPL_OPENGL_ES3
)

# -----------------------------------------------------------------------------
# Link SDL3 Static Library
# -----------------------------------------------------------------------------
target_link_libraries(app PRIVATE
    ${SDL3_DIR}/lib/libSDL3.a
)

# -----------------------------------------------------------------------------
# Emscripten Link Options (WebGL 2.0, Memory Growth, HTML Canvas)
# -----------------------------------------------------------------------------
target_link_options(app PRIVATE
    "-sUSE_WEBGL2=1"
    "-sFULL_ES3=1"
    "-sALLOW_MEMORY_GROWTH=1"
)