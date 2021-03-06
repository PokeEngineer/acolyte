
project(GTEST_BUILDER C CXX)
cmake_minimum_required(VERSION 2.8.10)

include(ExternalProject)

ExternalProject_Add(googletest
    GIT_REPOSITORY https://github.com/google/googletest.git
    CMAKE_ARGS -DCMAKE_ARCHIVE_OUTPUT_DIRECTORY:PATH=
               -Dgtest_disable_pthreads=ON
               -Dgtest_force_shared_crt=ON
    PREFIX "${CMAKE_CURRENT_BINARY_DIR}"
    INSTALL_COMMAND ""
    TIMEOUT 10
)

set_target_properties(googletest PROPERTIES EXCLUDE_FROM_ALL TRUE)

ExternalProject_Get_Property(googletest source_dir)
set(GTEST_INCLUDE_DIR "${source_dir}/googletest/include" PARENT_SCOPE)

ExternalProject_Get_Property(googletest binary_dir)
set(GTEST_LIBS_DIR "${binary_dir}")

add_library(gtest STATIC IMPORTED GLOBAL)
set_property(TARGET gtest PROPERTY IMPORTED_LOCATION
    ${GTEST_LIBS_DIR}/${CMAKE_CFG_INTDIR}/${CMAKE_STATIC_LIBRARY_PREFIX}gtest${CMAKE_STATIC_LIBRARY_SUFFIX}
)
add_dependencies(gtest googletest)

add_library(gtest_main STATIC IMPORTED GLOBAL)
set_property(TARGET gtest_main PROPERTY IMPORTED_LOCATION
    ${GTEST_LIBS_DIR}/${CMAKE_CFG_INTDIR}/${CMAKE_STATIC_LIBRARY_PREFIX}gtest_main${CMAKE_STATIC_LIBRARY_SUFFIX}
)
add_dependencies(gtest_main googletest)
