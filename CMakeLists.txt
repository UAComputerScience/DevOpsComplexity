# srcComplexity build
cmake_minimum_required(VERSION 3.0.0)

project(srcComplexity)

enable_testing()

option(BUILD_TESTS "Build srcComplexity tests" ON)

find_package(LibXml2 REQUIRED)
message(${LIBXML2_LIBRARIES})

add_definitions(-std=c++11)

include_directories(include)

file(GLOB SRCCOMPLEXITY_SOURCE "src/*.cpp")

add_executable(srcComplexity ${SRCCOMPLEXITY_SOURCE})
target_include_directories(srcComplexity PUBLIC ${LIBXML2_INCLUDE_DIR})
target_link_libraries(srcComplexity PUBLIC ${LIBXML2_LIBRARIES})

if(BUILD_TESTS)
    add_subdirectory(test)
endif()

set(CPACK_PACKAGE_NAME "srcComplexity")
set(CPACK_PACKAGE_VERSION_MAJOR "0")
set(CPACK_PACKAGE_VERSION_MINOR "9")
set(CPACK_PACKAGE_VERSION_PATCH "5")

include(CPack)
