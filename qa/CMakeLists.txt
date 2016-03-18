set(PA_QA_TESTS
    paqa_devs.c
    paqa_errs.c
    paqa_latency.c
)

foreach(PA_QA_TEST ${PA_QA_TESTS})
    get_filename_component(PA_QA_TEST_NAME ${PA_QA_TEST} NAME_WE)
    add_executable(${PA_QA_TEST_NAME} ${PA_QA_TEST})
    target_link_libraries(${PA_QA_TEST_NAME} portaudio ${PA_LIBRARIES})
endforeach()

add_subdirectory(loopback)
