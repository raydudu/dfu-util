cmake_minimum_required(VERSION 3.13)
project(dfu-util VERSION 0.11 LANGUAGES C)

set(CMAKE_C_STANDARD 99)

find_library(LIBUSB_LIBRARIES NAMES usb-1.0 REQUIRED
        PATH_SUFFIXES "lib" "lib32" "lib64")
find_path(LIBUSB_INCLUDE_DIR NAMES libusb.h REQUIRED
        PATH_SUFFIXES "include" "libusb" "libusb-1.0")

set(CMAKE_INSTALL_RPATH "${CMAKE_INSTALL_PREFIX}/lib")

if (NOT WIN32)
    add_definitions(-DHAVE_CONFIG_H)
    configure_file(config.h.in include/config.h)
    include_directories(${CMAKE_CURRENT_BINARY_DIR}/include)
endif()


add_executable(${PROJECT_NAME} main.c
        portable.h
        dfu.c dfu.h
        dfu_file.c dfu_file.h
        dfu_load.c dfu_load.h
        dfu_util.c dfu_util.h
        dfuse.c dfuse.h
        dfuse_mem.c dfuse_mem.h
        quirks.c quirks.h
        usb_dfu.h)
target_include_directories(${PROJECT_NAME} PRIVATE ${LIBUSB_INCLUDE_DIR})
target_link_libraries(${PROJECT_NAME} PRIVATE ${LIBUSB_LIBRARIES})

add_executable(dfu-suffix suffix.c
        portable.h
        dfu_file.c dfu_file.h)

add_executable(dfu-prefix prefix.c
        portable.h
        dfu_file.c dfu_file.h)

# MAN page generation using docker
if (NOT WIN32) # No idea how to make it work under windows
    find_program(DOCKER docker NO_CMAKE_SYSTEM_PATH)
    if (DOCKER)
        execute_process(COMMAND id -u OUTPUT_VARIABLE UID OUTPUT_STRIP_TRAILING_WHITESPACE)
        set(DOCKER ${DOCKER} run --rm --volume ${CMAKE_CURRENT_SOURCE_DIR}/doc:/data --user=${UID} pandoc/minimal)

        add_custom_command(
                OUTPUT ${CMAKE_CURRENT_SOURCE_DIR}/doc/dfu-suffix.1
                MAIN_DEPENDENCY ${CMAKE_CURRENT_SOURCE_DIR}/doc/dfu-suffix.1.md
                WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/doc
                COMMAND ${DOCKER} dfu-suffix.1.md -s -t man -o dfu-suffix.1
        )
        add_custom_command(
                OUTPUT ${CMAKE_CURRENT_SOURCE_DIR}/doc/dfu-prefix.1
                MAIN_DEPENDENCY ${CMAKE_CURRENT_SOURCE_DIR}/doc/dfu-prefix.1.md
                WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/doc
                COMMAND ${DOCKER} dfu-prefix.1.md -s -t man -o dfu-prefix.1
        )
        add_custom_target(man ALL
                DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/doc/dfu-prefix.1
                        ${CMAKE_CURRENT_SOURCE_DIR}/doc/dfu-suffix.1
        )
        install(FILES doc/dfu-suffix.1 doc/dfu-prefix.1 DESTINATION ${CMAKE_INSTALL_PREFIX}/man/man1)
    endif ()
endif ()

# Installation
install(TARGETS ${PROJECT_NAME} dfu-suffix dfu-prefix)
install(FILES doc/dfu-util.1 DESTINATION ${CMAKE_INSTALL_PREFIX}/man/man1)