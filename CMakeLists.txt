cmake_minimum_required (VERSION 3.5.0 FATAL_ERROR)
project (beast-jinja2cpp CXX)

include (cmake/conan.cmake)
if (NOT MSVC)
    set (CONAN_SETTINGS SETTINGS compiler.libcxx=libstdc++11)
endif ()

conan_cmake_run(REQUIRES 
                    jinja2cpp/1.0.0@flexferrum/testing
                    gtest/1.7.0@bincrafters/stable
                    boost_beast/1.69.0@bincrafters/stable
                BASIC_SETUP
                ${CONAN_SETTINGS}
                OPTIONS 
                    jinja2cpp:shared=False
                    gtest:shared=False
                BUILD missing)
                
                
set (TARGET_NAME ${PROJECT_NAME})

add_executable (${TARGET_NAME} src/main.cpp)

target_link_libraries (${TARGET_NAME} ${CONAN_LIBS})
set_target_properties (${TARGET_NAME} PROPERTIES
            CXX_STANDARD 14
            CXX_STANDARD_REQUIRED ON)

enable_testing()

add_test(NAME ${TARGET_NAME} COMMAND ${TARGET_NAME})