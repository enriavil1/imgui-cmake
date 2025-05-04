# Font files
file(GLOB FONTS_FILES ${IMGUI_SOURCE_DIR}/misc/fonts/*.ttf)

# Platform-specific files (e.g., OS X, SDL3, SDLRenderer3, OpenGL3)
if(CMAKE_SYSTEM_NAME MATCHES "Darwin")
  list(APPEND IMGUI_HEADERS ${IMGUI_BACKEND_DIR}/imgui_impl_osx.h)

  list(APPEND IMGUI_SOURCES ${IMGUI_BACKEND_DIR}/imgui_impl_osx.mm)
endif()

# Common backend files
if(USE_SDL)
  list(APPEND IMGUI_HEADERS ${IMGUI_BACKEND_DIR}/imgui_impl_sdl3.h
       ${IMGUI_BACKEND_DIR}/imgui_impl_sdlrenderer3.h)

  list(APPEND IMGUI_SOURCES ${IMGUI_BACKEND_DIR}/imgui_impl_sdl3.cpp
       ${IMGUI_BACKEND_DIR}/imgui_impl_sdlrenderer3.cpp)
else()
  list(APPEND IMGUI_HEADERS ${IMGUI_BACKEND_DIR}/imgui_impl_glfw.h)
  list(APPEND IMGUI_SOURCES ${IMGUI_BACKEND_DIR}/imgui_impl_glfw.cpp)
endif()

list(APPEND IMGUI_HEADERS ${IMGUI_BACKEND_DIR}/imgui_impl_opengl3.h
     ${IMGUI_BACKEND_DIR}/imgui_impl_opengl3_loader.h)

list(APPEND IMGUI_SOURCES ${IMGUI_BACKEND_DIR}/imgui_impl_opengl3.cpp)

# Combine header and source files
set(PROJECT_FILES ${IMGUI_HEADERS} ${IMGUI_SOURCES} ${FONTS_FILES})

# Output build messages
message(STATUS "[${PROJECT_NAME}] Building library")

# Create the ImGui library target
add_library(${PROJECT_NAME} STATIC ${PROJECT_FILES})

if(USE_SDL)
  # Link ImGui library with SDL3
  target_link_libraries(${PROJECT_NAME} PUBLIC SDL3::SDL3)
else()
  target_link_libraries(${PROJECT_NAME} PUBLIC glfw)
endif()

# Set include directories for ImGui
target_include_directories(
  ${PROJECT_NAME} PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}"
                         "${CMAKE_CURRENT_SOURCE_DIR}/backends")
