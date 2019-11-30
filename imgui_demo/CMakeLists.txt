project(imgui_demo)

add_executable(imgui_demo
    "main.cpp"
)

target_link_libraries(imgui_demo
    SDL2
    glad
    imgui
)