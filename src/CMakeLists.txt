add_executable(crane
    main.cpp
    render.cpp
    render.h
)

target_compile_definitions(crane PRIVATE _CRT_SECURE_NO_WARNINGS=1)
message("OPENGL_LIBRARIES: ${OPENGL_LIBRARIES}")
message("OPENGL_LIBRARY: ${OPENGL_LIBRARY}")
target_link_libraries(crane glad imgui_sdl2 ${OPENGL_LIBRARIES})
