cmake_minimum_required (VERSION 3.16)
project(ntldd)

#---------------------------------------------------------------------

option (DEBUG "Compile with debug symbols." OFF)
if (CMAKE_BUILD_TYPE STREQUAL "Debug")
    set (DEBUG ON)
endif ()
if (NOT DEBUG)
    set (RELEASE ON)
    set (NDEBUG ON)
else ()
    set (DEBUG ON)
    set (_DEBUG ON)
    set (RELEASE OFF)
endif ()

option (VERBOSE "Verbose console output." OFF)
if (VERBOSE)
    set (CMAKE_VERBOSE_MAKEFILE ON)
endif ()

#---------------------------------------------------------------------

add_executable(ntldd libntldd.c
                     ntldd.c)

add_definitions("-D__USE_MINGW_ANSI_STDIO=1"
                "-D_WIN32_WINNT=0x501")

if (NOT MSVC)
    add_definitions("--pedantic")

    target_compile_options (ntldd PRIVATE "-Wno-missing-braces"
                                          "-fno-common"
                                          "-Wall")

    if (DEBUG)
        target_compile_options (ntldd PRIVATE #"-Wno-bool-compare"
                                              #"-Wno-incompatible-pointer-types"
                                              "-Wno-unused-parameter"
                                              #"-Wno-unused-variable"
                                              "-Wno-int-to-pointer-cast"
                                              "-Wno-unused-function"
                                              "-Wno-format")
    endif ()

endif ()

if (DEBUG)
    message (STATUS "Compiling with DEBUG symbols")

    if (NOT MSVC)
        add_definitions (-g
                         #-Wall
                         #-Werror
                         #-Wfatal-errors
                         -fdiagnostics-show-option
                         -fmax-errors=3)
    else ()
        add_definitions ("-Zi")
    endif ()
else ()
    if (NOT MSVC)
        target_compile_options (ntldd PRIVATE "-O3"
                                              "-c"
                                              "-s")
    else ()
        target_compile_options (ntldd PRIVATE "-Ox"
                                              "-GF"
                                              "-c"
                                              "-W2"
                                              "-GL")
    endif ()
endif ()

target_link_libraries(ntldd "imagehlp")

