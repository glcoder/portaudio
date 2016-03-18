set(PA_LOOPBACK_INCLUDES
    src/audio_analyzer.h
    src/biquad_filter.h
    src/paqa_tools.h
    src/qa_tools.h
    src/test_audio_analyzer.h
    src/write_wav.h
)

set(PA_LOOPBACK_SOURCES
    src/audio_analyzer.c
    src/biquad_filter.c
    src/paqa.c
    src/paqa_tools.c
    src/test_audio_analyzer.c
    src/write_wav.c
)

add_executable(pa_loopback ${PA_LOOPBACK_INCLUDES} ${PA_LOOPBACK_SOURCES})
target_link_libraries(pa_loopback portaudio ${PA_LIBRARIES})
