cmake_minimum_required(VERSION 3.18)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

option(BUILD_TESTS "Build tests" ON)

project(banking)

add_subdirectory(third-party/gtest ${PROJECT_BINARY_DIR}/gtest)

add_library(${PROJECT_NAME} STATIC
        ${CMAKE_CURRENT_SOURCE_DIR}/banking/Account.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/banking/Transaction.cpp
        )

target_include_directories(${PROJECT_NAME} PUBLIC
        "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>"
        "$<INSTALL_INTERFACE:include>"
        )

if (BUILD_TESTS)
    enable_testing()
    add_executable(tests
            ${CMAKE_CURRENT_SOURCE_DIR}/tests/test.cpp)
    target_link_libraries(tests ${PROJECT_NAME} gtest_main)
    add_test(NAME unit_tests COMMAND tests)
endif()