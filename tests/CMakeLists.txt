cmake_minimum_required(VERSION 3.0 FATAL_ERROR)

project(cppzmq-test CXX)

# place binaries and libraries according to GNU standards

include(GNUInstallDirs)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/${CMAKE_INSTALL_LIBDIR})
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/${CMAKE_INSTALL_LIBDIR})
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/${CMAKE_INSTALL_BINDIR})

include(cmake/catch.cmake)
find_package(Threads)

add_executable(
    unit_tests
    message.cpp
    context.cpp
    socket.cpp
    poller.cpp
    active_poller.cpp
    multipart.cpp
	monitor.cpp
)

add_dependencies(unit_tests catch)

target_include_directories(unit_tests PUBLIC ${CATCH_INCLUDE_DIR})
target_link_libraries(
    unit_tests
    PRIVATE cppzmq
    PRIVATE ${CMAKE_THREAD_LIBS_INIT}
)

OPTION (COVERAGE "Enable gcda file generation needed by lcov" OFF)

if (COVERAGE)
    target_compile_options(unit_tests PRIVATE --coverage)
    target_link_libraries(unit_tests PRIVATE --coverage)
endif()

add_test(
  NAME
    unit
  COMMAND
    ${CMAKE_BINARY_DIR}/${CMAKE_INSTALL_BINDIR}/unit_tests
)
