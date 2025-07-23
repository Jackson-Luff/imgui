cmake_minimum_required(VERSION 3.10)
cmake_policy(VERSION ${CMAKE_MAJOR_VERSION}.${CMAKE_MINOR_VERSION})

project(IMGUI LANGUAGES CXX)

# --------------------------
# Gather core ImGui sources
# --------------------------
set(IMGUI_SRC
    imgui.cpp
    imgui_demo.cpp
    imgui_draw.cpp
    imgui_tables.cpp
    imgui_widgets.cpp

    backends/imgui_impl_sdlgpu3.cpp
    backends/imgui_impl_sdl3.cpp
)

set(IMGUI_HDRS
    imgui.h
    imconfig.h
    imgui_internal.h

    backends/imgui_impl_sdlgpu3.h
    backends/imgui_impl_sdl3.h
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
add_library(IMGUI STATIC ${IMGUI_SRC} ${IMGUI_HDRS} ${IMGUI_STB_HDRS})

# Organize nicely in IDE
source_group(TREE "${CMAKE_CURRENT_SOURCE_DIR}" FILES
    ${IMGUI_SRC} ${IMGUI_HDRS} ${IMGUI_STB_HDRS})

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

# --------------------------
# Vulkan support (optional)
# --------------------------
if(RENDERING_API STREQUAL "Vulkan")
    target_sources(IMGUI PRIVATE
        backends/imgui_impl_vulkan.cpp
        backends/imgui_impl_vulkan.h
    )
endif()