# Copyright (c) 2020, pasta Currency Project
# Portions copyright (c) 2014-2018, The Monero Project
#
# Portions of this file are available under BSD-3 license. Please see ORIGINAL-LICENSE for details
# All rights reserved.
# 
# Authors and copyright holders give permission for following:
#
# 1. Redistribution and use in source and binary forms WITHOUT modification.
#
# 2. Modification of the source form for your own personal use. 
# 
# As long as the following conditions are met:
# 
# 3. You must not distribute modified copies of the work to third parties. This includes 
#    posting the work online, or hosting copies of the modified work for download.
#
# 4. Any derivative version of this work is also covered by this license, including point 8.
#
# 5. Neither the name of the copyright holders nor the names of the authors may be
#    used to endorse or promote products derived from this software without specific
#    prior written permission.
#
# 6. You agree that this licence is governed by and shall be construed in accordance 
#    with the laws of England and Wales.
#
# 7. You agree to submit all disputes arising out of or in connection with this licence 
#    to the exclusive jurisdiction of the Courts of England and Wales.
#
# Authors and copyright holders agree that:
#
# 8. This licence expires and the work covered by it is released into the 
#    public domain on 1st of February 2021
# 
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND ANY
# EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF
# MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL
# THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
# SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO,
# PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS
# INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT,
# STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF
# THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

set(rpc_base_sources
  rpc_args.cpp)

set(rpc_sources
  core_rpc_server.cpp
  instanciations)

set(daemon_messages_sources
  message.cpp
  daemon_messages.cpp)

set(daemon_rpc_server_sources
  daemon_handler.cpp
  zmq_server.cpp)


set(rpc_base_headers
  rpc_args.h)

set(rpc_headers)

set(daemon_rpc_server_headers)


set(rpc_daemon_private_headers
  core_rpc_server.h
  core_rpc_server_commands_defs.h
  core_rpc_server_error_codes.h)

set(daemon_messages_private_headers
  message.h
  daemon_messages.h)

set(daemon_rpc_server_private_headers
  message.h
  daemon_messages.h
  daemon_handler.h
  rpc_handler.h
  zmq_server.h)


pasta_private_headers(rpc
  ${rpc_private_headers})

pasta_private_headers(daemon_rpc_server
  ${daemon_rpc_server_private_headers})


pasta_add_library(rpc_base
  ${rpc_base_sources}
  ${rpc_base_headers}
  ${rpc_base_private_headers})

pasta_add_library(rpc
  ${rpc_sources}
  ${rpc_headers}
  ${rpc_private_headers})

pasta_add_library(daemon_messages
  ${daemon_messages_sources}
  ${daemon_messages_headers}
  ${daemon_messages_private_headers})

pasta_add_library(daemon_rpc_server
  ${daemon_rpc_server_sources}
  ${daemon_rpc_server_headers}
  ${daemon_rpc_server_private_headers})


target_link_libraries(rpc_base
  PUBLIC
    common
    epee
    ${Boost_REGEX_LIBRARY}
    ${Boost_THREAD_LIBRARY}
    ${Boost_PROGRAM_OPTIONS_LIBRARY}
    fmt::fmt-header-only
  PRIVATE
    ${EXTRA_LIBRARIES})

target_link_libraries(rpc
  PUBLIC
    rpc_base
    common
    cryptonote_core
    cryptonote_protocol
    epee
    ${Boost_REGEX_LIBRARY}
    ${Boost_THREAD_LIBRARY}
    fmt::fmt-header-only
  PRIVATE
    ${EXTRA_LIBRARIES})

target_link_libraries(daemon_messages
  LINK_PUBLIC
    fmt::fmt-header-only
  LINK_PRIVATE
    cryptonote_core
    cryptonote_protocol
    serialization
    ${EXTRA_LIBRARIES})

target_link_libraries(daemon_rpc_server
  LINK_PUBLIC
    fmt::fmt-header-only
  LINK_PRIVATE
    rpc
    cryptonote_core
    cryptonote_protocol
    daemon_messages
    serialization
    ${Boost_CHRONO_LIBRARY}
    ${Boost_REGEX_LIBRARY}
    ${Boost_SYSTEM_LIBRARY}
    ${Boost_THREAD_LIBRARY}
    ${ZMQ_LIB}
    ${EXTRA_LIBRARIES})
target_include_directories(daemon_rpc_server PUBLIC ${ZMQ_INCLUDE_PATH})
target_include_directories(obj_daemon_rpc_server PUBLIC ${ZMQ_INCLUDE_PATH})
