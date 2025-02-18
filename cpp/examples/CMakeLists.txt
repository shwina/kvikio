# =============================================================================
# Copyright (c) 2021-2023, NVIDIA CORPORATION.
#
# Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except
# in compliance with the License. You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software distributed under the License
# is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express
# or implied. See the License for the specific language governing permissions and limitations under
# the License.
# =============================================================================

set(TEST_INSTALL_PATH bin/tests/libkvikio)
set(TEST_NAME BASIC_IO_TEST)

add_executable(BASIC_IO_TEST basic_io.cpp)
set_target_properties(BASIC_IO_TEST PROPERTIES INSTALL_RPATH "\$ORIGIN/../../lib")
target_include_directories(BASIC_IO_TEST PRIVATE ../include ${cuFile_INCLUDE_DIRS})
target_link_libraries(BASIC_IO_TEST PRIVATE kvikio CUDA::cudart)

if(CMAKE_COMPILER_IS_GNUCXX)
  set(KVIKIO_CXX_FLAGS "-Wall;-Werror;-Wno-unknown-pragmas")
  if(NOT cuFile_FOUND)
    set(KVIKIO_CXX_FLAGS "${KVIKIO_CXX_FLAGS};-DKVIKIO_DISABLE_CUFILE")
  endif()
  target_compile_options(BASIC_IO_TEST PRIVATE "$<$<COMPILE_LANGUAGE:CXX>:${KVIKIO_CXX_FLAGS}>")
endif()

install(
  TARGETS ${TEST_NAME}
  COMPONENT testing
  DESTINATION ${TEST_INSTALL_PATH}
  EXCLUDE_FROM_ALL
)
