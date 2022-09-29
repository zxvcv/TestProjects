cmake_minimum_required(VERSION 3.16)

set(PROJECT_NAME Test1)
project(${PROJECT_NAME})

include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
conan_basic_setup()

add_executable(${PROJECT_NAME}
    src/main.c
)

target_link_libraries(${PROJECT_NAME}
    ${CONAN_LIBS}
)
