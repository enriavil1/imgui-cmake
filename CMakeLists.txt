cmake_minimum_required(VERSION 3.10)
project(imgui)

set(IMGUI_BACKEND_DIR ${CMAKE_CURRENT_SOURCE_DIR}/backends)

# Specifying files for better cache
set(H_FILES
  ${CMAKE_CURRENT_SOURCE_DIR}/imgui.h
  ${CMAKE_CURRENT_SOURCE_DIR}/imconfig.h
  ${CMAKE_CURRENT_SOURCE_DIR}/imgui_internal.h
  ${CMAKE_CURRENT_SOURCE_DIR}/imstb_rectpack.h
  ${CMAKE_CURRENT_SOURCE_DIR}/imstb_textedit.h
  ${CMAKE_CURRENT_SOURCE_DIR}/imstb_truetype.h
)

set(CXX_FILES
  ${CMAKE_CURRENT_SOURCE_DIR}/imgui.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/imgui_draw.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/imgui_widgets.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/imgui_demo.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/imgui_tables.cpp
)

# This is not so bueno for cache but its small
file(GLOB FONTS_FILES ${IMGUI_SOURCE_DIR}/misc/fonts/*.ttf)

set(IMGUI_INTERFACE_LIBS "")

if(${OPERATING_SYSTEM} STREQUAL "Apple")
    message(STATUS "[${PROJECT_NAME}] Found apple operating system adding apple specific files")
    list(APPEND H_FILES "${IMGUI_BACKEND_DIR}/imgui_impl_osx.h")
    list(APPEND H_FILES "${IMGUI_BACKEND_DIR}/imgui_impl_osx.mm")
endif()

# Common files
list(APPEND H_FILES "${IMGUI_BACKEND_DIR}/imgui_impl_sdl3.h")
list(APPEND CXX_FILES "${IMGUI_BACKEND_DIR}/imgui_impl_sdl3.cpp")

list(APPEND H_FILES "${IMGUI_BACKEND_DIR}/imgui_impl_sdlrenderer3.h")
list(APPEND CXX_FILES "${IMGUI_BACKEND_DIR}/imgui_impl_sdlrenderer3.cpp")

list(APPEND H_FILES "${IMGUI_BACKEND_DIR}/imgui_impl_opengl3.h")
list(APPEND H_FILES "${IMGUI_BACKEND_DIR}/imgui_impl_opengl3_loader.h")
list(APPEND CXX_FILES "${IMGUI_BACKEND_DIR}/imgui_impl_opengl3.cpp")

set(PROJECT_FILES
  ${H_FILES}
  ${CXX_FILES}
  ${FONTS_FILES}
)

message(STATUS "[${PROJECT_NAME}] Building STATIC library")
add_library(${PROJECT_NAME} STATIC ${PROJECT_FILES})

target_include_directories(${PROJECT_NAME} PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}" "${CMAKE_CURRENT_SOURCE_DIR}/backends")
target_compile_definitions(imgui PUBLIC IMGUI_IMPL_OPENGL_LOADER_CUSTOM=<SDL_opengl.h>  GL_GLEXT_PROTOTYPES=1)
