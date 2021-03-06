# Copyright (c) 2017-2019 The Khronos Group Inc.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
# Author:
#

# Note: This is the top-level CMake file for the OpenXR project.
# It should contain only definitions that are applicable to the
# entire project and includes for the sub-directories.

cmake_minimum_required(VERSION 3.0)
project(OPENXR)

set_property(GLOBAL PROPERTY USE_FOLDERS ON)
set_property(GLOBAL PROPERTY PREDEFINED_TARGETS_FOLDER "")

add_subdirectory(include)
add_subdirectory(src)

find_program(BASH_COMMAND NAMES bash)
if(BASH_COMMAND)
    option(BUILD_SPECIFICATION "Run './makeAllExts all' in the specification directory as part of the build - intended for one-step checking of spec changes" OFF)
    if(BUILD_SPECIFICATION)
        add_custom_target(spec-all ALL
            ${BASH_COMMAND} ./makeAllExts all
            WORKING_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/specification"
            VERBATIM
            COMMENT "Running './makeAllExts all' in the specification directory"
            USES_TERMINAL)
    endif()
endif()
