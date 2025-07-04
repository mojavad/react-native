# Copyright (c) Meta Platforms, Inc. and affiliates.
#
# This source code is licensed under the MIT license found in the
# LICENSE file in the root directory of this source tree.

cmake_minimum_required(VERSION 3.13)
set(CMAKE_VERBOSE_MAKEFILE on)

include(${REACT_COMMON_DIR}/cmake-utils/react-native-flags.cmake)

file(GLOB_RECURSE hermes_executor_SRC CONFIGURE_DEPENDS ${hermes_executor_DIR}/*.cpp)
add_library(
        hermes_executor_common
        OBJECT
        ${hermes_executor_SRC}
)
target_include_directories(hermes_executor_common PUBLIC ${hermes_executor_DIR}/.)
target_link_libraries(hermes_executor_common
        hermes-engine::libhermes
        hermes_inspector_modern
        jsi
        jsireact
)

target_compile_reactnative_options(hermes_executor_common PRIVATE)
if(${CMAKE_BUILD_TYPE} MATCHES Debug)
        target_compile_options(
                hermes_executor_common
                PRIVATE
                -DHERMES_ENABLE_DEBUGGER=1
        )
else()
        target_compile_options(
                hermes_executor_common
                PRIVATE
                -DNDEBUG
        )
endif()
