cmake_minimum_required(VERSION 3.16)
project(PhysicsSimV5 LANGUAGES CXX)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
option(BUILD_SHARED_LIBS "Build shared libraries" OFF)

include(FetchContent)
FetchContent_Declare(SFML
    GIT_REPOSITORY https://github.com/SFML/SFML.git
    GIT_TAG 2.6.x)
FetchContent_MakeAvailable(SFML)

add_executable(PhysicsSimV5 src/main.cpp)
target_link_libraries(PhysicsSimV5 PRIVATE sfml-graphics)
target_compile_features(PhysicsSimV5 PRIVATE cxx_std_17)

if(WIN32)
    add_custom_command(
        TARGET PhysicsSimV5
        COMMENT "Copy OpenAL DLL"
        PRE_BUILD COMMAND ${CMAKE_COMMAND} -E copy ${SFML_SOURCE_DIR}/extlibs/bin/$<IF:$<EQUAL:${CMAKE_SIZEOF_VOID_P},8>,x64,x86>/openal32.dll $<TARGET_FILE_DIR:PhysicsSimV5>
        VERBATIM)
endif()

install(TARGETS PhysicsSimV5)
