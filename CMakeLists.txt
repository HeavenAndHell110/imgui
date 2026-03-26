cmake_minimum_required(VERSION 3.15)
project(ImGui LANGUAGES CXX)

# --- 1. 全局配置 ---
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# 静态运行时 (staticruntime "on")
set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")

# --- 2. 源文件列表 ---
set(IMGUI_SOURCES
    "imconfig.h"
    "imgui.h"
    "imgui.cpp"
    "imgui_draw.cpp"
    "imgui_internal.h"
    "imgui_tables.cpp"
    "imgui_widgets.cpp"
    "imstb_rectpack.h"
    "imstb_textedit.h"
    "imstb_truetype.h"
    "imgui_demo.cpp"
)

# --- 3. 定义静态库 ---
add_library(ImGui STATIC ${IMGUI_SOURCES})

# --- 4. 包含目录 ---
target_include_directories(ImGui PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

# --- 5. 平台特定设置 ---
if(WIN32)
    # Windows 默认行为通常符合 systemversion "latest"
else()
    # 对应 Linux 下的 pic "On"
    set_target_properties(ImGui PROPERTIES POSITION_INDEPENDENT_CODE ON)
endif()

# --- 6. 输出目录设置 ---
# 模拟 bin/%{cfg.buildcfg}/ImGui
set(OUTPUT_SUBDIR "bin/$<CONFIG>/ImGui")
set_target_properties(ImGui PROPERTIES
    ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/${OUTPUT_SUBDIR}"
    LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/${OUTPUT_SUBDIR}"
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/${OUTPUT_SUBDIR}"
)

# 如果需要在 IDE (如 VS) 中显示 Dist 配置，可以这样添加：
set_property(GLOBAL PROPERTY DEBUG_CONFIGURATIONS Debug)
# Dist 在 CMake 中通常直接复用 Release 的优化标志
