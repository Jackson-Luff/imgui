cmake_minimum_required(VERSION 3.10 FATAL_ERROR)
cmake_policy(VERSION ${CMAKE_VERSION})

project(IMGUI LANGUAGES CXX)
## Find dependencies

file(GLOB SOURCE
    "imgui.h"
    "imgui.cpp"
    "imconfig.h"
    "imgui_demo.cpp"
    "imgui_draw.cpp"
    "imgui_internal.h"
    "imstb_rectpack.h"
    "imstb_textedit.h"
    "imstb_truetype.h"
    "imgui_tables.cpp"
    "imgui_widgets.cpp"
)

if(RENDERING_API MATCHES "Vulkan")
    list(APPEND SOURCE
    "backends/imgui_impl_vulkan.h"
    "backends/imgui_impl_vulkan.cpp"
    )
endif()

add_library(IMGUI STATIC ${SOURCE})
source_group(TREE "${CMAKE_CURRENT_SOURCE_DIR}" FILES ${SOURCE})
set_target_properties(IMGUI PROPERTIES FOLDER "Dependencies")

target_include_directories(IMGUI 
PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}
PRIVATE
)

target_link_libraries(IMGUI
PRIVATE
)
