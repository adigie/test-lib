if(CONFIG_TEST_LIB)
    zephyr_include_directories(include)
    zephyr_library_named(test_lib)
    zephyr_library_sources(src/test_lib.c)

    set(STRIPPED_DIR ${CMAKE_CURRENT_BINARY_DIR}/stripped)
    set(STRIPPED_LIB ${STRIPPED_DIR}/test_lib.a)

    add_custom_command(
        OUTPUT ${STRIPPED_LIB}
        COMMAND ${CMAKE_STRIP} --strip-debug $<TARGET_FILE:test_lib> -o ${STRIPPED_LIB}
        DEPENDS test_lib
        COMMENT "Creating stripped version of test_lib"
    )

    add_custom_target(test_lib_stripped ALL DEPENDS ${STRIPPED_LIB})

    set(INSTALL_DIR bin/)

    install(
        FILES ${STRIPPED_LIB}
        DESTINATION ${INSTALL_DIR}
        COMPONENT test_lib
    )

endif()