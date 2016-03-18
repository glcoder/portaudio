cmake_minimum_required(VERSION 3.0.2)
project(portaudio VERSION 0.19 LANGUAGES C)

option(BUILD_SHARED_LIBS "Build shared libraries" OFF)
option(PA_BUILD_EXAMPLES "Build Portaudio examples" OFF)
option(PA_BUILD_TESTS "Build Portaudio tests" OFF)
option(PA_BUILD_QA_TESTS "Build Portaudio QA tests" OFF)

option(PA_ENABLE_DEBUG_OUTPUT "Enable debug output for Portaudio" OFF)
option(PA_USE_SKELETON "Enable support for Skeleton" ON)

if(APPLE)
    option(PA_USE_COREAUDIO "Enable support for Core Audio" ON)
    option(PA_USE_JACK "Enable support for JACK" ON)
endif(APPLE)

if(UNIX AND NOT APPLE)
    option(PA_USE_ALSA "Enable support for ALSA" ON)
    option(PA_USE_ASIHPI "Enable support for ASIHPI" ON)
    option(PA_USE_JACK "Enable support for JACK" ON)
    option(PA_USE_OSS "Enable support for OSS" ON)
endif(UNIX AND NOT APPLE)

if(WIN32)
    option(PA_UNICODE_BUILD "Enable Portaudio Unicode build" ON)
    option(PA_USE_WMME "Enable support for MME" ON)
    option(PA_USE_WASAPI "Enable support for WASAPI" ON)
    option(PA_USE_WDMKS "Enable support for WDMKS" ON)
    option(PA_USE_WDMKS_DEVICE_INFO "Use WDM/KS API for device info" ON)
    option(PA_WDMKS_NO_KSGUID_LIB "Disable linkage with ksguid" ON)
    mark_as_advanced(PA_USE_WDMKS_DEVICE_INFO PA_WDMKS_NO_KSGUID_LIB)
endif(WIN32)

include_directories(include src/common)
set(PA_INCLUDES
    include/portaudio.h
    src/common/pa_allocation.h
    src/common/pa_converters.h
    src/common/pa_cpuload.h
    src/common/pa_debugprint.h
    src/common/pa_dither.h
    src/common/pa_endianness.h
    src/common/pa_hostapi.h
    src/common/pa_memorybarrier.h
    src/common/pa_process.h
    src/common/pa_ringbuffer.h
    src/common/pa_stream.h
    src/common/pa_trace.h
    src/common/pa_types.h
    src/common/pa_util.h
)

set(PA_SOURCES
    src/common/pa_allocation.c
    src/common/pa_converters.c
    src/common/pa_cpuload.c
    src/common/pa_debugprint.c
    src/common/pa_dither.c
    src/common/pa_front.c
    src/common/pa_process.c
    src/common/pa_ringbuffer.c
    src/common/pa_stream.c
    src/common/pa_trace.c
)

#src/os/unix/pa_unix_hostapis.o
#src/os/unix/pa_unix_util.o
#src/hostapi/coreaudio/pa_mac_core.o
#src/hostapi/coreaudio/pa_mac_core_utilities.o
#src/hostapi/coreaudio/pa_mac_core_blocking.o
#src/common/pa_ringbuffer.o

if(UNIX)
    include_directories(src/os/unix)
    set(PA_INCLUDES ${PA_INCLUDES}
        src/os/unix/pa_unix_util.h
    )
    set(PA_SOURCES ${PA_SOURCES}
        src/os/unix/pa_unix_hostapis.c
        src/os/unix/pa_unix_util.c
    )
endif(UNIX)

if(WIN32)
    include_directories(src/os/win)
    set(PA_INCLUDES ${PA_INCLUDES}
        src/os/win/pa_win_coinitialize.h
        src/os/win/pa_win_wdmks_utils.h
    )
    set(PA_SOURCES ${PA_SOURCES}
        src/os/win/pa_win_coinitialize.c
        src/os/win/pa_win_hostapis.c
        src/os/win/pa_win_util.c
        src/os/win/pa_win_waveformat.c
        src/os/win/pa_win_wdmks_utils.c
    )
endif(WIN32)

if(PA_USE_SKELETON)
    set(PA_SOURCES ${PA_SOURCES}
        src/hostapi/skeleton/pa_hostapi_skeleton.c
    )
endif(PA_USE_SKELETON)

if(PA_USE_ALSA)
    set(PA_SOURCES ${PA_SOURCES}
        src/hostapi/alsa/pa_linux_alsa.c
    )
