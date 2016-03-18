set(PA_TESTS
    pa_minlat.c
    patest_buffer.c
    patest_callbackstop.c
    patest_clip.c
    patest_converters.c
    patest_dither.c
    patest_hang.c
    patest_in_overflow.c
    patest_latency.c
    patest_leftright.c
    patest_longsine.c
    patest_many.c
    patest_maxsines.c
    patest_mono.c
    patest_multi_sine.c
    patest_out_underflow.c
    patest_prime.c
    patest_read_record.c
    patest_ringmix.c
    patest_sine_channelmaps.c
    patest_sine_formats.c
    patest_sine_srate.c
    patest_sine_time.c
    patest_sine8.c
    patest_start_stop.c
    patest_stop.c
    patest_stop_playout.c
    patest_suggested_vs_streaminfo_latency.c
    patest_sync.c
    patest_timing.c
    patest_toomanysines.c
    patest_two_rates.c
    patest_underflow.c
    patest_wire.c
    patest_write_stop.c
    patest1.c
)

if(PA_USE_WASAPI)
    set(PA_TESTS ${PA_TESTS}
        patest_jack_wasapi.c
    )
endif(PA_USE_WASAPI)

if(PA_USE_WMME)
    set(PA_TESTS ${PA_TESTS}
        patest_wmme_find_best_latency_params.c
        patest_wmme_low_level_latency_params.c
    )
endif(PA_USE_WMME)

foreach(PA_TEST ${PA_TESTS})
    get_filename_component(PA_TEST_NAME ${PA_TEST} NAME_WE)
    add_executable(${PA_TEST_NAME} ${PA_TEST})
    target_link_libraries(${PA_TEST_NAME} portaudio ${PA_LIBRARIES})
endforeach()
