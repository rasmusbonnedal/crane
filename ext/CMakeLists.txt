set_property(GLOBAL PROPERTY USE_FOLDERS ON)

# https://github.com/glfw/glfw
set(GLFW_BUILD_EXAMPLES OFF CACHE BOOL "")
set(GLFW_BUILD_TESTS OFF CACHE BOOL "")
set(GLFW_BUILD_DOCS OFF CACHE BOOL "")
set(GLFW_INSTALL OFF CACHE BOOL "")
add_subdirectory(glfw)
target_include_directories(glfw PUBLIC glfw/include)
if (APPLE)
   find_library(COCOA_LIBRARY Cocoa)
   find_library(IOKIT_LIBRARY IOKit)
   target_link_libraries(glfw INTERFACE ${COCOA_LIBRARY} ${IOKIT_LIBRARY})
endif ()
set_target_properties(glfw PROPERTIES FOLDER "ext/GLFW3")
set_target_properties(update_mappings PROPERTIES FOLDER "ext/GLFW3")

# https://github.com/libsdl-org/SDL
set(SDL_STATIC ON CACHE BOOL "" FORCE)
set(SDL_SHARED OFF CACHE BOOL "" FORCE)
set(SDL2_DISABLE_UNINSTALL ON CACHE BOOL "" FORCE)
set(SDL_TEST OFF CACHE BOOL "" FORCE)
add_subdirectory(SDL)
set_target_properties(SDL2-static PROPERTIES FOLDER "ext")
set_target_properties(SDL2main PROPERTIES FOLDER "ext")
set_target_properties(sdl_headers_copy PROPERTIES FOLDER "ext")

# https://github.com/ocornut/imgui
find_package(OpenGL REQUIRED)
add_library(imgui
    imgui/imgui.cpp
    imgui/imgui_demo.cpp
    imgui/imgui_draw.cpp
    imgui/imgui_tables.cpp
    imgui/imgui_widgets.cpp
    imgui/misc/cpp/imgui_stdlib.cpp
)
target_include_directories(imgui PUBLIC imgui)
target_compile_definitions(imgui PUBLIC IMGUI_DEFINE_MATH_OPERATORS)
set_target_properties(imgui PROPERTIES FOLDER "ext")

add_library(imgui_sdl2
    imgui/backends/imgui_impl_sdl2.cpp
    imgui/backends/imgui_impl_opengl3.cpp
)
target_include_directories(imgui_sdl2 PUBLIC imgui/backends)
target_link_libraries(imgui_sdl2 imgui SDL2::SDL2 SDL2main ${OPENGL_LIBRARY})
set_target_properties(imgui_sdl2 PROPERTIES FOLDER "ext")

add_library(imgui_glfw
    imgui/backends/imgui_impl_glfw.cpp
    imgui/backends/imgui_impl_opengl3.cpp
)
target_include_directories(imgui_glfw PUBLIC imgui/backends)
target_link_libraries(imgui_glfw imgui glfw ${OPENGL_LIBRARY})
set_target_properties(imgui_glfw PROPERTIES FOLDER "ext")

add_executable(example_glfw_opengl3 imgui/examples/example_glfw_opengl3/main.cpp)
target_link_libraries(example_glfw_opengl3 imgui_glfw)
set_target_properties(example_glfw_opengl3 PROPERTIES FOLDER "ext")

# https://github.com/g-truc/glm
add_subdirectory(glm)

# https://github.com/imakris/glatter
