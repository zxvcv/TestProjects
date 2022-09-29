cmake_minimum_required(VERSION 3.16)

set(PROJECT_NAME tests_embd1)
project(${PROJECT_NAME})

enable_language(C ASM)
set(CMAKE_C_STANDARD 99)
set(CMAKE_C_STANDARD_REQUIRED ON)
set(CMAKE_C_EXTENSIONS OFF)

include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
conan_basic_setup()

# enable_language(C ASM) #?
# set(CMAKE_C_STANDARD 99) #?
# set(CMAKE_C_STANDARD_REQUIRED ON) #?
# set(CMAKE_C_EXTENSIONS OFF) #?
set(LDSCRIPT STM32F303RETx_FLASH.ld)

add_subdirectory(Drivers) # HAL_Driver

add_executable(${PROJECT_NAME}
    Core/Src/main.c
    Core/Src/stm32f3xx_hal_msp.c
    Core/Src/stm32f3xx_it.c
    Core/Src/system_stm32f3xx.c

    Core/Inc/main.h
    Core/Inc/stm32f3xx_hal_conf.h
    Core/Inc/stm32f3xx_it.h

    startup_stm32f303xe.s
)

target_include_directories(${PROJECT_NAME} PUBLIC
    Core/Inc
)


target_link_libraries(${PROJECT_NAME} PUBLIC
    ${CONAN_LIBS}
    HAL_Driver
)

#####################################   COMPILATION SETTINGS   ####################################
include(cmake/f_compile_definitions.cmake)
include(cmake/f_compile_options.cmake)

set_target_compile_definitions(${PROJECT_NAME})
set_target_compile_options(${PROJECT_NAME})

target_link_options(${PROJECT_NAME} PRIVATE
    -T${CMAKE_SOURCE_DIR}/${LDSCRIPT}
    -mcpu=cortex-m4
    -mthumb
    -mfpu=fpv4-sp-d16
    -mfloat-abi=hard
    -specs=nano.specs
    -specs=nosys.specs
    -lc
    -lm
    -lnosys
    -Wl,-Map=${PROJECT_NAME}.map,--cref
    -Wl,--gc-sections
    -u _printf_float
)

#####################################   PRINT EXECUTABLE SIZE   ###################################
add_custom_command(TARGET ${PROJECT_NAME}
    POST_BUILD
    COMMAND arm-none-eabi-size bin/${PROJECT_NAME}
)

# #####################################   CREATE HEX FILE   ###################################
add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD
    COMMAND arm-none-eabi-objcopy -O ihex bin/${PROJECT_NAME} bin/${PROJECT_NAME}.hex
    COMMAND arm-none-eabi-objcopy -O binary bin/${PROJECT_NAME} bin/${PROJECT_NAME}.bin
)
