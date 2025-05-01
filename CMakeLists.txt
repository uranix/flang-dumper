cmake_minimum_required(VERSION 3.14)
project(FlangAstDumper)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

set(FLANG_VERSION 19)

add_compile_options(-Wsuggest-override)
set(CC clang-${FLANG_VERSION})
set(CXX clang++-${FLANG_VERSION})
set(CMAKE_C_COMPILER ${CC})
set(CMAKE_CXX_COMPILER ${CXX})
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

SET(LLVM_DIR "/usr/lib/llvm-${FLANG_VERSION}/cmake")
SET(MLIR_DIR "/usr/lib/llvm-${FLANG_VERSION}/lib/cmake/mlir")
SET(Clang_DIR "/usr/lib/llvm-${FLANG_VERSION}/lib/cmake/clang")
SET(Flang_DIR "/usr/lib/llvm-${FLANG_VERSION}/lib/cmake/flang")
find_package(LLVM REQUIRED CONFIG)
find_package(MLIR REQUIRED CONFIG)
find_package(Clang REQUIRED CONFIG)
find_package(Flang REQUIRED CONFIG)

message(STATUS "Found LLVM ${LLVM_PACKAGE_VERSION}")
message(STATUS "Using LLVMConfig.cmake in: ${LLVM_DIR}")

# Set the LLVM and Flang include directories
include_directories(${LLVM_INCLUDE_DIRS})
include_directories(${MLIR_INCLUDE_DIRS})
include_directories(${CLANG_INCLUDE_DIRS})
include_directories(${FLANG_INCLUDE_DIRS})

# Add definitions needed for LLVM and Flang
add_definitions(${LLVM_DEFINITIONS})
add_definitions(${MLIR_DEFINITIONS})
add_definitions(${Clang_DEFINITIONS})
add_definitions(${Flang_DEFINITIONS})


# Set Endianess flag for Flang
include(TestBigEndian)

test_big_endian(IS_BIG_ENDIAN)

if(IS_BIG_ENDIAN)
    add_definitions(-DFLANG_BIG_ENDIAN)
    message(STATUS "System is big endian")
else()
    add_definitions(-DFLANG_LITTLE_ENDIAN)
    message(STATUS "System is little endian")
endif()

# Build plugin
add_library(plugin MODULE src/plugin.cpp)
target_compile_features(plugin PRIVATE)
