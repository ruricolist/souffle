# Souffle - A Datalog Compiler
# Copyright (c) 2021 The Souffle Developers. All rights reserved
# Licensed under the Universal Permissive License v 1.0 as shown at:
# - https://opensource.org/licenses/UPL
# - <souffle root>/licenses/SOUFFLE-UPL.txt

include(SouffleTests)

function(SOUFFLE_LINK_TEST)
    cmake_parse_arguments(
        PARAM
        ""
        "TEST_NAME"
        "DATALOG_FILES"
        ${ARGV}
    )

    set(INPUT_DIR "${CMAKE_CURRENT_SOURCE_DIR}/${PARAM_TEST_NAME}")
    set(FACTS_DIR "${INPUT_DIR}/facts")
    set(OUTPUT_DIR "${CMAKE_CURRENT_BINARY_DIR}/${PARAM_TEST_NAME}")

    ADD_TEST(NAME ${PARAM_TEST_NAME}
        COMMAND
        ${Python3_EXECUTABLE} ${PROJECT_SOURCE_DIR}/cmake/redirect.py
        --out ${PARAM_TEST_NAME}.out
        --err ${PARAM_TEST_NAME}.err
        ${Python3_EXECUTABLE}
        ${INPUT_DIR}/test.py
        ${INPUT_DIR}
        ${OUTPUT_DIR}
        ${CMAKE_CXX_COMPILER}
        $<TARGET_FILE:souffle>
        driver.cpp
        "${CMAKE_SOURCE_DIR}/src/include"
        ${PARAM_DATALOG_FILES}
        COMMAND_EXPAND_LISTS
        WORKING_DIRECTORY ${INPUT_DIR}
    )

endfunction()

if (UNIX)
    souffle_link_test(TEST_NAME link2 DATALOG_FILES insert_for1.dl insert_for2.dl)
endif(UNIX)
