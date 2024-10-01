# == DO NEVER EDIT THE NEXT LINES for Raspberry Pi Pico VS Code Extension to work ==
if(WIN32)
    set(USERHOME $ENV{USERPROFILE})
else()
    set(USERHOME $ENV{HOME})
endif()
set(sdkVersion 2.0.0)
set(toolchainVersion 13_2_Rel1)
set(picotoolVersion 2.0.0)
set(picoVscode ${USERHOME}/.pico-sdk/cmake/pico-vscode.cmake)
if (EXISTS ${picoVscode})
    include(${picoVscode})
endif()
# ====================================================================================
# == DO NEVER EDIT THE NEXT LINES for Raspberry Pi Pico VS Code Extension to work ==
if(WIN32)
    set(USERHOME $ENV{USERPROFILE})
else()
    set(USERHOME $ENV{HOME})
endif()
set(sdkVersion 2.0.0)
set(toolchainVersion 13_2_Rel1)
set(picotoolVersion 2.0.0)
set(picoVscode ${USERHOME}/.pico-sdk/cmake/pico-vscode.cmake)
if (EXISTS ${picoVscode})
    include(${picoVscode})
endif()
# ====================================================================================

cmake_minimum_required(VERSION 3.13)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# ====================================================================================
set(PICO_BOARD pico2 CACHE STRING "Board type")

# Pull in PICO SDK (must be before project)
include(pico_sdk_import.cmake)

# The following line controls which app is built
# Set it to PICO_DISPLAY_1 to create the pico_display version
# Set it to PICO_DISPLAY_2 to create the pico_display_2 version WITH TEXT + GRAPHICS
# Set it to PICO_DISPLAY_2_MANDELBROT to create the pico_display version WITH FRACTAL

set(SOURCE_FILES examples/pico_display_28/pico_display_28_demo.cpp)

project(picodisp28 C CXX ASM)
add_executable(${PROJECT_NAME} ${SOURCE_FILES})

# Initialize the SDK
pico_sdk_init()

include_directories(
  ${CMAKE_CURRENT_LIST_DIR}
)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Werror")

pico_enable_stdio_uart(${PROJECT_NAME} 0)
pico_enable_stdio_usb(${PROJECT_NAME} 1)

# Add libraries + drivers etc
add_subdirectory(common)
add_subdirectory(drivers)
add_subdirectory(libraries)

# Pull in pico libraries that we need
target_link_libraries(${PROJECT_NAME} pico_stdlib hardware_spi hardware_pwm hardware_dma rgbled pico_graphics st7789 button)

target_link_libraries(${PROJECT_NAME} pico_display_28)
#  target_link_libraries(${PROJECT_NAME} pico_display_28 pico_multicore)


# create map/bin/hex file etc.
pico_add_extra_outputs(${PROJECT_NAME})