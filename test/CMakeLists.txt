# Testing

enable_testing()

file(GLOB TEST_SOURCES "*.cpp")
file(GLOB IGNORE FOO.cpp)
list(REMOVE_ITEM TEST_SOURCES ${IGNORE})

foreach(TEST ${TEST_SOURCES})
    get_filename_component(TEST_NAME ${TEST} NAME_WE)

    add_executable(${TEST_NAME} ${TEST})
    target_include_directories(${TEST_NAME} PUBLIC ${LIBXML2_INCLUDE_DIR})
    target_link_libraries(${TEST_NAME} PUBLIC ${LIBXML2_LIBRARIES})

    add_test(NAME ${TEST_NAME} COMMAND ${TEST_NAME})
endforeach()

# copy over input test files
file(COPY srcMLXPathCount.cpp.xml DESTINATION ${CMAKE_CURRENT_BINARY_DIR})

include(CTest)
