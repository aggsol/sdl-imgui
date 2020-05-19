cmake_minimum_required(VERSION 3.8)

project("sdl-imgui" LANGUAGES CXX VERSION 0.1.0)

# Local hack
if(WIN32)
	set(SDL2_DIR "C:\\sketchbook\\SDL2-2.0.12")
	message(WARNING "HACK: Using local SDL2: ${SDL2_DIR}")
endif()

# C++ requirements for all targets
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Create compile_commands.json for cppcheck
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

find_package(SDL2 REQUIRED)
include_directories(${SDL2_INCLUDE_DIRS})

# Add source to this project's executable.
add_executable(sdl-imgui 
	src/main.cpp
	src/imgui_sdl/example.cpp
	src/imgui_sdl/imgui_sdl.cpp
	src/imgui/imgui.cpp 
	src/imgui/imgui_draw.cpp
	src/imgui/imgui_widgets.cpp
	src/imgui/imgui_demo.cpp
)
target_link_libraries(sdl-imgui ${SDL2_LIBRARIES})
target_include_directories(sdl-imgui 
PRIVATE 
	src/imgui/
	src/imgui_sdl
)


# Copy SDL2 DLLs to output folder on Windows
if(WIN32)
	foreach(DLL ${SDL2_DLLS})
		add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD COMMAND
			${CMAKE_COMMAND} -E copy_if_different ${DLL} $<TARGET_FILE_DIR:${PROJECT_NAME}>)
	endforeach()
endif()

message(STATUS "Version = ${PROJECT_VERSION}")