cmake_minimum_required(VERSION 3.4)

set(TARGET_NAME cpp-starter-template)
project(${TARGET_NAME} VERSION 0.1)

# Export compile_commands.json during build for integration with other tools
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

set(CMAKE_MODULE_PATH ${CMAKE_BINARY_DIR} ${CMAKE_MODULE_PATH})
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

include(cmake/conan.cmake)
include(cmake/clang-format.cmake)
# include(cmake/clang-tidy.cmake)

# Conan and dependencies configuration
conan_add_remote(
    NAME
        catchorg
    URL
        https://api.bintray.com/conan/catchorg/Catch2
)
conan_add_remote(
    NAME
        jkconan-vendor
    URL
        https://api.bintray.com/conan/jan-kelemen/conan-vendor
)

conan_cmake_run(
    REQUIRES
        Catch2/2.12.2@catchorg/stable
    BASIC_SETUP
        CMAKE_TARGETS
    BUILD
        missing
)

enable_testing()

add_compile_options(-W -Wall -Wextra -pedantic)

add_subdirectory(source)

