# Copyright (c) 2020-2022 Pawel Piskorz
# Licensed under the Eclipse Public License 2.0
# See attached LICENSE file

cmake_minimum_required(VERSION 3.16.3)


#####################################   INCLUDES   ################################################
include(../cmake/f_compile_definitions.cmake)
include(../cmake/f_compile_options.cmake)


#####################################   CREATE LOCAL TARGET HAL_DRIVERS   #########################
# file(GLOB HAL_SOURCE_FILES "STM32F3xx_HAL_Driver/Src/stm32f3xx_hal*.c")
add_library(HAL_Driver
    STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_tim.c
    STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_tim_ex.c
    STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_uart.c
    STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_uart_ex.c
    STM32F3xx_HAL_Driver/Src/stm32f3xx_hal.c
    STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_rcc.c
    STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_rcc_ex.c
    STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_gpio.c
    STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_dma.c
    STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_cortex.c
    STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_pwr.c
    STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_pwr_ex.c
    STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_flash.c
    STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_flash_ex.c
    STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_i2c.c
    STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_i2c_ex.c
    STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_exti.c
    # ${HAL_SOURCE_FILES}
)

target_include_directories(HAL_Driver PUBLIC
    STM32F3xx_HAL_Driver/Inc
    STM32F3xx_HAL_Driver/Inc/Legacy

    CMSIS/Device/ST/STM32F3xx/Include
    CMSIS/Core/Include
    ../Core/Inc
)

#####################################   COMPILATION SETTINGS   ####################################
target_compile_options(HAL_Driver PRIVATE
    -mcpu=cortex-m4
    -mthumb
    -mfpu=fpv4-sp-d16
    -mfloat-abi=hard

    -fdata-sections
    -ffunction-sections

    -Wall

    $<$<CONFIG:Debug>:-Og>
)
target_compile_definitions(HAL_Driver PRIVATE
    -DUSE_HAL_DRIVER
    -DSTM32F303xE
)
