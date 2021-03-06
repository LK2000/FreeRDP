# FreeRDP: A Remote Desktop Protocol Implementation
# FreeRDP cmake build script
#
# Copyright 2012 Laxmikant Rashinkar <LK.Rashinkar@gmail.com>
# Copyright 2012 Marc-Andre Moreau <marcandre.moreau@gmail.com>
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

set(RDPSND_MACAUDIO_SRCS
	rdpsnd_audio_q.c)

include_directories(..)
include_directories(${MACAUDIO_INCLUDE_DIRS})

add_library(rdpsnd_macaudio ${RDPSND_MACAUDIO_SRCS})
set_target_properties(rdpsnd_macaudio PROPERTIES PREFIX "")

if(MONOLITHIC_BUILD)
	target_link_libraries(rdpsnd_macaudio freerdp)
else()
	target_link_libraries(rdpsnd_macaudio freerdp-utils)
endif()

target_link_libraries(rdpsnd_macaudio ${MAC_AUDIOTOOLBOX_LIBRARY_PATH})
target_link_libraries(rdpsnd_macaudio ${MAC_COREFOUNDATION_LIBRARY_PATH})

install(TARGETS rdpsnd_macaudio DESTINATION ${FREERDP_PLUGIN_PATH})

