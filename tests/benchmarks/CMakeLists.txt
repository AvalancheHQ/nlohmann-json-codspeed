cmake_minimum_required(VERSION 3.11...3.14)
project(JSON_Benchmarks LANGUAGES CXX)

# set compiler flags
if((CMAKE_CXX_COMPILER_ID MATCHES GNU) OR (CMAKE_CXX_COMPILER_ID MATCHES Clang))
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DNDEBUG -O3")
endif()

# configure Google Benchmarks
# include(FetchContent)
# FetchContent_Declare(
#     benchmark
#     GIT_REPOSITORY https://github.com/google/benchmark.git
#     GIT_TAG origin/main
#     GIT_SHALLOW TRUE
# )
#
# FetchContent_GetProperties(benchmark)
# if(NOT benchmark_POPULATED)
#     FetchContent_Populate(benchmark)
#     set(BENCHMARK_ENABLE_TESTING OFF CACHE INTERNAL "" FORCE)
#     add_subdirectory(${benchmark_SOURCE_DIR} ${benchmark_BINARY_DIR})
# endif()

# download test data
set(CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/../../cmake ${CMAKE_MODULE_PATH})
include(download_test_data)

# benchmark binary
add_executable(json_benchmarks src/benchmarks.cpp)
target_compile_features(json_benchmarks PRIVATE cxx_std_11)
target_link_libraries(json_benchmarks benchmark::benchmark ${CMAKE_THREAD_LIBS_INIT})
add_dependencies(json_benchmarks download_test_data)
target_include_directories(json_benchmarks PRIVATE ${CMAKE_SOURCE_DIR}/../../single_include ${CMAKE_BINARY_DIR}/include)


# CODSPEED:
project(my_codspeed_project VERSION 0.0.0 LANGUAGES CXX)
set(BENCHMARK_DOWNLOAD_DEPENDENCIES ON)
include(FetchContent)
FetchContent_Declare(
    google_benchmark
    GIT_REPOSITORY https://github.com/CodSpeedHQ/codspeed-cpp
    GIT_TAG origin/main
    SOURCE_SUBDIR
    google_benchmark
)
FetchContent_MakeAvailable(google_benchmark)

# Declare your benchmark executable and its sources here
#add_executable(my_benchmark_executable main.cpp)

# Link your executable against the `benchmark::benchmark` library
# target_link_libraries(json_benchmarks benchmark::benchmark)
