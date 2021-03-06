# Copyright (c) 2014-2015, The Monero Project
#
# All rights reserved.
#
# Redistribution and use in source and binary forms, with or without modification, are
# permitted provided that the following conditions are met:
#
# 1. Redistributions of source code must retain the above copyright notice, this list of
#    conditions and the following disclaimer.
#
# 2. Redistributions in binary form must reproduce the above copyright notice, this list
#    of conditions and the following disclaimer in the documentation and/or other
#    materials provided with the distribution.
#
# 3. Neither the name of the copyright holder nor the names of its contributors may be
#    used to endorse or promote products derived from this software without specific
#    prior written permission.
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

set(blockchain_converter_sources
  blockchain_converter.cpp
  )

set(blockchain_converter_private_headers)

bitmonero_private_headers(blockchain_converter
	  ${blockchain_converter_private_headers})

set(blockchain_import_sources
  blockchain_import.cpp
  )

set(blockchain_import_private_headers
  import.h
  fake_core.h
  )

bitmonero_private_headers(blockchain_import
	  ${blockchain_import_private_headers})

set(blockchain_export_sources
  blockchain_export.cpp
  )

set(blockchain_export_private_headers
  import.h
  blockchain_export.h
  )

bitmonero_private_headers(blockchain_export
	  ${blockchain_export_private_headers})



if (BLOCKCHAIN_DB STREQUAL DB_LMDB)
bitmonero_add_executable(blockchain_converter
  ${blockchain_converter_sources}
  ${blockchain_converter_private_headers})

target_link_libraries(blockchain_converter
  LINK_PRIVATE
    cryptonote_core
	p2p
	blockchain_db
    ${CMAKE_THREAD_LIBS_INIT})

add_dependencies(blockchain_converter
	version)
set_property(TARGET blockchain_converter
	PROPERTY
	OUTPUT_NAME "blockchain_converter")
endif ()

bitmonero_add_executable(blockchain_import
  ${blockchain_import_sources}
  ${blockchain_import_private_headers})

target_link_libraries(blockchain_import
  LINK_PRIVATE
    cryptonote_core
	blockchain_db
	p2p
    ${CMAKE_THREAD_LIBS_INIT})

add_dependencies(blockchain_import
	version)
set_property(TARGET blockchain_import
	PROPERTY
	OUTPUT_NAME "blockchain_import")

bitmonero_add_executable(blockchain_export
  ${blockchain_export_sources}
  ${blockchain_export_private_headers})

target_link_libraries(blockchain_export
  LINK_PRIVATE
    cryptonote_core
	p2p
	blockchain_db
    ${CMAKE_THREAD_LIBS_INIT})

add_dependencies(blockchain_export
	version)
set_property(TARGET blockchain_export
	PROPERTY
	OUTPUT_NAME "blockchain_export")
