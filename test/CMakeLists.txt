cmake_minimum_required(VERSION 3.14)

project(ska_sort_tests CXX)

# ---- Declare dependencies ----

include(FetchContent)

set(gtest_force_shared_crt ON CACHE INTERNAL "Always use msvcrt.dll")
FetchContent_Declare(
    googletest
    GIT_REPOSITORY https://github.com/google/googletest.git
    GIT_TAG release-1.10.0
)

FetchContent_Declare(ska_sort SOURCE_DIR "${PROJECT_SOURCE_DIR}/..")

# ---- Import dependencies ----

FetchContent_MakeAvailable(googletest ska_sort)

# ---- Options ----

option(FULL_TESTS_SLOW_COMPILE_TIME "Include slow to compile tests" OFF)

# ---- Test ----

enable_testing()

function(test_case NAME)
  add_executable("${NAME}" "source/${NAME}.cpp")
  target_include_directories("${NAME}" PRIVATE "${PROJECT_SOURCE_DIR}/include")
  target_link_libraries("${NAME}" PRIVATE gtest_main ska::sort)
  target_compile_features("${NAME}" PRIVATE cxx_std_14)
  add_test(NAME "${NAME}" COMMAND "${NAME}")
endfunction()

test_case(ska_sort_tests)
# test_case(ska_sort_benchmarks)