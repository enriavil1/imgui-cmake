cmake_minimum_required(VERSION 3.10)
project(imgui)

# Set the directory for ImGui backends
set(IMGUI_BACKEND_DIR ${CMAKE_CURRENT_SOURCE_DIR}/backends)

# List header and source files explicitly
set(IMGUI_HEADERS imgui.h imconfig.h imgui_internal.h imstb_rectpack.h
                  imstb_textedit.h imstb_truetype.h)

set(IMGUI_SOURCES imgui.cpp imgui_draw.cpp imgui_widgets.cpp imgui_demo.cpp
                  imgui_tables.cpp)

# Font files
file(GLOB FONTS_FILES ${IMGUI_SOURCE_DIR}/misc/fonts/*.ttf)

# Platform-specific files (e.g., OS X, SDL3, SDLRenderer3, OpenGL3)
if(${OPERATING_SYSTEM} STREQUAL "Apple")
  list(APPEND IMGUI_HEADERS ${IMGUI_BACKEND_DIR}/imgui_impl_osx.h)

  list(APPEND IMGUI_SOURCES ${IMGUI_BACKEND_DIR}/imgui_impl_osx.mm)
endif()

# Common backend files
list(
  APPEND
  IMGUI_HEADERS
  ${IMGUI_BACKEND_DIR}/imgui_impl_sdl3.h
  ${IMGUI_BACKEND_DIR}/imgui_impl_sdlrenderer3.h
  ${IMGUI_BACKEND_DIR}/imgui_impl_opengl3.h
  ${IMGUI_BACKEND_DIR}/imgui_impl_opengl3_loader.h)

list(APPEND IMGUI_SOURCES ${IMGUI_BACKEND_DIR}/imgui_impl_sdl3.cpp
     ${IMGUI_BACKEND_DIR}/imgui_impl_sdlrenderer3.cpp
     ${IMGUI_BACKEND_DIR}/imgui_impl_opengl3.cpp)

# Combine header and source files
set(PROJECT_FILES ${IMGUI_HEADERS} ${IMGUI_SOURCES} ${FONTS_FILES})

# Output build messages
message(STATUS "[${PROJECT_NAME}] Building library")

# Create the ImGui library target
add_library(${PROJECT_NAME} STATIC ${PROJECT_FILES})

# Link ImGui library with SDL3
target_link_libraries(${PROJECT_NAME} PUBLIC SDL3::SDL3)

# Set include directories for ImGui
target_include_directories(
  ${PROJECT_NAME} PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}"
                         "${CMAKE_CURRENT_SOURCE_DIR}/backends")
