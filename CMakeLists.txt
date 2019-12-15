# Define the project name
project(SDL2-prototype)

# Set the cmake minimum version to 3.5.1
cmake_minimum_required(VERSION 3.5.1)

set(projectDir  "${CMAKE_CURRENT_LIST_DIR}")
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_DEBUG "${projectDir}/cmake-build-debug")
# Add SDL2 CMake modules
#list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake/sdl2)

# Add all c++ source files under the src directory
file(GLOB SOURCES "src/*.cc")

add_executable(${PROJECT_NAME} ${SOURCES})

# Add all headers files under the include directory
target_include_directories(${PROJECT_NAME} PRIVATE include)

# Add compiler errors/warnings flags
target_compile_options(${PROJECT_NAME} PRIVATE $<$<C_COMPILER_ID:MSVC>:/W4 /WX>)
target_compile_options(${PROJECT_NAME} PRIVATE $<$<NOT:$<C_COMPILER_ID:MSVC>>:-Wall -Wextra -pedantic -Werror>)

if(MINGW)
    set(VIDEO_OPENGLES OFF CACHE STRING "")
endif()

if(NOT ANDROID)
    find_package(OpenGL REQUIRED)
endif()

if(MINGW)
    # -Link standard libs statically to reduce dll clutter.
    # -lmingw32 is needed to make WinMain not disappear when linking.
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -static -static-libgcc")
    # Only show the console in debug builds and also strip unused dependencies on MinSizeRel.
    set(CMAKE_EXE_LINKER_FLAGS_DEBUG "-mconsole")
    set(CMAKE_EXE_LINKER_FLAGS_RELEASE "-mwindows")
    set(CMAKE_EXE_LINKER_FLAGS_MINSIZEREL "-mwindows -s")
    target_link_libraries(${PROJECT_NAME}
            -lmingw32
            )
endif()

add_definitions(-DSDL_JOYSTICK=0)

set(sdlDir "${projectDir}/contrib/SDL")
add_subdirectory(${sdlDir} EXCLUDE_FROM_ALL)
set(SDL2MAIN_LIBRARY SDL2main)
set(SDL2_LIBRARY SDL2)

set(projectIncludeDirs ${projectIncludeDirs}
        "${sdlDir}/include"
        )

message("     Sources: ${projectSources}\n")
message("Include dirs: ${projectIncludeDirs}\n")

target_link_libraries(${PROJECT_NAME}
        ${SDL2MAIN_LIBRARY}
        ${SDL2_LIBRARY}
        ${OPENGL_LIBRARY}
        )

target_include_directories(${PROJECT_NAME}
        PUBLIC ${projectIncludeDirs}
        )