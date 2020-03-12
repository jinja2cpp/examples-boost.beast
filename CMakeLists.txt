cmake_minimum_required (VERSION 3.5.0 FATAL_ERROR)
project (beast-jinja2cpp CXX)

include (cmake/conan.cmake)
if (NOT MSVC)
    set (CONAN_SETTINGS SETTINGS compiler.libcxx=libstdc++11)
endif ()

conan_cmake_run(REQUIRES
        jinja2cpp/1.1.0
        gtest/1.10.0
        boost/1.72.0
        nlohmann_json/3.7.0
        BASIC_SETUP
        ${CONAN_SETTINGS}
        OPTIONS
        jinja2cpp:shared=False
        gtest:shared=False
        BUILD missing)
                
                
set (TARGET_NAME ${PROJECT_NAME})

if (${CMAKE_CXX_COMPILER_ID} MATCHES "Clang" OR ${CMAKE_CXX_COMPILER_ID} MATCHES "GNU")
    if (NOT UNIX AND ${CMAKE_CXX_COMPILER_ID} MATCHES "GNU")
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wa,-mbig-obj")
    endif ()
endif ()

add_executable (${TARGET_NAME} src/main.cpp)

target_link_libraries(${TARGET_NAME} ${CONAN_LIBS} Ws2_32 Mswsock)
set_target_properties(${TARGET_NAME} PROPERTIES
        CXX_STANDARD 14
        CXX_STANDARD_REQUIRED ON)

enable_testing()

add_test(NAME ${TARGET_NAME} COMMAND ${TARGET_NAME})