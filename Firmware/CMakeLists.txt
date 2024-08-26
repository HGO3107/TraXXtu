cmake_minimum_required(VERSION 3.13)


#set the targe_file executable name to variable NAME
set(NAME TRT_OUT)

#include build functions from pic SDK
include(pico_sdk_import.cmake)

#set name of project and languages that excutable file can process 
project(${NAME} C CXX ASM)

# sets up everything needed to use the Pico SDK, by creating a sub directory in my project(in this cas my buld directory)
pico_sdk_init()

#define executable and it source file
add_executable(${PROJECT_NAME}  ${CMAKE_SOURCE_DIR}/src/main.cpp
                                ${CMAKE_SOURCE_DIR}/src/hw_config.c)

#tell cmake where to find other source files
add_subdirectory(${CMAKE_SOURCE_DIR}/lib/no-OS-FatFS-SD-SPI-RPi-Pico/FatFs_SPI build)

#link to pico_stdlib (gpio, timers and so on) and SD libraries
target_link_libraries(${PROJECT_NAME} 
                pico_stdlib
                FatFs_SPI
                        )

#Enable usb and disbale uart
pico_enable_stdio_usb(${PROJECT_NAME} 1)
pico_enable_stdio_uart(${PROJECT_NAME} 0)


#create map/bin/hex/uf2 files
pico_add_extra_outputs(${PROJECT_NAME})


