cmake_minimum_required(VERSION 3.13)

include(pico-sdk/pico_sdk_init.cmake)

project(starcon_project C CXX ASM)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)
pico_sdk_init()

add_executable(starcon
    starcon.cpp
    TinyUSB_Mouse_and_Keyboard/TinyUSB_Mouse_and_Keyboard.cpp
    Adafruit_TinyUSB_Arduino/src/arduino/hid/Adafruit_USBD_HID.cpp
    Adafruit_TinyUSB_Arduino/src/arduino/Adafruit_USBD_Device.cpp
    Adafruit_TinyUSB_Arduino/src/arduino/ports/rp2040/Adafruit_TinyUSB_rp2040.cpp
    Print.cpp
)

target_include_directories(starcon PRIVATE ${CMAKE_CURRENT_LIST_DIR})

target_link_libraries(starcon
    pico_stdlib
    pico_unique_id
    tinyusb_device
    tinyusb_board
)
pico_add_extra_outputs(starcon)

add_definitions(-DUSE_TINYUSB)
add_definitions(-DUSB_PRODUCT="StarCon")
add_definitions(-DARDUINO_ARCH_RP2040)
add_definitions(-DCFG_TUSB_CONFIG_FILE="includes/tusb_config.h")
add_definitions(-DUSB_VID=0x05ac)
add_definitions(-DUSB_PID=0x024f)
add_definitions(-DUSB_MANUFACTURER="Catry")

include_directories(includes
    TinyUSB_Mouse_and_Keyboard
    Adafruit_TinyUSB_Arduino/src
)

# remove Adafruit tinyusb library code so that only the pico-sdk tinyusb code is used
# to restore, cd Adafruit_TinyUSB_Arduino/ && git checkout HEAD .
file(REMOVE Adafruit_TinyUSB_Arduino/src/tusb.c)
file(REMOVE Adafruit_TinyUSB_Arduino/src/tusb.h)
file(REMOVE Adafruit_TinyUSB_Arduino/src/tusb_option.h)
file(REMOVE_RECURSE Adafruit_TinyUSB_Arduino/src/osal/)
file(REMOVE_RECURSE Adafruit_TinyUSB_Arduino/src/portable/)
file(REMOVE_RECURSE Adafruit_TinyUSB_Arduino/src/device/)
file(REMOVE_RECURSE Adafruit_TinyUSB_Arduino/src/common/)
file(REMOVE_RECURSE Adafruit_TinyUSB_Arduino/src/class/)
