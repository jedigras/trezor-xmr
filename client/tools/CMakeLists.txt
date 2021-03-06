# Copyright (c) 2014-2016, The Monero Project
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

set(trezorctl_c_sources
	../terminal.c
  	../../shared/keccak/KeccakSponge.c
  	../../shared/keccak/KeccakNISTInterface.c
  	../../shared/keccak/KeccakF-1600-opt32.c
)

set(trezorctl_cpp_sources trezorctl.cpp)
set_source_files_properties(${trezorctl_c_sources} PROPERTIES COMPILE_FLAGS ${disabled_warnings_1})
set_source_files_properties(${trezorctl_cpp_sources} PROPERTIES COMPILE_FLAGS "-Wno-format -Wno-vla ${disabled_warnings_0}")
   
set(trezorctl_headers)
set(trezorctl_private_headers)
bitmonero_private_headers(trezorctl
	${trezorctl_private_headers})
bitmonero_add_executable(trezorctl
  	${trezorctl_cpp_sources}
  	${trezorctl_c_sources}
  	${trezorctl_headers}
  	${trezorctl_private_headers})

target_link_libraries(trezorctl
	LINK_PUBLIC
    	trezor
    	crypto
    	mnemonics
  	LINK_PRIVATE
    	${Boost_FILESYSTEM_LIBRARY}
    	${Boost_SYSTEM_LIBRARY}
    	${Boost_THREAD_LIBRARY}
    	${CMAKE_THREAD_LIBS_INIT}
)
