cmake_minimum_required(VERSION 3.28)

project(cpp_dev_environment
    VERSION 1.0
    LANGUAGES CXX
)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# -----------------------------------------------------------------------------
# Main executable
# -----------------------------------------------------------------------------

add_executable(cpp_dev_environment
    src/main.cpp
)

target_include_directories(cpp_dev_environment PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/include
)

if(MSVC)
    # MSVC-specific flags
    set(COMPILE_FLAGS /W4)
else()
    # GCC/Clang flags
    set(COMPILE_FLAGS -Wall -Wextra -Wpedantic)
endif()

target_compile_options(cpp_dev_environment PRIVATE ${COMPILE_FLAGS})

# -----------------------------------------------------------------------------
# Testing
# -----------------------------------------------------------------------------

include(CTest)
enable_testing()

include(FetchContent)

FetchContent_Declare(
    googletest
    URL https://github.com/google/googletest/archive/refs/tags/v1.17.0.zip
)

# Prevent GoogleTest from overriding our compiler/linker settings on Windows.
set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)

FetchContent_MakeAvailable(googletest)

add_executable(cpp_tests
    tests/main_test.cpp
)

target_link_libraries(cpp_tests PRIVATE
    GTest::gtest_main
)

target_include_directories(cpp_tests PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/include
)

target_compile_options(cpp_tests PRIVATE ${COMPILE_FLAGS})

include(GoogleTest)

gtest_discover_tests(cpp_tests)
