# Copyright (c) 2019, NVIDIA CORPORATION. All rights reserved.
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions
# are met:
#  * Redistributions of source code must retain the above copyright
#    notice, this list of conditions and the following disclaimer.
#  * Redistributions in binary form must reproduce the above copyright
#    notice, this list of conditions and the following disclaimer in the
#    documentation and/or other materials provided with the distribution.
#  * Neither the name of NVIDIA CORPORATION nor the names of its
#    contributors may be used to endorse or promote products derived
#    from this software without specific prior written permission.
#
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS ``AS IS'' AND ANY
# EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
# IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR
# PURPOSE ARE DISCLAIMED.  IN NO EVENT SHALL THE COPYRIGHT OWNER OR
# CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL,
# EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO,
# PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR
# PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY
# OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
# (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
# OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

cmake_minimum_required (VERSION 3.5)

#
# Custom backend
#
set(
  CUSTOM_SRCS
  custom_backend_factory.cc
  custom_backend.cc
  loader.cc
)

set(
  CUSTOM_HDRS
  custom_backend_factory.h
  custom_backend.h
  loader.h
)

add_library(
  custom-backend-library EXCLUDE_FROM_ALL OBJECT
  ${CUSTOM_SRCS} ${CUSTOM_HDRS}
)
add_dependencies(custom-backend-library proto-library)

if(${TRITON_ENABLE_GPU})
  target_include_directories(custom-backend-library PRIVATE ${CUDA_INCLUDE_DIRS})
endif() # TRITON_ENABLE_GPU
if(${TRITON_ENABLE_METRICS})
  target_include_directories(
    custom-backend-library
    PRIVATE $<TARGET_PROPERTY:prometheus-cpp::core,INTERFACE_INCLUDE_DIRECTORIES>
  )
endif() # TRITON_ENABLE_METRICS
