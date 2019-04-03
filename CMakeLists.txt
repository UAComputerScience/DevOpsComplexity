# srcComplexity build
cmake_minimum_required(VERSION 3.0.0)

project(srcComplexity)

enable_testing()

option(BUILD_TESTS "Build srcComplexity tests" ON)

find_package(LibXml2 REQUIRED)
message(${LIBXML2_LIBRARIES})

add_definitions(-std=c++11)

include_directories(include)

file(GLOB SRCCOMPLEXITY_SOURCE "src/srcComplexity.cpp")

add_library(libsrcmlxpathcount OBJECT src/srcMLXPathCount.cpp)
target_include_directories(libsrcmlxpathcount PUBLIC ${LIBXML2_INCLUDE_DIR})

add_library(libsrcmlxpathcount_shared SHARED $<TARGET_OBJECTS:libsrcmlxpathcount>)
set_target_properties(libsrcmlxpathcount_shared PROPERTIES OUTPUT_NAME srcmlxpathcount)
target_link_libraries(libsrcmlxpathcount_shared PRIVATE ${LIBXML2_LIBRARIES})

add_library(libsrcmlxpathcount_static STATIC $<TARGET_OBJECTS:libsrcmlxpathcount>)
set_target_properties(libsrcmlxpathcount_static PROPERTIES OUTPUT_NAME srcmlxpathcount)
target_link_libraries(libsrcmlxpathcount_static PRIVATE ${LIBXML2_LIBRARIES})

add_executable(srcComplexity ${SRCCOMPLEXITY_SOURCE})
target_include_directories(srcComplexity PUBLIC ${LIBXML2_INCLUDE_DIR})
target_link_libraries(srcComplexity PRIVATE libsrcmlxpathcount_shared)

if(BUILD_TESTS)
    add_subdirectory(test)
endif()

set(CPACK_PACKAGE_NAME "srcComplexity")
set(CPACK_PACKAGE_VERSION_MAJOR "0")
set(CPACK_PACKAGE_VERSION_MINOR "9")
set(CPACK_PACKAGE_VERSION_PATCH "5")

include(CPack)
