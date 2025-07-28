cmake_minimum_required(VERSION 3.10)
cmake_policy(VERSION ${CMAKE_MAJOR_VERSION}.${CMAKE_MINOR_VERSION})

project(IMGUI LANGUAGES CXX)
message(STATUS "Configuring IMGUI")

# --------------------------
# Gather sources files
# --------------------------
set(IMGUI_SOURCE
    imconfig.h
    imgui.h
    imgui.cpp
    imgui_internal.h
    imgui_demo.cpp
    imgui_draw.cpp
    imgui_tables.cpp
    imgui_widgets.cpp
)

# stb headers (not compiled, but needed)
set(IMGUI_STB_HDRS
    imstb_rectpack.h
    imstb_textedit.h
    imstb_truetype.h
)

# --------------------------
# Add ImGui library
# --------------------------
add_library(IMGUI STATIC ${IMGUI_SOURCE} ${IMGUI_STB_HDRS})

# Organize nicely in IDE
source_group(TREE ${CMAKE_CURRENT_SOURCE_DIR} FILES
    ${IMGUI_SOURCE} ${IMGUI_STB_HDRS})

# SDLGPU3 support (optional)
if( USE_SDLGPU3 )
    target_sources(IMGUI PRIVATE
    backends/imgui_impl_sdlgpu3.h
    backends/imgui_impl_sdlgpu3.cpp
    backends/imgui_impl_sdl3.h
    backends/imgui_impl_sdl3.cpp
    )
endif()

set_target_properties(IMGUI PROPERTIES
    FOLDER "Dependencies"
    CXX_STANDARD 17
    CXX_STANDARD_REQUIRED YES
)

# --------------------------
# Include directories
# --------------------------
target_include_directories(IMGUI PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}
)

# --------------------------
# External linking
# --------------------------
target_link_libraries(IMGUI PRIVATE
    SDL3_image::SDL3_image
    SDL3::SDL3
)