endif(PA_USE_ALSA)

if(PA_USE_ASIHPI)
    set(PA_SOURCES ${PA_SOURCES}
        src/hostapi/asihpi/pa_linux_asihpi.c
    )
endif(PA_USE_ASIHPI)

if(PA_USE_COREAUDIO)
    include_directories(src/hostapi/coreaudio)
    set(PA_INCLUDES ${PA_INCLUDES}
        src/hostapi/coreaudio/pa_mac_core_blocking.h
        src/hostapi/coreaudio/pa_mac_core_internal.h
        src/hostapi/coreaudio/pa_mac_core_utilities.h
    )
    set(PA_SOURCES ${PA_SOURCES}
        src/hostapi/coreaudio/pa_mac_core.c
        src/hostapi/coreaudio/pa_mac_core_blocking.c
        src/hostapi/coreaudio/pa_mac_core_utilities.c
    )
endif(PA_USE_COREAUDIO)

if(PA_USE_JACK)
    set(PA_SOURCES ${PA_SOURCES}
        src/hostapi/jack/pa_jack.c
    )
endif(PA_USE_JACK)

if(PA_USE_OSS)
    set(PA_SOURCES ${PA_SOURCES}
        src/hostapi/oss/pa_unix_oss.c
    )
endif(PA_USE_OSS)

if(PA_USE_WASAPI)
    set(PA_SOURCES ${PA_SOURCES}
        src/hostapi/wasapi/pa_win_wasapi.c
    )
endif(PA_USE_WASAPI)

if(PA_USE_WDMKS)
    set(PA_SOURCES ${PA_SOURCES}
        src/hostapi/wdmks/pa_win_wdmks.c
    )
endif(PA_USE_WDMKS)

if(PA_USE_WMME)
    set(PA_SOURCES ${PA_SOURCES}
        src/hostapi/wmme/pa_win_wmme.c
    )
endif(PA_USE_WMME)

if(PA_UNICODE_BUILD)
    set_source_files_properties(
      ${PA_SOURCES}
      PROPERTIES
      COMPILE_DEFINITIONS "UNICODE;_UNICODE"
    )
endif(PA_UNICODE_BUILD)

set(PA_LIBRARIES)

if(APPLE)
    list(APPEND PA_LIBRARIES AudioToolbox AudioUnit Carbon)
endif(APPLE)

if(UNIX AND NOT APPLE)
    list(APPEND PA_LIBRARIES m pthread)
endif(UNIX AND NOT APPLE)

if(PA_USE_ALSA)
    list(APPEND PA_LIBRARIES asound)
endif(PA_USE_ALSA)

if(PA_USE_ASIHPI)
    list(APPEND PA_LIBRARIES hpi)
endif(PA_USE_ASIHPI)

if(PA_USE_COREAUDIO)
    list(APPEND PA_LIBRARIES CoreAudio)
endif(PA_USE_COREAUDIO)

if(PA_USE_OSS)
    list(APPEND PA_LIBRARIES ossaudio)
endif(PA_USE_OSS)

if(PA_USE_WDMKS)
    list(APPEND PA_LIBRARIES setupapi)
endif(PA_USE_WDMKS)

if(PA_USE_WMME)
    list(APPEND PA_LIBRARIES winmm)
endif(PA_USE_WMME)

set(PA_LIBRARIES ${PA_LIBRARIES} CACHE STRING "Dependencies of Portaudio")

configure_file(pa_config.h.in ${PROJECT_BINARY_DIR}/pa_config.h)

include_directories(${PROJECT_BINARY_DIR})
set(PA_INCLUDES ${PA_INCLUDES}
    ${PROJECT_BINARY_DIR}/pa_config.h
)

if(MSVC)
add_definitions(-D_CRT_SECURE_NO_WARNINGS)
endif(MSVC)

add_library(portaudio ${PA_INCLUDES} ${PA_SOURCES})

if(BUILD_SHARED_LIBS)
    target_link_libraries(portaudio ${PA_LIBRARIES})
endif(BUILD_SHARED_LIBS)

if(PA_BUILD_EXAMPLES)
    add_subdirectory(examples)
endif(PA_BUILD_EXAMPLES)

if(PA_BUILD_TESTS)
    add_subdirectory(test)
endif(PA_BUILD_TESTS)

if(PA_BUILD_QA_TESTS)
    add_subdirectory(qa)
endif(PA_BUILD_QA_TESTS)
