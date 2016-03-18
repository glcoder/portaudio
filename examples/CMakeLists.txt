set(PA_EXAMPLES
    pa_devs.c
    pa_fuzz.c
    paex_ocean_shore.c
    paex_pink.c
    paex_read_write_wire.c
    paex_record.c
    paex_record_file.c
    paex_saw.c
    paex_sine.c
    paex_write_sine.c
    paex_write_sine_nonint.c
)

if(PA_USE_WMME)
    set(PA_EXAMPLES ${PA_EXAMPLES}
        paex_wmme_ac3.c
        paex_wmme_surround.c
    )
endif(PA_USE_WMME)

foreach(PA_EXAMPLE ${PA_EXAMPLES})
    get_filename_component(PA_EXAMPLE_NAME ${PA_EXAMPLE} NAME_WE)
    add_executable(${PA_EXAMPLE_NAME} ${PA_EXAMPLE})
    target_link_libraries(${PA_EXAMPLE_NAME} portaudio ${PA_LIBRARIES})
endforeach()
